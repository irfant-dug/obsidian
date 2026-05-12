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