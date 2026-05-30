About time I documents and explains the services hosted in the homelab

# tijanieserver
## 1. tailscale-ct (192.168.0.220)
* tailscale - bind to tijanieserver@gmail.com account
* No longer running cloudflare-ddns
## 2. jellyfin-ct (192.168.0.221)
* Running jellyfin
* Mount nas.tijanet.com SMB
## 3. nextcloudpi-ct (192.168.0.223)
* Running NextCloud
* Mount nas.tijanet.com SMB
## 4. wikijs-ct (192.168.0.224)
* running Wiki-JS
* Should be a container
* Not used
## 5. dmz-vm (192.168.0.204)
* Public IP will point to this vm
* Port forwarding will point to VM that need to be publicly accesible
* Firewalld is used for port forwarding

| Port  | VM         |
| ----- | ---------- |
| 80    | proxy-vm   |
| 443   | proxy-vm   |
| 51820 | gwireguard |
## 6.[[ proxy-vm]] (192.168.0.205)
* Running nginx reverse proxy

## 7. tijanet-jumpbox (192.168.0.206)
* jumpbox for accessing tijanieserver

## 8. grafana-vm
* Formerly used to give data to Ms. Chong
* Running grafana and connected to DUG through OpenVPN

## 9. docker-vm
* Running qbittorent, uptime-kuma, and Librespeed using Docker Compose

# gori

## 1. gjump (192.168.0.152)
## 2. TrueNAS-VM (192.168.0.153)
* Running Truenas Scale
* Serving Pool1 as SMB and NFS

## 2. [[gwireguard]]
* Should be the one-stop VPN services

## 3. 