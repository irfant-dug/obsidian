[[lustre]]
```
[adm_irfant@kud42 ~]$ cd /kl5/admin/balance
[adm_irfant@kud42 balance]$ sudo mkdir 20260302-rebalance
[adm_irfant@kud42 balance]$ sudo rsync -aP /d/admin/scripts/lustre/rebalance/ 20260302-rebalance/
[adm_irfant@kud42 balance]$ sudo -sE
[root@kud42 balance]# cd 20260302-rebalance/
```

```
[root@kud42 20260302-rebalance]# vi config.sh
export FSNAME=kl5
export PARTITIONS=archive
```

```
[root@kud42 20260302-rebalance]# ./01_make_schema.sh 
[root@kud42 20260302-rebalance]# less 02_list_files.schema
[root@kud42 20260302-rebalance]# ./submit.sh 
[root@kud42 20260302-rebalance]# squeue -u root
PARTITION   PRIORITY   NAME                     USER ST       TIME TIME_LEFT  NODES NODELIST(REASON JOBID
archive     990        02_rebalance             root PD       0:00  12:00:00      1 (Resources)     40529269_[1-388%8]
archive     990        03_rebalance             root PD       0:00  12:00:00      1 (Dependency)    40529270
```

Monitor:
```
watch lfs df -h /kl5
```

```
[root@kud42 20260302-rebalance]# multitail --follow-all --mergeall -Iw "000scratch/logs/*" 1
```