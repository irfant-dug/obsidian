
* Create Node Reservation
```
scontrol create ReservationName="irfan=testing NVMe backplane" StartTime=now Nodes=
```

* Check 
```
[adm_irfant@knod4-2-16 ~]$ sacct -j 41727073 --format=JobID%30,JobName,Partition,Account,AllocCPUS,State,ExitCode
                         JobID    JobName  Partition    Account  AllocCPUS      State ExitCode 
------------------------------ ---------- ---------- ---------- ---------- ---------- -------- 
                      41727073 PSDM_TRAV+   petronas   petronas        224 CANCELLED+      0:0 
                41727073.batch      batch              petronas        112  CANCELLED      0:0 
               41727073.extern     extern              petronas        224  CANCELLED      0:0 

```