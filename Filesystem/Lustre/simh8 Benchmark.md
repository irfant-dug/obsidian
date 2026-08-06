[[lustre]]

* RAIDz3 12-disks
```
zpool create -f -o ashift=12 -O overlay=on -O atime=off -O recordsize=1M -O compression=lz4 -O dnodesize=auto lustre raidz3 /dev/sdc /dev/sde /dev/sdg /dev/sdi /dev/sday /dev/sdba /dev/sdbc /dev/sdbe /dev/sdbo /dev/sdbq /dev/sdbs /dev/sdbu
```


* RAIDz2 12-disks
```
zpool create -f -o ashift=12 -O overlay=on -O atime=off -O recordsize=1M -O compression=lz4 -O dnodesize=auto lustre raidz2 /dev/sdc /dev/sde /dev/sdg /dev/sdi /dev/sday /dev/sdba /dev/sdbc /dev/sdbe /dev/sdbo /dev/sdbq /dev/sdbs /dev/sdbu
```

* RAIDz2 11-disks
```
zpool create -f -o ashift=12 -O overlay=on -O atime=off -O recordsize=1M -O compression=lz4 -O dnodesize=auto lustre raidz2 /dev/sdc /dev/sde /dev/sdg /dev/sdi /dev/sday /dev/sdba /dev/sdbc /dev/sdbe /dev/sdbo /dev/sdbq /dev/sdbs
```