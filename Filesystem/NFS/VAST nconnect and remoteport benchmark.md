```
H200:
sudo mount -t nfs -o _netdev,vers=3,noatime,nodiratime,actimeo=3,proto=tcp,mountproto=tcp   10.200.0.108:/h7 /h7
fio --name=seq --directory=/h7/dug/IT/adm_irfant/benchmark/io/layout_file --rw=write --bs=1m --size=64g --numjobs=1 --end_fsync=1 --refill_buffers --group_reporting --output=/h7/dug/IT/adm_irfant/benchmark/io/logs/${HOSTNAME}-num1-default.fio
fio --name=par --directory=/h7/000scratch --rw=write --bs=1m --size=8g --numjobs=16 --end_fsync=1 --refill_buffers --group_reporting --output=/h7/dug/IT/adm_irfant/benchmark/io/logs/${HOSTNAME}-num1-default.fio
```