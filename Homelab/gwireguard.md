* Set up Wireguard
https://www.digitalocean.com/community/tutorials/how-to-set-up-wireguard-on-ubuntu-20-04
```
[root@gwireguard wireguard]# cat wg0.conf
[Interface]  
PrivateKey = **********************************  
Address = 10.168.168.1/24, fd62:86fa:17de::1/64  
DNS = 1.1.1.1, 8.8.8.8  
#MTU = 1320  
ListenPort = 51820  
PostUp = nft add table ip wireguard; nft add chain ip wireguard wireguard_chain {type nat hook postrouting priority srcnat\; policy accept\;}; nft add rule ip wireguard wireguard_chain coun  
ter packets 0 bytes 0 masquerade; nft add table ip6 wireguard; nft add chain ip6 wireguard wireguard_chain {type nat hook postrouting priority srcnat\; policy accept\;}; nft add rule ip6 wi  
reguard wireguard_chain counter packets 0 bytes 0 masquerade  
PostDown = nft delete table ip wireguard; nft delete table ip6 wireguard  
  
#OP12  
[Peer]  
PublicKey = ************************************** 
AllowedIPs = 10.168.168.2/32, fd62:86fa:17de::2/128
```

* Enable the wg-quick systemd service
```
[root@gwireguard zones]# systemctl sudo systemctl enable wg-quick@wg0.service  
Unknown operation sudo.  
[root@gwireguard zones]# sudo systemctl enable wg-quick@wg0.service  
Created symlink /etc/systemd/system/multi-user.target.wants/wg-quick@wg0.service → /usr/lib/systemd/system/wg-quick@.service.  
[root@gwireguard zones]# sudo systemctl daemon-reload  
[root@gwireguard zones]# sudo systemctl start wg-quick@wg0  
[root@gwireguard zones]# systemctl status wg-quick@wg0
```

* Enable port 51820 in firewalld
* Ping VPN gateway (172.20.104.1) every 1 hour to keep the OpenVPN connection alive (/etc/cron.hourly/openvpn_keepalive.sh)
* Set DNS server to gdns0001 (192.168.0.158)

* Set static route to avoid wireguard from hijacking
```
[magus@gwireguard ~]$ sudo nmcli connection modify ens18 +ipv4.routes "96.9.168.174 192.168.0.1"
[sudo] password for magus:
[magus@gwireguard ~]$ sudo nmcli connection modify ens18 +ipv4.routes "193.228.10.100 192.168.0.1"
[magus@gwireguard ~]$ ip route
default via 192.168.0.1 dev ens18 proto static metric 100
10.168.168.0/24 dev wg0 proto kernel scope link src 10.168.168.1
192.168.0.0/24 dev ens18 proto kernel scope link src 192.168.0.154 metric 100
```