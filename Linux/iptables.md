[[linux]] [[Linux Networking]]

* Port Forwarding
```
vi /etc/sysctl.conf

[root@gwireguard magus]# sysctl -p
net.ipv4.ip_forward = 1
net.ipv6.conf.all.forwarding = 1

sudo iptables -t nat -A PREROUTING -p tcp --dport 5900 -j DNAT --to-destination 192.168.0.156:5900

sudo iptables -t nat -A POSTROUTING -j MASQUERADE
```