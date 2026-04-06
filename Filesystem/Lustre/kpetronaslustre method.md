[[lustre]]

* Get controller with Drives
```
[root@kpetronaslustre45 /]# STORCLI /call show | grep -E "Controller|Physical Drives" | grep "Physical Drives" -B 1 | head -1 | awk '{print $3}'
0
```

* Get Drives of Controller with Drives
```
[root@kpetronaslustre45 /]# STORCLI='/d/sw/3ware/storcli/opt/MegaRAID/storcli/storcli64'; $STORCLI /call show | grep -E "Controller|Physical Drives" | grep "Physical Drives" -B 1 | head -1 | awk '{print $3}' | while read line; do $STORCLI /c$line show all | grep -E "Device attributes|SN"; done;
```

* Get Drives for lustre_2
```
[root@kpetronaslustre45 /]# STORCLI='/d/sw/3ware/storcli/opt/MegaRAID/storcli/storcli64'; $STORCLI /call show | grep -E "Controller|Physical Drives" | grep "Physical Drives" -B 1 | head -1 | awk '{print $3}' | while read line; do $STORCLI /c$line show all | grep -E "Device attributes|SN" | grep -E "e45|s5|s11|s17|s23" -A 1 --no-group-separator; done;
```


* Get disks not used by zpool lustre
```
[root@kpetronaslustre45 ~]# zpool status lustre -Lv | grep dm | awk '{print $1}' | tr "\n" "|" | sed 's/|$/\n/' | while read line; do ls -l /dev/disk/by-id/dm-uuid* | grep -vE $line | awk '{print $9}' | sed 's/\/dev\/disk\/by-id\///' | tr "\n" " " | sed 's/ $/\n/'; done;
```

* Get Drive for Lustre_2
```
[root@kpetronaslustre45 /]# STORCLI='/d/sw/3ware/storcli/opt/MegaRAID/storcli/storcli64'; $STORCLI /call show | grep -E "Controller|Physical Drives" | grep "Physical Drives" -B 1 | head -1 | awk '{print $3}' | while read line; do $STORCLI /c$line show all | grep -E "Device attributes|SN" | grep -E "e45|s5|s11|s17|s23" -A 1 --no-group-separator | while read line; do echo $line; if echo $line | grep -q "SN = "; then echo $line | awk '{print $3}' ; fi;done; done;
```

* Get Drives with dm-uuid
```
[root@kpetronaslustre45 /]# STORCLI='/d/sw/3ware/storcli/opt/MegaRAID/storcli/storcli64'; $STORCLI /ca
ll show | grep -E "Controller|Physical Drives" | grep "Physical Drives" -B 1 | head -1 | awk '{print $
3}' | while read line; do $STORCLI /c$line show all | grep -E "Device attributes|SN" | grep -E "e45|s5
|s11|s17|s23" -A 1 --no-group-separator | while read line; do echo $line; if echo $line | grep -q "SN
= "; then echo $line | awk '{print $3}' | while read line; do lsblk -o name,serial | grep $line | head
 -1 | awk '{print $1}' | while read line; do multipath -l | tac | sed -n "/$line /,/mpath/p" | grep mp
ath | awk '{print $2}' | sed 's/(/dm-uuid-mpath-/ ; s/)//' ; echo; done; done; fi;done; done
```