![[Pasted image 20260207161808.png]]
![[Pasted image 20260207161934.png]]


kp43 both card missing 2 disk (16,18)



## NVME

* failed drive
```
[adm_irfant@kud42 ~]$ pdsh -g "filesystem=epic20" '/usr/bin/lsscsi | grep nvm | wc -l' 2>/dev/null | dshbak -c
kpetronaslustre[11,13,18,22,26,31,38,44]
----------------
2

[adm_irfant@kud42 ~]$ pdsh -w "kpetronaslustre[11,13,18,22,26,31,38,44]" 'find /dev -maxdepth 1 -regex '.*nvme[0-9]*' | sort -V | while read line; do echo $line; sudo smartctl -a $line | grep -iE "test result|fa
iled"; done;' 2>/dev/null | dshbak -c

kpetronaslustre[18,22,31,44] 
kpetronaslustre[26,38] nvme1
```


* sanitize
```
msecli -X -B -n /dev/nvmeXX
```
* reset
```
nvme reset /dev/nvmeXX
```

* Upgrade
```
/d/sw/3ware/msecli/msecli -F -U /d/sw/3ware/msecli/Micron_7500_E3MQ005_release.ubi -n /dev/nvme0 -S 2
```

* confirm
```
msecli -F -n /dev/nvmeX
```

* Remove spare zpool
```
zpool remove lustre.ssd nvme2n1
```

* Add back the spare
```
zpool add lustre.ssd spare /dev/nvme2n1
```


Excerpt
* /var/log/messages
```
Feb  8 01:00:57 kpetronaslustre11 monitoring[30053]: echo -n 2048 > /sys/block/nvme2n1/queue/nr_requests                                                                                                           
Feb  8 01:00:57 kpetronaslustre11 monitoring[30053]: echo -n 2048 > /sys/block/nvme2n1/queue/read_ahead_kb

Feb  8 01:07:58 kpetronaslustre11 monitoring[30053]: echo -n 2048 > /sys/block/nvme2n1/queue/nr_requests
```