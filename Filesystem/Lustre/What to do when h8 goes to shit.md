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
```