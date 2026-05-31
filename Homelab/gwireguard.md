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

