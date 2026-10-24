1. https://downunder.zendesk.com/agent/tickets/370767 - Job fail - tomosolve node/connection issues
2. https://downunder.zendesk.com/agent/tickets/370845 - Houston tank 5-1 (Rocky 9.6 KNL) kernel panics in vastnfs client "dev" build - please rebuild image with latest vastnfs 4.5
3. https://downunder.zendesk.com/agent/tickets/370789 - disk I/O error
```
What ZD#370789 is

Andrey Lima is running a 3D binning workflow in Insight. The data is on /data/h8 (project ricocuPr_099).

On 22 Oct, array task 47809148_72 ran for 86 minutes on hnod7-10-7. It died at 04:54 CDT with exit 255, because Insight couldn't save an update to the volume's index file. That file is a small SQLite database, 000scratch/volsTemp/.../72.dugio/3D-3dvolume.single.idx.

The task went into SE (held). Jack asked Andrey to leave it there, but it was cancelled before anyone released it. The ticket is open and unassigned.

What the error means.
- error code 10:778 is SQLite's "write failed" code: a write() to that file got an error back from the filesystem. SQLite and Insight didn't cause it.
- The transaction had been stuck for 41 s before it failed. Insight's progress table shows the writer freezing at about 04:53:15: queue full at 272/272, only 22 records processed in 30 s.
- "If this database is stored on a network disk it may not be sufficiently reliable" is a stock hint Insight adds to this kind of error (SimpleDBUtils.rethrow in the stack trace). It isn't evidence of anything. That's the "shade" Dan noticed.

What Jack's client log says. The log is from the job's node.
- Total 8 stale ZC_REQs for peer_ni 10.2.20.5@tcp: the node sent write data to the storage server h8dat2-20-5 (OST0036). That server never acknowledged 8 of the sends in time, so the node dropped the connection.
- client_bulk_callback() event type 1, status -5 (×8): these are the server's attempts to pull the node's write data. All 8 failed with an I/O error.
- Health = 900: Lustre's network layer marked the node's own interface as having had a failure.
- o4->h8-OST0036 ... failed due to network error: o4 is a write request. It failed, the node reconnected, and it sent the request again.
- The retry: timed out for slow reply plus Unexpected # bytes transferred: 0 (requested 10158080). The server answered, but none of the 9.7 MiB of data moved on the retry, so the node rejected the answer and failed the write.

The main correction: the times match

Jack wrote that the client log shows nothing at the time of failure. The log looks that way because dmesg -T on hnod7-10-7 is 4 min 38 s fast.

The Lustre lines also record times from the real clock. The "timed out" line is printed at the moment its deadline (dl) passes, so that gives the exact offset:

┌──────────────────────────────────────────┬────────────────┬────────────────────────────┐
│                  Event                   │ dmesg -T shows │      Real time (CDT)       │
├──────────────────────────────────────────┼────────────────┼────────────────────────────┤
│ Write sent to OST0036                    │ –              │ 04:52:49 (sent 1792662769) │
├──────────────────────────────────────────┼────────────────┼────────────────────────────┤
│ Unacknowledged sends, connection dropped │ 04:57:31       │ ≈04:52:53                  │
├──────────────────────────────────────────┼────────────────┼────────────────────────────┤
│ Write sent again                         │ –              │ 04:52:54 (sent 1792662774) │
├──────────────────────────────────────────┼────────────────┼────────────────────────────┤
│ Insight's writer stalls                  │ –              │ ≈04:53:15                  │
├──────────────────────────────────────────┼────────────────┼────────────────────────────┤
│ Retry times out, "0 bytes transferred"   │ 04:58:35       │ 04:53:57 (dl 1792662837)   │
├──────────────────────────────────────────┼────────────────┼────────────────────────────┤
│ SQLite save fails with 778 (job log)     │ –              │ 04:53:57                   │
├──────────────────────────────────────────┼────────────────┼────────────────────────────┤
│ Task exits 255 and goes into SE          │ –              │ 04:54:43                   │
└──────────────────────────────────────────┴────────────────┴────────────────────────────┘

The Lustre write failure and the SQLite error happen in the same second.

There are two other slips in the internal notes:
- Jack's note says hnod7-19-7, but his shell prompt says hnod7-10-7. The job log confirms HOST=hnod7-10-7, so his client log is from the right node.
- He searched h8dat2-20-5's log for 10.7.19.7, which is the wrong IP. The node's address is 10.7.10.7 (it's in the log line itself). So "no logs on the server" proves nothing.

How it relates to #370765

Dan's guess is right. #370765 is the same user, the same project and the earlier stage of the same workflow.
- On 21 Oct the setup job 47783059 ran on hnod7-23-30. From 18:24 CDT its file flushes (fdatasync) were slow, often taking minutes and up to 8 minutes at worst. At 19:41:44 one returned an I/O error and the job died.
- Andrey then resubmitted. Task 72 of the next stage became this ticket.
- Shawn's log excerpt on #370765 is from 7–8 hours after that failure. It shows the node losing other storage targets (OST002d and OST0020) later, not the failure itself.

Both tickets look the same: a compute node's writes to /data/h8 stall, then fail. Camille was right that it isn't memory, and it isn't Insight either. Three different storage servers show up across the two nodes, which hints it isn't one bad server, but that's weak evidence.

Still unknown, most useful checks first

1. The server's side. Search h8dat2-20-5's kernel log for 10.7.10.7 between 04:52 and 04:54 CDT. Use journalctl -k, since that server's dmesg -T may be skewed too.
2. How widespread. Did other nodes log h8-OST0036 or 10.2.20.5 errors in that window? If many did, the server is the problem. If only hnod7-10-7 did, it's the node or its network path.
3. The node's network state. On hnod7-10-7, run lnetctl peer show --nid 10.2.20.5@tcp -v and lnetctl net show -v. This state usually survives the automatic unmount, so Jack may be wrong that little more can be learned. It also rules out the IT-7603 Multi-Rail mis-pairing seen on h8dat2-20-x servers, although that problem looked different (floods of 137-5 and ENOTCONN errors, which aren't here).
4. Which storage target held the file. If the .idx file still exists, run lfs getstripe -i on it. I'd expect 54 (OST0036). That would show exactly how the failed write reached SQLite, which isn't proven yet.

There's nothing left to release for Andrey: task 72 was cancelled, so he needs to resubmit it.

I can draft an internal note for Jack with the corrected timeline and the right IP. I won't post it without your OK.

[0] https://downunder.zendesk.com/agent/tickets/370789
[1] https://downunder.zendesk.com/agent/tickets/370765

```