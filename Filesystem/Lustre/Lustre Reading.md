1. [Tutorial: How to install, tune and Monitor a ZFS based Lustre file system](https://wiki.lustre.org/images/1/13/LustreOnZfs.pdf)
* LLNL prefer a single pool with multiple VDEV as its OSTs.
* JBOD is connected to 2 different OSSes, but each of them only mount 1 30-disks raidz2 pool at a time. Provide OSSes redundancy using HA
* Snapshot can be taken by stopping Lustre services, import pool, and take snapshot
* Monitoring: `arcstat.py` and `cat /proc/spl/kstat/zfs/lustre/txgs`
* Metrics: `/proc/slabinfo$` and `/proc/spl/kstat/zfs/arcstats`
### Tuning
*  zfs_prefetch_disable=1
	* Doesn't fit Lustre workload. Caused IO waste
* metaslab_debug_unload = 1 for OSS, = 0 for MDS
* zfs_txg_history=120
	* For logging purpose. To know how long it took to sync to disk

2. https://wiki.lustre.org/OBDFilter_Survey
* OBDFilter-Survey tests the performance of one or more OSTs by simulating Lustre client IO. Each OSS server in an installation is tested individually.
* The `network` and `netdisk` modes are not normally used for benchmarking as they may produce unreliable results. Only `disks` test is reliable
* Tried using obdfilter-survey but they are abysmally slow

2. [Lustre Benchmarking Tips And Tricks](https://wiki.lustre.org/images/4/40/Wednesday_shpc-2009-benchmarking.pdf)
### Tuning
* max_sectors_kb=max_hw_sectors_kb
* Deadline scheduler

3. [SGPDD Survey](https://wiki.lustre.org/SGPDD_Survey)
* Part of lustre io_kit. Send scsi command to scsi disk, bypassing kernel and filsystem. Most accurate way to benchmark disk througput
```
crglo=1 crghi=256 \
thrlo=1 thrhi=4096 \
size=51200 \
rslt_loc=/var/tmp/sgpdd-survey_out \
scsidevs=$(zpool status lustre -Lv | grep -Eo "dm-[0-9]+"  | while read line; do sudo multipath -l | grep $line -A 5 | grep -Eo "sd[a-z]+" | head -1; done | sed 's/^/kpetronaslustre45:\/dev\//' | tr '\n' ' ' | sed 's/ $/\n/') \
sgpdd-survey
```