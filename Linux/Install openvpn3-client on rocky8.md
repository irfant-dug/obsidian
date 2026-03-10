[[linux]]

```
dnf --enablerepo=devel install tinyxml2
```
```
sudo dnf -y install epel-release
sudo dnf -y install jsoncpp
```
```
dnf install https://packages.openvpn.net/openvpn-openvpn3-epel-repo-1-1.noarch.rpm -y; dnf --enablerepo=devel install tinyxml2 -y; dnf -y install epel-release;  dnf -y install jsoncpp; dnf install openvpn3-client -y
```