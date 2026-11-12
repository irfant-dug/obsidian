
```
#Go to BIOS
[adm_irfant@hrud3-18-49 ~]$ ipmitool -I lanplus -H hpetabitepve2-24-1-ipmi -U ipmi -P ipmi chassis bootdev bios
Set Boot Device to bios

#Power Reset
[adm_irfant@hrud3-18-49 ~]$ ipmitool -I lanplus -H hpetabitepve2-24-1-ipmi -U ipmi -P ipmi chassis power reset
Chassis Power Control: Reset

#BMC Reset
ipmitool -I lanplus -H hpetabitepve2-24-1-ipmi -U ipmi -P ipmi  mc reset cold
```