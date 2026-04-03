[[Wisdom]] [[linux]]

* The problem (20260403)
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

* Problem (20260402)
```
Apr  2 17:24:17 tijanet-jumpbox openvpn3-service-sessionmgr[229670]: OpenVPN3/Linux v27 (openvpn3-service-sessionmgr)
Apr  2 17:24:17 tijanet-jumpbox openvpn3-service-sessionmgr[229670]: OpenVPN core v3.11.6 linux x86_64 64-bit
Apr  2 17:24:17 tijanet-jumpbox openvpn3-service-sessionmgr[229670]: Copyright (C) 2012- OpenVPN Inc. All rights reserved.
Apr  2 17:24:17 tijanet-jumpbox openvpn3-service-log[224057]: {tag:7419288117326837780} OpenVPN 3 Session Manager started
Apr  2 17:24:17 tijanet-jumpbox systemd[1]: Started dbus-:1.1-net.openvpn.v3.configuration@1.service.
Apr  2 17:24:17 tijanet-jumpbox openvpn3-service-configmgr[229676]: OpenVPN3/Linux v27 (openvpn3-service-configmgr)
Apr  2 17:24:17 tijanet-jumpbox openvpn3-service-configmgr[229676]: OpenVPN core v3.11.6 linux x86_64 64-bit
Apr  2 17:24:17 tijanet-jumpbox openvpn3-service-configmgr[229676]: Copyright (C) 2012- OpenVPN Inc. All rights reserved.
Apr  2 17:24:17 tijanet-jumpbox systemd[1]: Started dbus-:1.1-net.openvpn.v3.backends@1.service.
Apr  2 17:24:17 tijanet-jumpbox openvpn3-service-backendstart[229682]: OpenVPN3/Linux v27 (openvpn3-service-backendstart)
Apr  2 17:24:17 tijanet-jumpbox openvpn3-service-backendstart[229682]: OpenVPN core v3.11.6 linux x86_64 64-bit
Apr  2 17:24:17 tijanet-jumpbox openvpn3-service-backendstart[229682]: Copyright (C) 2012- OpenVPN Inc. All rights reserved.
Apr  2 17:24:17 tijanet-jumpbox openvpn3-service-backendstart[229688]: Re-initiated process from pid 229688 to backend process pid 229689
Apr  2 17:24:17 tijanet-jumpbox openvpn3-service-backendstart[229689]: OpenVPN3/Linux v27 (openvpn3-service-client)
Apr  2 17:24:17 tijanet-jumpbox openvpn3-service-backendstart[229689]: OpenVPN core 3.11.6 linux x86_64 64-bit
Apr  2 17:24:17 tijanet-jumpbox openvpn3-service-backendstart[229689]: Copyright (C) 2012- OpenVPN Inc. All rights reserved.
Apr  2 17:24:18 tijanet-jumpbox systemd[1]: Starting Hostname Service...
Apr  2 17:24:18 tijanet-jumpbox systemd[1]: Started Hostname Service.
Apr  2 17:24:18 tijanet-jumpbox systemd[1]: Started dbus-:1.1-net.openvpn.v3.netcfg@1.service.
Apr  2 17:24:18 tijanet-jumpbox openvpn3-service-netcfg[229695]: Loading configuration file: /var/lib/openvpn3/netcfg.json
Apr  2 17:24:18 tijanet-jumpbox openvpn3-service-netcfg[229695]: OpenVPN3/Linux v27 (openvpn3-service-netcfg)
Apr  2 17:24:18 tijanet-jumpbox openvpn3-service-netcfg[229695]: OpenVPN core v3.11.6 linux x86_64 64-bit
Apr  2 17:24:18 tijanet-jumpbox openvpn3-service-netcfg[229695]: Copyright (C) 2012- OpenVPN Inc. All rights reserved.
Apr  2 17:24:23 tijanet-jumpbox openvpn3-service-log[224057]: {tag:11469761511745992382} Starting connection
Apr  2 17:24:23 tijanet-jumpbox openvpn3-service-log[224057]: {tag:8616980854965810742} Cleaning up resources for PID 229689.
Apr  2 17:24:23 tijanet-jumpbox openvpn3-service-log[224057]: {tag:8616980854965810742} Socket protect called for socket 8, remote: '96.9.168.174', tun: '', ipv6: no, device_path=/, device-object: missing
Apr  2 17:24:23 tijanet-jumpbox openvpn3-service-log[224057]: {tag:11469761511745992382} Connecting
Apr  2 17:24:25 tijanet-jumpbox openvpn3-service-log[224057]: {tag:8616980854965810742} Virtual device '1a6353fdt3a34t4e17ta49bta1de7dc6bee5' registered on
/net/openvpn/v3/netcfg/229689_1a6353fdt3a34t4e17ta49bta1de7dc6bee5 (owner uid 996, owner pid 229689)
Apr  2 17:24:25 tijanet-jumpbox openvpn3-service-log[224057]: {tag:8616980854965810742} Adding IP Address 172.20.104.6/24 gw 172.20.104.1 ipv6: no
Apr  2 17:24:25 tijanet-jumpbox openvpn3-service-log[224057]: {tag:8616980854965810742} Setting remote IP address to 96.9.168.174 ipv6: no
Apr  2 17:24:25 tijanet-jumpbox openvpn3-service-log[224057]: {tag:8616980854965810742} Adding network 172.16.0.0/12, metric: (default), exclude: no, ipv6: no
Apr  2 17:24:25 tijanet-jumpbox openvpn3-service-log[224057]: {tag:8616980854965810742} Adding network 10.0.0.0/8, metric: (default), exclude: no, ipv6: no
Apr  2 17:24:25 tijanet-jumpbox openvpn3-service-log[224057]: {tag:8616980854965810742} Adding network 192.168.0.0/16, metric: (default), exclude: no, ipv6: no
Apr  2 17:24:25 tijanet-jumpbox NetworkManager[880]: <info>  [1775121865.6101] manager: (tun0): new Tun device (/org/freedesktop/NetworkManager/Devices/5)
Apr  2 17:24:25 tijanet-jumpbox openvpn3-service-log[224057]: {tag:11469761511745992382} Connected: irfant@kualalumpur.dug.com:1194 (96.9.168.174) via /UDPv4 on tun/172.20.104.6/ gw=[172.20.104.1/] mtu=1500
Apr  2 17:24:25 tijanet-jumpbox NetworkManager[880]: <info>  [1775121865.6254] device (tun0): state change: unmanaged -> unavailable (reason 'connection-assumed', managed-type: 'external')
Apr  2 17:24:25 tijanet-jumpbox NetworkManager[880]: <info>  [1775121865.6263] device (tun0): state change: unavailable -> disconnected (reason 'connection-assumed', managed-type: 'external')
Apr  2 17:24:25 tijanet-jumpbox NetworkManager[880]: <info>  [1775121865.6271] device (tun0): Activation: starting connection 'tun0' (e7c8d8db-2de7-4adb-8341-4063a1c454df)
Apr  2 17:24:25 tijanet-jumpbox NetworkManager[880]: <info>  [1775121865.6272] device (tun0): state change: disconnected -> prepare (reason 'none', managed-type: 'external')
Apr  2 17:24:25 tijanet-jumpbox NetworkManager[880]: <info>  [1775121865.6279] device (tun0): state change: prepare -> config (reason 'none', managed-type: 'external')
Apr  2 17:24:25 tijanet-jumpbox NetworkManager[880]: <info>  [1775121865.6285] device (tun0): state change: config -> ip-config (reason 'none', managed-type: 'external')
Apr  2 17:24:25 tijanet-jumpbox NetworkManager[880]: <info>  [1775121865.6313] device (tun0): state change: ip-config -> ip-check (reason 'none', managed-type: 'external')
Apr  2 17:24:25 tijanet-jumpbox systemd[1]: Starting Network Manager Script Dispatcher Service...
Apr  2 17:24:25 tijanet-jumpbox systemd[1]: Started Network Manager Script Dispatcher Service.
Apr  2 17:24:25 tijanet-jumpbox NetworkManager[880]: <info>  [1775121865.6572] device (tun0): state change: ip-check -> secondaries (reason 'none', managed-type: 'external')
Apr  2 17:24:25 tijanet-jumpbox NetworkManager[880]: <info>  [1775121865.6574] device (tun0): state change: secondaries -> activated (reason 'none', managed-type: 'external')
Apr  2 17:24:25 tijanet-jumpbox NetworkManager[880]: <info>  [1775121865.6580] device (tun0): Activation: successful, device activated.
Apr  2 17:24:25 tijanet-jumpbox systemd[1]: iscsi.service: Unit cannot be reloaded because it is inactive.
Apr  2 17:24:35 tijanet-jumpbox systemd[1]: NetworkManager-dispatcher.service: Deactivated successfully.
Apr  2 17:24:48 tijanet-jumpbox systemd[1]: systemd-hostnamed.service: Deactivated successfully.
Apr  2 17:25:17 tijanet-jumpbox systemd[1]: dbus-:1.1-net.openvpn.v3.backends@1.service: Deactivated successfully.
Apr  2 17:25:17 tijanet-jumpbox systemd[1]: dbus-:1.1-net.openvpn.v3.backends@1.service: Unit process 229689 (openvpn3-servic) remains running after unit stopped
```

* Caused: NetworkManager delete tun0 when the tunnel change state to inactive

* Solution: Tell NetworkManager to ignore tun state change
```
[keyfile]
unmanaged-devices=type:tun
```