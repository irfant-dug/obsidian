
```
sudo ipmitool sensor | grep inlet -i | awk -F'|' '{print $2}' | xargs
```

