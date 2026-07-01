1. #317746 Conflict in Session
* Insight database lock. Caused by unexpected Insight shutdown
* check the .lock, confirm that the lock belong to the user, delete lock
2. #319123 3ware disk replacement
* During disk replacement, slot shows unit as:
```
  p10   OK             u?   2.73 TB   SATA  10  -            ST33000650NS
```
* The unit need to be deleted
```
sudo /cluster/bin/tw_cli maint deleteunit c0 u10
sudo /cluster/bin/tw_cli /c0 rescan noscan
```
* Then rescan the bus
```
sudo /cluster/bin/tw_cli /c0 rescan noscan
echo 1 > /sys/block/sdk/device/delete
```

3. https://downunder.zendesk.com/agent/tickets/319395
* Look at perfmon to see which user/desktop is opening the project. Check every desktop to know which one is hanging (holding project lock). 
* Ask the user to kill their insight

3. https://downunder.zendesk.com/agent/tickets/363058
```
[adm_irfant@pwebapp bin]$ ps -eo pid,ppid,user,cmd | grep -iE 'pwebapp|dugportal|gunicorn|uwsgi|node|flask|estimate' | grep -v grep
 1787     1 akbarg   /bin/bash /opt/apps/estimate_website_app/app-startup.sh
 1788  1787 akbarg   java -jar /opt/apps/estimate_website_app/dugwave_estimate-1510060.jar
[adm_irfant@pwebapp bin]$ sudo cat /proc/1788/environ | tr '\0' '\n' | grep -E 'DUG_LICENSE|DUGEO_OFFICE'
DUG_LICENSE=PLAT-LP5P-VX3A-YQDK-0NK9-8J
DUG_LICENSE_SERVER=plic0001:8080
USE_DUG_LICENSE=true
```