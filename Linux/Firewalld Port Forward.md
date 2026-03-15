[[linux]]

```
sudo firewall-cmd --zone=public --add-forward-port=port=80:proto=tcp:toport=80:toaddr=192.168.0.227 --permanent
sudo firewall-cmd --zone=public --add-masquerade --permanent
sudo firewall-cmd --reload
sudo firewall-cmd --zone=public --add-port=80/tcp --permanent
```