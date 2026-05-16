● Here's the full breakdown and evaluation of Murshid's approach from ticket #231230:

  ---
  Steps taken

  1. Disk replacement (repeated multiple times)
  - Alert → identify FAULTED disk → log with supplier as OOW → replace disk → start resilvering
  - Each replacement documented with serial numbers (old → new)
  - Ticket kept open with a checklist until rebuild finished

  2. Identify corrupted files using ll_decode_filter_fid
  This is the key technique — and it's different from what I suggested earlier. Since lfs fid2path alone doesn't always work (especially for older /O/0/ sequence format), Murshid:
  # Temporarily unmount the OST from Lustre and mount it as ZFS
  umount lustre/kl5-OST002c
  zfs set canmount=on lustre/kl5-OST002c
  zfs mount lustre/kl5-OST002c

  # Read the parent FID from the OST object's extended attributes
  ll_decode_filter_fid /lustre/kl5-OST002c/O/0/d27/10645563
  # → parent=[0x20000b2e1:0x32e8:0x0]

  # Remount as Lustre
  umount lustre/kl5-OST002c
  zfs set canmount=off lustre/kl5-OST002c
  mount -t lustre lustre/kl5-OST002c /lustre/kl5-OST002c

  3. Resolve parent FID to file path (on a Lustre client)
  lfs fid2path /kl5 [0x20000b2e1:0x32e8:0x0]
  # → /kl5/dug/kl/teamgenting/petronas/.../3dvolume.1

  4. Verify unreadable with dd
  dd if=/kl5/.../3dvolume.1 of=/dev/null bs=1024k
  # → "Invalid exchange" error = confirmed unreadable

  5. Delete corrupted files, scrub, then zpool clear
  - Files deleted from Lustre
  - zpool scrub lustre run
  - After all resilvering completed: zpool clear lustre
  - Pool back to ONLINE, 0 errors

  ---
  Evaluation

  What was done well:
  - Thorough documentation (serial numbers, timestamps, zpool output at each step)
  - ll_decode_filter_fid is the correct technique — more reliable than constructing FIDs from path components (which is why my earlier suggestion failed for you)
  - Verified files were unreadable with dd before deleting
  - Multiple simultaneous resilvers — same problem we discussed. Disks kept being replaced without waiting for the previous resilver to finish. This stretched the incident from March 21 to April 8 (~18 days).
  Each new replacement reset the resilver and added I/O contention.
  - A replacement disk itself failed mid-resilver (WMC1F1672749 died during slot 1 resilvering), leaving replacing-1 with 3 historical UNAVAIL entries — a very precarious state.
  - No mention of notifying the file owner until Murshid mentioned it in passing — the actual data was seismic project data belonging to user ijazn / project teamgenting/petronas.

  ---
  What this means for your current situation

  Your corrupted file lustre/kl5-OST002a:/O/ac0000400/d15/311983 uses a newer FID format (non-zero sequence), which is why lfs fid2path with a constructed FID failed. The correct approach is Murshid's
  ll_decode_filter_fid method:

  # On the OSS (kdat0036), after resilvering completes:
  umount lustre/kl5-OST002a
  zfs set canmount=on lustre/kl5-OST002a
  zfs mount lustre/kl5-OST002a

  ll_decode_filter_fid /lustre/kl5-OST002a/O/ac0000400/d15/311983
  # → will give you the parent FID

  umount lustre/kl5-OST002a
  zfs set canmount=off lustre/kl5-OST002a
  mount -t lustre lustre/kl5-OST002a /lustre/kl5-OST002a

  Then use the parent FID with lfs fid2path /kl5 [<parent_fid>] on a Lustre client.


**TRIAL**

```
[root@kpetronaslustre45 tmp]# sudo umount lustre/irfanfs-OST0001
[root@kpetronaslustre45 tmp]# sudo zfs set canmount=on lustre/irfanfs-OST0001
[root@kpetronaslustre45 tmp]# sudo zfs mount lustre/irfanfs-OST0001
[root@kpetronaslustre45 0]# sudo ll_decode_filter_fid /lustre/irfanfs-OST0001/O/0/d6/6
/lustre/irfanfs-OST0001/O/0/d6/6: parent=[0x200000401:0x6d:0x0] stripe=1 stripe_size=1048576 stripe_count=2 layout_version=0 range=0
[root@kpetronaslustre45 ~]# sudo umount lustre/irfanfs-OST0001
[root@kpetronaslustre45 ~]# sudo zfs set canmount=off lustre/irfanfs-OST0001
[root@kpetronaslustre45 ~]# sudo mount -t lustre lustre/irfanfs-OST0001 /lustre/irfanfs-OST0001
```