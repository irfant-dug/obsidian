[[dug]]

```
rsync -av /skel /newuser
chown uid:group dir
```

* Generate VPN cert
```
[adm_irfant@kud42 ~]$ ssh pcertms
[adm_irfant@pcertms ~]$ cd /home/docker/dugca-certmaker/
[adm_irfant@pcertms dugca-certmaker]$ ls
auth  dugca-certmaker.py  dugca-certmaker.py.bak  dugca-email-vpn-certs.py  id-bundles  ipmi-bundles  logs  __pycache__  README.md  svc-bundles  templates  vpn-bundles
[adm_irfant@pcertms dugca-certmaker]$ ./dugca-certmaker.py -t vpn -u amiraha
Creating VPN CSR config /home/docker/dugca-certmaker/vpn-bundles/amiraha-vpn-1777947051.1459284/csr.conf
```