[[ZFS]]
In raidz3 pool with 20 disks

* Total disks = 20
* Parity disks = 3
* Data disks = 17
* Disk speed = 250 MB/s
* Disk IO = 220 IOPS
```
Aggregate Throughput = 17 data disks × 250 MB/s = 4,250 MB/s.

Aggeregate IO = 1 x 220 IOPS = 220 IOPS (raidz3 have IOPS of a single disk)
```

Assuming all IOPS is 1MB read/write
```
Actual Speed = 220 x 1MB = 220MBps = 1.76Gbps
```

* In actual practice, ZFS caches incoming writes in RAM and aggregates them into large Transaction Groups (TXGs).