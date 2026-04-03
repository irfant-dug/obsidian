[[Wisdom]] [[linux]]

* The problem
```
Apr  3 01:35:41 tijanet-jumpbox openvpn3-service-log[224057]: {tag:11469761511745992382} Reconnecting
Apr  3 01:35:41 tijanet-jumpbox openvpn3-service-log[224057]: {tag:8616980854965810742} Socket protect called for socket 8, remote: '96.9.168.174', tun: 'tun0', ipv6: no, device_path=/net/openvpn/v3/netcfg/229689_1a6353fdt3a34t4e17ta49bta1de7dc6bee5, device-object: valid
Apr  3 01:35:51 tijanet-jumpbox openvpn3-service-log[224057]: {tag:11469761511745992382} Reconnecting
Apr  3 01:35:51 tijanet-jumpbox openvpn3-service-log[224057]: {tag:8616980854965810742} Socket protect called for socket 8, remote: '96.9.168.174', tun: 'tun0', ipv6: no, device_path=/net/openvpn/v3/netcfg/229689_1a6353fdt3a34t4e17ta49bta1de7dc6bee5, device-object: valid
Apr  3 01:36:01 tijanet-jumpbox openvpn3-service-log[224057]: {tag:11469761511745992382} Reconnecting
Apr  3 01:36:01 tijanet-jumpbox openvpn3-service-log[224057]: {tag:8616980854965810742} Socket protect called for socket 8, remote: '96.9.168.174', tun: 'tun0', ipv6: no, device_path=/net/openvpn/v3/netcfg/229689_1a6353fdt3a34t4e17ta49bta1de7dc6bee5, device-object: valid
Apr  3 01:36:11 tijanet-jumpbox openvpn3-service-log[224057]: {tag:11469761511745992382} Reconnecting
Apr  3 01:36:11 tijanet-jumpbox openvpn3-service-log[224057]: {tag:8616980854965810742} Socket protect called for socket 8, remote: '96.9.168.174', tun: 'tun0', ipv6: no, device_path=/net/openvpn/v3/netcfg/229689_1a6353fdt3a34t4e17ta49bta1de7dc6bee5, device-object: valid
Apr  3 01:36:13 tijanet-jumpbox NetworkManager[880]: <info>  [1775151373.6680] device (tun0): state change: activated -> unmanaged (reason 'unmanaged-external-down', managed-type: 'external')
Apr  3 01:36:13 tijanet-jumpbox systemd[1]: Starting Network Manager Script Dispatcher Service...
Apr  3 01:36:13 tijanet-jumpbox openvpn3-service-log[224057]: {tag:8616980854965810742} Virtual device '1a6353fdt3a34t4e17ta49bta1de7dc6bee5' registered on /net/openvpn/v3/netcfg/229689_1a6353fdt3a34t4e17ta49bta1de7dc6bee5 (owner uid 996, owner pid 229689)
Apr  3 01:36:13 tijanet-jumpbox openvpn3-service-log[224057]: {tag:8616980854965810742} Adding IP Address 172.20.104.5/24 gw 172.20.104.1 ipv6: no
Apr  3 01:36:13 tijanet-jumpbox systemd[1]: Started Network Manager Script Dispatcher Service.
Apr  3 01:36:13 tijanet-jumpbox openvpn3-service-log[224057]: {tag:8616980854965810742} Setting remote IP address to 96.9.168.174 ipv6: no
Apr  3 01:36:13 tijanet-jumpbox openvpn3-service-log[224057]: {tag:8616980854965810742} Adding network 172.16.0.0/12, metric: (default), exclude: no, ipv6: no
Apr  3 01:36:13 tijanet-jumpbox openvpn3-service-log[224057]: {tag:8616980854965810742} Adding network 10.0.0.0/8, metric: (default), exclude: no, ipv6: no
Apr  3 01:36:13 tijanet-jumpbox openvpn3-service-log[224057]: {tag:8616980854965810742} Adding network 192.168.0.0/16, metric: (default), exclude: no, ipv6: no
Apr  3 01:36:13 tijanet-jumpbox NetworkManager[880]: <info>  [1775151373.7058] manager: (tun0): new Tun device (/org/freedesktop/NetworkManager/Devices/6)
Apr  3 01:36:13 tijanet-jumpbox openvpn3-service-netcfg[229695]: Error while executing NetlinkRoute4(add: 0) tun0: -2
Apr  3 01:36:13 tijanet-jumpbox openvpn3-service-netcfg[229695]: Error while executing NetlinkRoute4(add: 0) tun0: -2
Apr  3 01:36:13 tijanet-jumpbox openvpn3-service-netcfg[229695]: Error while executing NetlinkRoute4(add: 0) tun0: -2
Apr  3 01:36:13 tijanet-jumpbox openvpn3-service-netcfg[229695]: Error while executing NetlinkAddr4(add: 0) tun0: -2
Apr  3 01:36:13 tijanet-jumpbox openvpn3-service-netcfg[229695]: Error while executing NetlinkLinkSet tun0 mtu 1500: -1
Apr  3 01:36:13 tijanet-jumpbox openvpn3-service-netcfg[229695]: Error while executing NetlinkLinkSet tun0 up 0: -2
Apr  3 01:36:13 tijanet-jumpbox openvpn3-service-log[224057]: {tag:11469761511745992382} Failed configuring TUN device (TUN_SETUP_FAILED)
Apr  3 01:36:13 tijanet-jumpbox openvpn3-service-log[224057]: {tag:11469761511745992382} Stopping VPN client
Apr  3 01:36:13 tijanet-jumpbox openvpn3-service-log[224057]: {tag:8616980854965810742} Cleaning up resources for PID 229689.
Apr  3 01:36:23 tijanet-jumpbox systemd[1]: NetworkManager-dispatcher.service: Deactivated successfully.
Apr  3 01:36:29 tijanet-jumpbox systemd[1]: cloudflare-ddns.service: Main process exited, code=exited, status=1/FAILURE
Apr  3 01:36:29 tijanet-jumpbox systemd[1]: cloudflare-ddns.service: Failed with result 'exit-code'.
Apr  3 01:39:17 tijanet-jumpbox systemd[1]: dbus-:1.1-net.openvpn.v3.sessions@1.service: Deactivated successfully.
Apr  3 01:44:18 tijanet-jumpbox systemd[1]: dbus-:1.1-net.openvpn.v3.netcfg@1.service: Deactivated successfully.

```