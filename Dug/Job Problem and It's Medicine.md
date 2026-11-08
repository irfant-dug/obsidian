* pt2 jobs crashing KNC [#371785](https://downunder.zendesk.com/agent/tickets/371785)
```
#Requeuehold job, update job to exclude knc, release job
[adm_irfant@hrud3-18-49 ~]$ scontrol show job 49736587_16 | grep -i restart
   Requeue=1 Restarts=13 BatchFlag=1 Reboot=0 ExitCode=0:0
[adm_irfant@hrud3-18-49 ~]$ scontrol requeuehold 49736587_16
[adm_irfant@hrud3-18-49 ~]$ scontrol update jobid=49736587_16 Features="cpu&(nodetype-knl-192&fastio|nodetype-knl-256&fastio|nodetype-knl-384&fastio)"
[adm_irfant@hrud3-18-49 ~]$ scontrol release 49736587_16
[adm_irfant@hrud3-18-49 ~]$ scontrol show job 49736587_16 | grep -i -e state -e features
   JobState=PENDING Reason=Resources Dependency=(null)
   Features=cpu&(nodetype-knl-192&fastio|nodetype-knl-256&fastio|nodetype-knl-384&fastio) DelayBoot=00:00:00
```
