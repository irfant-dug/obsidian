[[linux]]

* Install
```
dnf install make autoconf automake openssl-devel libnl3-devel gcc
```

* lib
```
dnf --enablerepo=devel install file-devel ipset-devel libnfnetlink-devel libnftnl-devel kmod-devel

dnf install gcc make autoconf automake openssl-devel libnl3-devel     iptables-devel ipset-devel net-snmp-devel libnfnetlink-devel file-devel     glib2-devel pcre2-devel libnftnl-devel libmnl-devel systemd-devel kmod-devel
```

* firewalld
```
firewall-cmd --zone=public --add-rich-rule='rule family="ipv4" destination address="224.0.0.18" protocol value="112" accept' --permanent
```

* 