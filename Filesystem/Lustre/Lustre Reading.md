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

2. 