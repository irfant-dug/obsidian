[[Lustre]]

By default
```
[adm_irfant@hrud3-18-49 ~]$ scontrol show licenses h8@houston; scontrol show licenses ioh8@houston
LicenseName=h8@houston
    Total=2400 Used=1357 Free=1043 Reserved=0 Remote=yes
    LastConsumed=0 LastDeficit=0 LastUpdate=2026-06-12T08:01:02
LicenseName=ioh8@houston
    Total=80 Used=1 Free=79 Reserved=0 Remote=yes
    LastConsumed=0 LastDeficit=0 LastUpdate=2026-06-01T11:07:28
```

What to do when thing goes to shit

* Decrease h8 license count
```
sacctmgr modify resource name=h8 server=houston set count=2200 --immediate
```

* Find the job with high
```
scontrol update ArrayTaskThrottle=10 jobid=30971547,30971545,30951720,30971553
scontrol requeue "40684146_[64700-64811]"

scontrol update ArrayTaskThrottle=100 job=29996878
scontrol requeue 29996878_[200-438]
```

Currently, we are focusing on bandwidth and disk utilization to control h8. One way we can look at is to monitor the IOPS. If it is unusually high alongside high read bandwidth, this may point to **Read amplification** phenomenon. Find the file the user that send a lot of this small read IO and throttle it.