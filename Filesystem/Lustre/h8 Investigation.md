## Collecting Logs

```
#run in it-houston

eval $(ssh-agent); ssh-add
pdsh -w "it-kl" 'mkdir -p ~/stuff/h8/h8_temp';
pdsh -g "filesystem=h8" '/bin/unbuffer /bin/iostat -xmd 1 10 | tee -a ${HOSTNAME}_iostat_$(date +%Y%m%d_%H%M) & /usr/sbin/zpool iostat -r 1 10 | tee -a ${HOSTNAME}_zpool_iostat_r_$(date +%Y%m%d_%H%M) & /usr/sbin/zpool iostat -v 1 10 | tee -a ${HOSTNAME}_zpool_iostat_v_$(date +%Y%m%d_%H%M) & /usr/sbin/zpool iostat -vr 1 10 | tee -a ${HOSTNAME}_zpool_iostat_vr_$(date +%Y%m%d_%H%M); /bin/rsync -av -e "ssh -o StrictHostKeyChecking=no" ~/* adm_irfant@it-kl:~/stuff/h8/h8_temp; rm * -f; '; 
pdsh -w "it-kl" 'ls -1 ~/stuff/h8/h8_temp/ | head -1 | awk -F '_' "{print \$(NF-1),\$NF}" | while read line; do echo $line | tr " " "_" | xargs -I {} mv ~/stuff/h8/h8_temp ~/stuff/h8/{}; done';
```

* Can it be the slot? No
```
[adm_irfant@hrud3-18-49 ~]$ pdsh -w "h8dat0018,h8dat0003" ' sudo /d/admin/scripts/lustre/storcli64 /call show all | grep -E "SN|Device attributes" | while read line; do echo $line; if [[ $line == *"SN"* ]]; then echo $line | awk "{print \$3}" | while read serial; do lsblk -o name,serial | grep $serial; done; fi; done' | dshbak -c
----------------
h8dat0003
----------------
Drive /c0/e0/s0 Device attributes :
SN = 69J3WSAE
sda    69J3WSAE
Drive /c0/e0/s1 Device attributes :
SN = 69J3WSVE
sdb    69J3WSVE
Drive /c0/e0/s2 Device attributes :
SN = 69H1XVTE
sdc    69H1XVTE
Drive /c0/e0/s3 Device attributes :
SN = 66HKYVJK
sdd    66HKYVJK
Drive /c0/e0/s4 Device attributes :
SN = 66HV4G9N
sde    66HV4G9N
Drive /c0/e0/s5 Device attributes :
SN = 69J3WUEE
sdf    69J3WUEE
Drive /c0/e0/s6 Device attributes :
SN = 68JBJ84H
sdg    68JBJ84H
Drive /c0/e0/s7 Device attributes :
SN = 69H1XVDE
sdh    69H1XVDE
Drive /c0/e0/s8 Device attributes :
SN = 69HGR7PE
sdi    69HGR7PE
Drive /c0/e0/s9 Device attributes :
SN = 69J3N7ME
sdj    69J3N7ME
Drive /c0/e0/s10 Device attributes :
SN = 69J3MVGE
sdk    69J3MVGE
Drive /c0/e0/s11 Device attributes :
SN = 69G3SL8F
sdl    69G3SL8F
----------------
h8dat0018
----------------
Drive /c0/e0/s0 Device attributes :
SN = 69GY5S6E
sda    69GY5S6E
Drive /c0/e0/s1 Device attributes :
SN = 69G266ZF
sdb    69G266ZF
Drive /c0/e0/s2 Device attributes :
SN = 66HR1TML
sdc    66HR1TML
Drive /c0/e0/s3 Device attributes :
SN = 69GXRWGE
sde    69GXRWGE
Drive /c0/e0/s4 Device attributes :
SN = 69J3WJZE
sdf    69J3WJZE
Drive /c0/e0/s5 Device attributes :
SN = 69G1K1UF
sdg    69G1K1UF
Drive /c0/e0/s6 Device attributes :
SN = 66HWUNAM
sdh    66HWUNAM
Drive /c0/e0/s7 Device attributes :
SN = 69GMNDNE
sdd    69GMNDNE
Drive /c0/e0/s8 Device attributes :
SN = 69GZ43TE
sdi    69GZ43TE
Drive /c0/e0/s9 Device attributes :
SN = 69GXEAME
sdj    69GXEAME
Drive /c0/e0/s10 Device attributes :
SN = 69GKWUSE
sdk    69GKWUSE
Drive /c0/e0/s11 Device attributes :
SN = 69G2R78F
sdl    69G2R78F
```

* Can it be SAS link problem
```
grep -H . /sys/class/sas_phy/phy-*/sas_address
grep -H . /sys/class/sas_phy/phy-*/negotiated_linkrate

```

### Status 27/05
```
h8dat0008 and potentially some others still struggling:  
  

> [Wed May 27 08:54:17 2026] Lustre: ll_ost_io00_073: service thread pid 3255727 was inactive for 201.639 seconds. Watchdog stack traces are limited to 3 per 300 seconds, skipping this one.
```

### Status 03/06
* It is confirmed that the problem does not caused by the hardware. I am able to replicate the same problem happen on h8 using kpetronaslustre hardware
```
[adm_irfant@kud37 ~]$ pdsh -w "knod2-2-7,knod2-4-6,knod2-4-9,knod2-4-10,knod4-4-2" 'fio --name=zfs_sync_read --filename=/simh8/$HOSTNAME/sync_write/write_throughput.1.0 --rw=randread --bs=4k --direct=1 --ioengine=libaio --size=100G --numjobs=4 --iodepth=32 --group_reporting'
```

```
iostat -xmd 1 40 dm-9 dm-33 dm-6 dm-40 dm-1 dm-0 dm-14 dm-41 dm-15 dm-2 dm-39 dm-4 | tee iostat.sync.read & zpool iostat lustre -r 1 40 | tee zpool.iostat.sync.read & zpool iostat lustre 1 40 | tee zpool.iostat.v.sync.read
```

### Status 04/06
* Initial hypothesis that the problematic disks are throttling zpool is wrong. Offlining the problematic disks during log collection shows that the pool bandwidth and total I/O doesn't experience any improvement.
* We know that it was sync read that throttle other IO. 
* I don't know if epic20 is experiencing the same issue

### Status 05/06
```
#stripe to a specific ost
lfs setstripe -i 1 -c 1 stripe_to_ost_1

```