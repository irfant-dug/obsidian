[[lustre]]

## Preferred Method

* Get a list of disk that have been used by zpool lustre
```
[root@kpetronaslustre45 ~]# zpool status lustre | grep dm | awk '{print $1}' | tr "\n" "|" | sed 's/|$//'
dm-uuid-mpath-35000cca2c058cbf0|dm-uuid-mpath-35000cca2c05a5c90|dm-uuid-mpath-35000cca2c05b56a0|dm-uuid-mpath-35000cca2c05db5e4|dm-uuid-mpath-35000cca2c05e7334|dm-uuid-mpath-35000cca2c05e7670|dm-uuid-mpath-35000cca2c05e8d70|dm-uuid-mpath-35000cca2c05eaf64|dm-uuid-mpath-35000cca2c05eb420|dm-uuid-mpath-35000cca2c05ebea0|dm-uuid-mpath-35000cca2c05ec30c|dm-uuid-mpath-35000cca2c05ec494|dm-uuid-mpath-35000cca2c05ec4c0|dm-uuid-mpath-35000cca2c05ec744|dm-uuid-mpath-35000cca2c05eca18|dm-uuid-mpath-35000cca2c05eca94|dm-uuid-mpath-35000cca2c05eca98|dm-uuid-mpath-35000cca2c05ee258|dm-uuid-mpath-35000cca412975944|dm-uuid-mpath-35000cca4129864d8
```

* Compared to all disks to get disks that have not yet used
```
[root@kpetronaslustre45 ~]# ls -l /dev/disk/by-id/dm-uuid* | awk '{print $9}' | grep -vE "$(zpool status lustre | grep dm | awk '{print $1}' | tr "\n" "|" | sed 's/|$//')" 
/dev/disk/by-id/dm-uuid-mpath-35000cca2c0594cfc
/dev/disk/by-id/dm-uuid-mpath-35000cca2c05962cc
/dev/disk/by-id/dm-uuid-mpath-35000cca2c05b5558
/dev/disk/by-id/dm-uuid-mpath-35000cca2c05da714
/dev/disk/by-id/dm-uuid-mpath-35000cca2c05da830
/dev/disk/by-id/dm-uuid-mpath-35000cca2c05dafd0
/dev/disk/by-id/dm-uuid-mpath-35000cca2c05dcbb0
/dev/disk/by-id/dm-uuid-mpath-35000cca2c05dfe38
/dev/disk/by-id/dm-uuid-mpath-35000cca2c05e6ba0
/dev/disk/by-id/dm-uuid-mpath-35000cca2c05e94e4
/dev/disk/by-id/dm-uuid-mpath-35000cca2c05e9ff4
/dev/disk/by-id/dm-uuid-mpath-35000cca2c05ea2cc
/dev/disk/by-id/dm-uuid-mpath-35000cca2c05eb3d8
/dev/disk/by-id/dm-uuid-mpath-35000cca2c05eb528
/dev/disk/by-id/dm-uuid-mpath-35000cca2c05ebf30
/dev/disk/by-id/dm-uuid-mpath-35000cca2c05ec1bc
/dev/disk/by-id/dm-uuid-mpath-35000cca2c05ec268
/dev/disk/by-id/dm-uuid-mpath-35000cca2c05ec464
/dev/disk/by-id/dm-uuid-mpath-35000cca2c05ec654
/dev/disk/by-id/dm-uuid-mpath-35000cca2c05eca44
/dev/disk/by-id/dm-uuid-mpath-35000cca2c05eca84
/dev/disk/by-id/dm-uuid-mpath-35000cca2c05ede78
/dev/disk/by-id/dm-uuid-mpath-35000cca2c05edf90
/dev/disk/by-id/dm-uuid-mpath-35000cca4129802e0

```

* Change format from newline to space
```
[root@kpetronaslustre45 ~]# ls -l /dev/disk/by-id/dm-uuid* | awk '{print $9}' | grep -vE "$(zpool status lustre | grep dm | awk '{print $1}' | tr "\n" "|" | sed 's/|$//')" | tr "\n" " "
/dev/disk/by-id/dm-uuid-mpath-35000cca2c0594cfc /dev/disk/by-id/dm-uuid-mpath-35000cca2c05962cc /dev/disk/by-id/dm-uuid-mpath-35000cca2c05b5558 /dev/disk/by-id/dm-uuid-mpath-35000cca2c05da714 /dev/disk/by-id/dm-uuid-mpath-35000cca2c05da830 /dev/disk/by-id/dm-uuid-mpath-35000cca2c05dafd0 /dev/disk/by-id/dm-uuid-mpath-35000cca2c05dcbb0 /dev/disk/by-id/dm-uuid-mpath-35000cca2c05dfe38 /dev/disk/by-id/dm-uuid-mpath-35000cca2c05e6ba0 /dev/disk/by-id/dm-uuid-mpath-35000cca2c05e94e4 /dev/disk/by-id/dm-uuid-mpath-35000cca2c05e9ff4 /dev/disk/by-id/dm-uuid-mpath-35000cca2c05ea2cc /dev/disk/by-id/dm-uuid-mpath-35000cca2c05eb3d8 /dev/disk/by-id/dm-uuid-mpath-35000cca2c05eb528 /dev/disk/by-id/dm-uuid-mpath-35000cca2c05ebf30 /dev/disk/by-id/dm-uuid-mpath-35000cca2c05ec1bc /dev/disk/by-id/dm-uuid-mpath-35000cca2c05ec268 /dev/disk/by-id/dm-uuid-mpath-35000cca2c05ec464 /dev/disk/by-id/dm-uuid-mpath-35000cca2c05ec654 /dev/disk/by-id/dm-uuid-mpath-35000cca2c05eca44 /dev/disk/by-id/dm-uuid-mpath-35000cca2c05eca84 /dev/disk/by-id/dm-uuid-mpath-35000cca2c05ede78 /dev/disk/by-id/dm-uuid-mpath-35000cca2c05edf90 /dev/disk/by-id/dm-uuid-mpath-35000cca4129802e0
```


```
[root@kpetronaslustre45 tmp]# sh query.sh e45 | grep dm- | tr "\n" " " 
dm-uuid-mpath-35000cca2c0594cfc dm-uuid-mpath-35000cca2c05b5558 dm-uuid-mpath-35000cca2c05da714 dm-uuid-mpath-35000cca2c05da830 dm-uuid-mpath-35000cca2c05dafd0 dm-uuid-mpath-35000cca2c05e6ba0 dm-uuid-mpath-35000cca2c05e94e4 dm-uuid-mpath-35000cca2c05e9ff4 dm-uuid-mpath-35000cca2c05ea2cc dm-uuid-mpath-35000cca2c05eb3d8 dm-uuid-mpath-35000cca2c05eb528 dm-uuid-mpath-35000cca2c05ebf30 dm-uuid-mpath-35000cca2c05ec1bc dm-uuid-mpath-35000cca2c05ec268 dm-uuid-mpath-35000cca2c05ec464 dm-uuid-mpath-35000cca2c05eca44 dm-uuid-mpath-35000cca2c05eca84 dm-uuid-mpath-35000cca2c05ede78 dm-uuid-mpath-35000cca2c05edf90 dm-uuid-mpath-35000cca4129802e0
```
```
[root@kpetronaslustre45 tmp]# sh query.sh c0/e24 | grep -B 4 -E "s5|s11|s17|s23" | grep dm | tr "\n" " "
dm-uuid-mpath-35000cca2c05962cc dm-uuid-mpath-35000cca2c05dcbb0 dm-uuid-mpath-35000cca2c05dfe38 dm-uuid-mpath-35000cca2c05ec654
```
```
[root@kpetronaslustre45 tmp]# sh query.sh e24 | grep dm- | grep -vE "dm-uuid-mpath-35000cca2c05962cc|dm-uuid-mpath-35000cca2c05dcbb0|dm-uuid-mpath-35000cca2c05dfe38|dm-uuid-mpath-35000cca2c05ec654" | tr "\n" " "
dm-uuid-mpath-35000cca2c058cbf0 dm-uuid-mpath-35000cca2c05a5c90 dm-uuid-mpath-35000cca2c05b56a0 dm-uuid-mpath-35000cca2c05db5e4 dm-uuid-mpath-35000cca2c05e7334 dm-uuid-mpath-35000cca2c05e7670 dm-uuid-mpath-35000cca2c05e8d70 dm-uuid-mpath-35000cca2c05eaf64 dm-uuid-mpath-35000cca2c05eb420 dm-uuid-mpath-35000cca2c05ebea0 dm-uuid-mpath-35000cca2c05ec30c dm-uuid-mpath-35000cca2c05ec494 dm-uuid-mpath-35000cca2c05ec4c0 dm-uuid-mpath-35000cca2c05ec744 dm-uuid-mpath-35000cca2c05eca18 dm-uuid-mpath-35000cca2c05eca94 dm-uuid-mpath-35000cca2c05eca98 dm-uuid-mpath-35000cca2c05ee258 dm-uuid-mpath-35000cca412975944 dm-uuid-mpath-35000cca4129864d8
```

```
[root@kpetronaslustre45 tmp]# zpool status lustre | grep dm-uuid | while read line; do sh query.sh $line | grep -i drive | head -1; done 2>&1 | sort -V 
Drive /c0/e24/s0
Drive /c0/e24/s1
Drive /c0/e24/s2
Drive /c0/e24/s3
Drive /c0/e24/s4
Drive /c0/e24/s6
Drive /c0/e24/s7
Drive /c0/e24/s8
Drive /c0/e24/s9
Drive /c0/e24/s10
Drive /c0/e24/s12
Drive /c0/e24/s13
Drive /c0/e24/s14
Drive /c0/e24/s15
Drive /c0/e24/s16
Drive /c0/e24/s18
Drive /c0/e24/s19
Drive /c0/e24/s20
Drive /c0/e24/s21
Drive /c0/e24/s22
```

```
[root@kpetronaslustre45 tmp]# zpool status lustre_2 | grep dm-uuid | while read line; do sh query.sh $line | grep -i drive | head -1; done 2>&1 | sort -V 
Drive /c0/e24/s5
Drive /c0/e24/s11
Drive /c0/e24/s17
Drive /c0/e24/s23
Drive /c0/e45/s0
Drive /c0/e45/s1
Drive /c0/e45/s2
Drive /c0/e45/s3
Drive /c0/e45/s4
Drive /c0/e45/s5
Drive /c0/e45/s6
Drive /c0/e45/s7
Drive /c0/e45/s8
Drive /c0/e45/s9
Drive /c0/e45/s10
Drive /c0/e45/s11
Drive /c0/e45/s12
Drive /c0/e45/s13
Drive /c0/e45/s14
Drive /c0/e45/s15
Drive /c0/e45/s16
Drive /c0/e45/s17
Drive /c0/e45/s18
Drive /c0/e45/s19
```