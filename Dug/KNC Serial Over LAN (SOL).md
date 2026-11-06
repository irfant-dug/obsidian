```
stty -F /dev/ttyS1 115200
ipmitool -I lanplus -H hnod4-14-21-ipmi -U ipmi -P ipmi sol activate
```