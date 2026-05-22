## /kl5
```
[root@kpetronaslustre45 tmp]# sudo umount lustre/irfanfs-OST0001
[root@kpetronaslustre45 tmp]# sudo zfs set canmount=on lustre/irfanfs-OST0001
[root@kpetronaslustre45 tmp]# sudo zfs mount lustre/irfanfs-OST0001
[root@kpetronaslustre45 0]# sudo ll_decode_filter_fid /lustre/irfanfs-OST0001/O/0/d6/6
/lustre/irfanfs-OST0001/O/0/d6/6: parent=[0x200000401:0x6d:0x0] stripe=1 stripe_size=1048576 stripe_count=2 layout_version=0 range=0
[root@kpetronaslustre45 ~]# sudo umount lustre/irfanfs-OST0001
[root@kpetronaslustre45 ~]# sudo zfs set canmount=off lustre/irfanfs-OST0001
[root@kpetronaslustre45 ~]# sudo mount -t lustre lustre/irfanfs-OST0001 /lustre/irfanfs-OST0001
```

Find the file location
```
sudo -sE
sudo umount lustre/kl5-OST002a
sudo zfs set canmount=on lustre/kl5-OST002a
sudo zfs mount lustre/kl5-OST002a
sudo ls -l /lustre/kl5-OST002a/O/ac0000400/d15/311983
sudo ll_decode_filter_fid lustre/kl5-OST002a:/O/ac0000400/d15/311983
####################################
sudo umount /lustre/kl5-OST002a
sudo umount lustre/kl5-OST002a - if above not working
####################################
sudo zfs set canmount=off lustre/kl5-OST002a
sudo mount -t lustre lustre/kl5-OST002a /lustre/kl5-OST002a
```

Find the corrupted file
https://confluence.dug.com/pages/viewpage.action?pageId=136545848
https://downunder.zendesk.com/agent/tickets/231230
```
sudo lfs fid2path /kl5 
```

## kpetronaslustre42 Disk Replacement

```
#umount kpetronaslustre from all client
ssh kpetronaslustre00
lshowmount | grep -v lo | sed 's/@tcp$//' | tr '\n' ',' | sed 's/,$/\n/'
scontrol show hostname ##output of the lshowmount## | tr '\n' ',' | sed 's/,$/\n/' | xargs -I {} pdsh -w {} "sudo kill -9 \$(sudo /bin/lsof /data/epic20 | awk '{ print \$2 }'); sleep 1; sudo /bin/umount /data/epic20"

#umount lustre OST
ssh kpetronaslustre42
sudo umount lustre/epic20-OST0019
sudo umount lustre_2/epic20-OST0059

#mount lustre OST
ssh kpetronaslustre42
sudo mount -t lustre/epic20-OST0019 /lustre/epic20-OST0019
sudo mount -t lustre_2/epic20-OST0059 /lustre_2/epic20-OST0059
```


