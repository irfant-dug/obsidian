Exclusive to RAIDz3
* For 4K record file, 3 parity block written per 1 data block
* For ~1M record file, 3 parity block written per data stripe. For the same record, parity block will always land on the same 3 disks
* A record must be in divisible by (n-parity + 1 - = 4 blocks) to ensure that the space is reclaimable
* The smallest readable unit for a file is record. You want to read a portion of a file, you have to read the entire record. Checksum is calculated per record
* A sequential read just mean that the disk is reading records which reside in sequential address of each other. In one disks swoop, the disk is reading a huge number of sectors
* A "normal" read is where the disk have to change spindle position fulfil IO request, but the jump is not too big that the latency is low-to-average
* A random read is where the spindle have to jump far to reach certain sectors, which will result in higher latency
* When IO is diverse, a seemingly sequential IO (job is reading a large file compared to many random small file) will result in random IO. Queue is comprise of diverse IO from different jobs that need to be fulfill
* We don't have to really care about write IO. Disk will write to any spot closest to the spindle, which not gonna result in higher latency
* 