[[linux]]

* Port Forwarding
```
sudo firewall-cmd --zone=public --add-forward-port=port=80:proto=tcp:toport=80:toaddr=192.168.0.227 --permanent
sudo firewall-cmd --zone=public --add-masquerade --permanent
sudo firewall-cmd --reload
sudo firewall-cmd --zone=public --add-port=80/tcp --permanent
```

* Enable denied logging
```
sudo vi /etc/firewalld/firewalld.conf

LogDenied=off -> LogDenied=all

sudo systemctl restart firewalld.service
```