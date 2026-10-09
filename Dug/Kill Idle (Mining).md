[[dug]]

```
To stop it on this node (run as root on knod3-7-11):
screen -S idle -X quit          # ends the screen session, idle-inner.sh and tee
pkill -f idle-inner.sh          # in case anything survived
ps aux | rg -i 'xmrig|idle-inner'   # check it's gone
```
```
[root@knod3-7-11 adm_irfant]# systemctl status idle
○ idle.service - idle task
     Loaded: loaded (/etc/systemd/system/idle.service; enabled; preset: disabled)
     Active: inactive (dead) since Thu 2026-10-08 23:45:13 +08; 9h ago
   Duration: 268ms
   Main PID: 47406 (code=exited, status=0/SUCCESS)
        CPU: 270ms
[root@knod3-7-11 adm_irfant]# cat /etc/systemd/system/idle.service
[Unit]
Description=idle task
After=network.target slurmd.service remote-fs.target
Requires=remote-fs.target
ConditionPathExists=/d/sw/dug/idle/latest/idle

[Service]
Type=forking
ExecStart=/d/sw/dug/idle/latest/idle start
ExecStop=/d/sw/dug/idle/latest/idle stop
[Install]
WantedBy=multi-user.target
```