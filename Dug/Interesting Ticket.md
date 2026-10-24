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
```
Short answer: not disk, memory or Insight. A Lustre client timeout fired too early, and then a resend race turned a few-second network hiccup into a write error. The OSS actually wrote and committed the data that Lustre reported as failed.

hnod7-10-7 is a CentOS 7 KNL running the Lustre 2.12.4 client. One 9.7 MiB write to h8-OST0036 (h8dat2-20-5) went about 5 s without the OSS acknowledging it at the network-driver level, and 2.12 treats that as a dead connection. What made the OSS that slow is still open.

What happened (CDT)

Times below are corrected. The node's dmesg -T runs about 4m37s–4m38s fast, which I calibrated from the RPC's own sent/dl fields.

- 04:52:49 The write RPC x1876984172925504 goes to OST0036. LNet sends its 10,158,080 bytes as 10 chunks of 1 MiB.
- 04:52:53–54 The client logs Total 8 stale ZC_REQs for peer_ni 10.2.20.5@tcp ... resid: 0, wmem: 0 and closes every connection to that OSS. All 8 client_bulk_callback ... status -5 lines carry the same desc ffff93ca6d129400. So these are 8 chunks of this one write, abandoned 4–5 s after it was sent.
- 04:52:54 The client resends the write, and "Connection restored" follows immediately.
- 04:53:16 The SQLite COMMIT on the .idx starts, and Insight's writer stalls at 124.0 GB.
- 04:53:57 The RPC hits its 63 s deadline with flags Bulk:RXM and transno 17274977911, meaning the OSS replied "success, committed". The client still logs Unexpected # bytes transferred: 0. SQLite fails with 778 in the same second, and the task exits 255 at 04:54:43.

Why a hiccup killed the job

I checked both of these in the Lustre source.

1. 2.12 clients give up after 5 s.
   - lnet_lnd_timeout starts at 5 s (2.12.4 api-ni.c:223). It is only recalculated if lnet_transaction_timeout is set to a non-default value or lnet_retry_count is set.
   - Upstream fixed this in LU-13510, but only from 2.14. It is still 5 s in 2.12.9, while 2.15.5 waits 16 s.
   - When the timer fires, the client closes every socket to that OSS.
   - I found no LNet options in /cluster/templates/{hknl7,global} or 13-tuning.sh. I couldn't see the compute7.9 image itself, but the 4–5 s give-up in the log fits 5 s, not 16 or 50.
   - wmem: 0 only means TCP acknowledged the data. With tcp_rmem max at 128 MiB, the OSS may not have read it yet.
2. The resend lost a race and returned -EPROTO.
   - Every resend gets new bulk match bits (2.12.4 client.c:3274-3290). 2.15.5 clients do the same.
   - A 2.15.5 OSS still working on the original drops a resend with the same xid and sends the original's reply instead (service.c:1831-1869). It logs this only at debug level, so nothing appears on the console.
   - The client got a success reply, but the OSS never pulled its new bulk buffers. At the deadline it counted 0 bytes and returned -EPROTO.
   - osc doesn't retry -EPROTO; it only retries EIO, EROFS, ENOMEM, EAGAIN and EINPROGRESS. Upstream describes the same "will never get the bulk" outcome in LU-6780.
   - Houston 2.12 clients also have max_dirty_mb=32 set by 13-tuning.sh, so one stuck 10 MiB RPC blocks further writes to that OST. That fits the 41 s SQLite stall.

Still open: why h8dat2-20-5 was more than 5 s late

There are three candidates:
- OSS backlog. hnod5-1-41 also logged slow-reply timeouts from OST0036 on 10-11 to 10-14, and h8's switches are being replaced tomorrow (ZD#370855).
- Packet loss on the way back to the client. hnod7-10-7 shares one 50G multi-host NIC with hnod7-10-5, -6 and -8 (leaf switch hsb2-slf-7-10).
- The KNL itself was too busy to process the acknowledgements in time.

ZD#370765 hit different OSSes from another KNL, so I lean towards a hair-trigger client plus general h8 load, not one bad server. I haven't verified that. These checks, run on a Houston host, would narrow it down:

- OSS log: grep '2026-10-22T04:5[0-5]' /node/logs/h8dat2-20-5.log | grep -E '10\.7\.10\.7|bulk|ZC_REQ|inactive'. Jack grepped for 10.7.19.7, the wrong node, so the OSS log hasn't really been checked.
- Other clients in the same window: grep -l 'stale ZC_REQs for peer_ni 10.2.20.5' /node/logs/hnod*.log.
  - Many clients points to the OSS.
  - Only the four nodes sharing the NIC points to the NIC or switch port.
  - Only hnod7-10-7 points to the client itself.
- Which OST holds the file: lfs getstripe -i on the .idx. I expect 54 (0x36).
- Is the 5 s timeout live? On any hknl7 node, read /sys/module/lnet/parameters/lnet_{transaction_timeout,retry_count} and check for lnet options in /etc/modprobe.d and /etc/lnet.conf. A reading of 50/2 with nothing set means 5 s is in effect.

What it isn't

- Not memory or Insight. Insight's "network disk may not be sufficiently reliable" message is generic.
- Not the Multi-Rail mis-pairing, since there are no 137-5 or ENOTCONN errors.
- Not a dead link: TCP delivered everything and the reconnect was instant.

ZD#370765 is probably the same problem, but I haven't checked it.

Fix options

Both of these change the whole fleet, so they need testing first.

- Set lnet_retry_count explicitly on the KNLs. That makes 2.12 recalculate the timeout to 50/2 = 25 s. Setting lnet_transaction_timeout=50 alone does nothing, because the code skips a value that hasn't changed.
- Move the KNLs to 2.15.x. Jacek argues for this in ZD#370877 for a separate crash.

Either option makes the connection teardown much rarer. Neither removes the resend race.

Want me to draft this as an internal note on #370789?

References
- [0] https://downunder.zendesk.com/agent/tickets/370789
- [1] https://downunder.zendesk.com/agent/tickets/370765
- [2] https://downunder.zendesk.com/agent/tickets/370855
- [3] https://downunder.zendesk.com/agent/tickets/370877
- [4] LU-13510 fix: https://github.com/lustre/lustre-release/commit/0127d64b8c
- [5] LU-6780 patch: https://lkml.iu.edu/hypermail/linux/kernel/1610.3/04043.html
- [6] 2.12.4 source: https://github.com/lustre/lustre-release/blob/2.12.4/lnet/lnet/api-ni.c#L223
- [7] 2.15.5 source: https://github.com/lustre/lustre-release/blob/2.15.5/lustre/ptlrpc/service.c#L1831

```

4. https://downunder.zendesk.com/agent/tickets/369244 - Job fails at 100% complete with "marked as finished" error
```
 https://downunder.zendesk.com/agent/tickets/369244

  Help me give a reasoning. I can provide you with more log if needed

● Skill(zendesk)
Successfully loaded skill

  Ran 5 shell commands

Found that the job died on hnod2-3-19 (Houston) at 07:48 CDT on 09-29, but the tailed log only shows the end of the run. I'll now look for the full job log to see what led up to the fdatasync: Bad file descriptor error.

  Listed 2 directories, ran 3 shell commands

/h7 isn't accessible from it-kl, so I can't pull the full job log directly. I'm checking the docs to figure out whether /h7 is Lustre or VAST, since that affects how to interpret the "Bad file descriptor" error.

● Skill(cluster)
Successfully loaded skill

  Read 1 file

● Skill(filesystem)
Successfully loaded skill

  Ran 2 shell commands

Still investigating—the "Bad file descriptor" error likely stems from the job's shutdown rather than causing it. I'll pull hnod2-3-19's syslog around 07:48 CDT on 09-29 to verify this theory.

  Read 1 file, listed 3 directories, ran 11 shell commands

● Skill(github)
Successfully loaded skill

  Searched for 1 pattern, ran 11 shell commands

Found the key evidence. I'm saving a short memory note for this ticket, then I'll write up the reasoning.

  Ran 3 shell commands, wrote 3 memories

The logs don't support "transient filesystem issue". Bad file descriptor (9) is a side effect of the job shutting itself down, not the cause. What actually failed was inside Insight's workflow engine, in memory. I'd push back on Neil's "Yep" and send it back to SW.

Reasoning

1. Storage wasn't holding anything up. Every queue in the final status tables is either empty (0/272, 0/1904) or finished. All eight writers (A–H) report 11.9 MB written, and the log says only 4 s were spent waiting for work over the 46-minute run. A storage stall looks different: a long hang, then an EIO-type error. Compare ZD#366006 (h8, hung for ages, then failed on the write timeout) and ZD#366185 (h8, Protocol error (71)).
2. The failure was a hand-off between two in-memory queues. The log has two status tables, both stamped 07:48:14:
   - In the first, sector 5 (the 17.x rows) is still on its last record: step 17.14 is active at 2,828/2,829.
   - In the second, sector 5 has reached 2,829, but the full-azimuth Join (20.2J, "sum 6 az stacks") stays at 2,828/2,829, and so does Writer H.

   So that last record never reached the Join. The fatal error, Queue 17.16-17.17 …-tee has been marked as finished, is that record being put into a queue that had already been closed. The CallSiteException shows the close was called from the queue class's own background task (MultiThreadQueueInterruptible$2$1:476), not from any I/O code. Only SW can say which came first: a race in the queue code that triggered the shutdown, or the shutdown closing the queues under the in-flight record. Either way, no disk I/O is involved.
3. The shutdown began with an InterruptedException, not an I/O error. The message was "operation interrupted / cancelled". In Java that comes from one thread interrupting another. A failing filesystem shows up as an I/O error with an errno.
4. EBADF is not a storage error code. fdatasync() returns errno 9 when the file descriptor is no longer open in the process. The kernel checks that before it calls the filesystem at all. /h7 is Houston VAST over NFS, and NFSv3 never turns a server problem into EBADF. In the log, the EBADF sits under close() failed - suppressing exception, after [WorkflowTreeRunner] An error occurred, shutting down…. The cleanup tried to sync a file the abort had already closed.
5. This has happened before. ZD#239046 (2022, also /h7) had the same -tee has been marked as finished and Found no work 100 times in a row! messages, and also passed on requeue. That warning also shows up in healthy runs: ZD#170715 logged it 273 times in one task. "Requeue worked" fits a timing race just as well as a filesystem blip.

What I couldn't check

/h7 isn't mounted on it-kl and my sandbox blocks ssh. Two checks on hnod2-3-19 would close out the filesystem theory:
- NFS version. If /h7 is NFSv4.x rather than v3, NFS can return EBADF after a failed state recovery, and that would show in the kernel log. The NFS data mounts on it-kl are vers=3, but I can't see Houston's.
- Kernel log for 07:30–07:55 CDT on 09-29. Look for nfs: server … not responding, path errors or hung tasks.

You can run both here so the output lands in this session:
! ssh hnod2-3-19 "findmnt -T /h7 -o SOURCE,FSTYPE,OPTIONS; journalctl -k --since '2026-09-29 07:30' --until '2026-09-29 07:55' | grep -iE 'nfs|vast|rpc|hung|link'"
More logs that would help SW:
- the full .o45077028.3725, especially the queue dump printed right after Found no work 100 times and the full InterruptedException stack
- Mihnea's other recent failed job/task IDs. If they show the same pattern (Join one record short, "marked as finished") on different nodes and times, that settles it.

Draft internal note (not posted)

▎ IT review: we don't see evidence of a storage fault. The logs point to the workflow engine.
▎
▎ 1. Storage wasn't a bottleneck. Every queue is empty or finished, writers A–H each wrote 11.9 MB, and only 4 s were spent waiting for work over the run.
▎ 2. The last record of sector 5 (the slowest branch) was in flight when the queues were closed. The full-azimuth Join 20.2J ended at 2,828/2,829, and so did Writer H. The fatal error is a put() into the sector-5 tee after finish(), which was called from MultiThreadQueueInterruptible$2$1:476. The shutdown itself began with an InterruptedException ("operation interrupted / cancelled"), not an I/O error.
▎ 3. "fdatasync: Bad file descriptor (9)" is not a storage error. EBADF means the descriptor had already been closed in the process, and the kernel rejects it before the filesystem is called. NFS/VAST problems show up as EIO, ESTALE or ETIMEDOUT (compare ZD#366185, "Protocol error (71)"). Here it appears under "close() failed - suppressing exception", after the shutdown had begun, so it is fallout from the abort.
▎ 4. ZD#239046 (2022, /h7) had the same signature and was also fixed by requeue.
▎
▎ SW: could you look at the tee/Join end-of-stream handling in 6.1 (609180)? Mihnea: please send the job/task IDs of the other recent failures so we can check whether they match.

Zendesk comments can't be edited, so I haven't posted this. If the node check comes back clean, I'll add a line saying so first. I also saved a memory note on this ticket.

[0] https://downunder.zendesk.com/agent/tickets/369244
[1] https://downunder.zendesk.com/agent/tickets/239046
[2] https://downunder.zendesk.com/agent/tickets/366185
[3] https://downunder.zendesk.com/agent/tickets/366006
[4] https://downunder.zendesk.com/agent/tickets/170715

```