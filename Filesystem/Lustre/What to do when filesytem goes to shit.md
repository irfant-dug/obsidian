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
```

```

What to do when thing goes to shit

* Decrease h8 license count
```
sacctmgr modify resource name=h8 server=houston set count=2200 --immediate
```

* Find the job with high
```
#This will update all tasks for the given jobid. If immediate reduction in running tasks is required you will need to requeue them (select the tasks that have been running for the shortest time)

scontrol update ArrayTaskThrottle=10 jobid=30971547,30971545,30951720,30971553
scontrol requeue "40684146_[64700-64811]"

scontrol update ArrayTaskThrottle=100 job=29996878
scontrol requeue 29996878_[200-438]


```

Currently, we are focusing on bandwidth and disk utilization to control h8. One way we can look at is to monitor the IOPS. If it is unusually high alongside high read bandwidth, this may point to **Read amplification** phenomenon. Find the file the user that send a lot of this small read IO and throttle it.
Actually, ARC will take care of this for us. Record that is being read multiple time will be stored in ARC.

```
#list job by licence
squeue -tR -o "%20i %15u %20j %.10T %.20W" | awk '/pe1@perth/' | sort -V -k 1

#iostat by 3 hot disks at top
S_COLORS=always stdbuf -oL iostat -y -xmd 1 $(zpool status lustre -L | grep -Eo "sd[a-z]+" | tr '\n' ' ') | awk -v tops="$(zpool status lustre -L | grep -Eo 'sd[a-z]+' | sed -n '4p;8p;12p' | tr '\n' ' ')" 'BEGIN { split(tops, arr, " "); for(i in arr) if (arr[i] != "") top_map[arr[i]]=1; } /^Linux/ || /^Device/ { print; fflush(); next; } NF == 0 { if (t > 0 || r > 0) { for(i=1; i<=t; i++) print top[i]; n = asort(rest, sorted); for(i=1; i<=n; i++) print sorted[i]; print ""; delete top; delete rest; t = 0; r = 0; fflush(); } next; } { is_top = 0; for (dev in top_map) { if ($1 ~ dev) { is_top = 1; break; } } if (is_top) { top[++t] = $0; } else { rest[++r] = $0; } }'

#List all available license
scontrol show licenses

```