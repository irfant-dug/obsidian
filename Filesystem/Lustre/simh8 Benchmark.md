[[lustre]]

MDT Pool
```
zpool create -f -o ashift=12 -O overlay=on -O atime=off -O dnodesize=auto lustre.mdt /dev/sdcc /dev/sdcd /dev/sdce /dev/sdcf /dev/sdcg
```
```
z
```
1. RAIDz3 12-disks
```
zpool create -f -o ashift=12 -O overlay=on -O atime=off -O recordsize=1M -O compression=lz4 -O dnodesize=auto lustre raidz3 /dev/sdc /dev/sde /dev/sdg /dev/sdi /dev/sday /dev/sdba /dev/sdbc /dev/sdbe /dev/sdbo /dev/sdbq /dev/sdbs /dev/sdbu
```

2. RAIDz2 12-disks
```
zpool create -f -o ashift=12 -O overlay=on -O atime=off -O recordsize=1M -O compression=lz4 -O dnodesize=auto lustre raidz2 /dev/sdc /dev/sde /dev/sdg /dev/sdi /dev/sday /dev/sdba /dev/sdbc /dev/sdbe /dev/sdbo /dev/sdbq /dev/sdbs /dev/sdbu
```

* 4k show only 3 disks utilize
* sdbs show lower util during 1M file write. Probably an always skip sector. Reading is fine
* 


3. RAIDz2 11-disks
```
zpool create -f -o ashift=12 -O overlay=on -O atime=off -O recordsize=1M -O compression=lz4 -O dnodesize=auto lustre raidz2 /dev/sdc /dev/sde /dev/sdg /dev/sdi /dev/sday /dev/sdba /dev/sdbc /dev/sdbe /dev/sdbo /dev/sdbq /dev/sdbs
```
* 1M sequential read shows very hot first and last disk. Why the fuck


```
- 4k
for i in $(seq 1 10240); do dd if=/dev/urandom of=4k.$i bs=4k count=1 ; done; while true; do sudo sync; echo 3 | sudo tee /proc/sys/vm/drop_caches; for i in $(seq 1 10240); do dd if=4k.$i of=/dev/null; done; done;

- 1M Seq
	
```