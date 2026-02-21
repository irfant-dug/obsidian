[[linux]]

* Master
```
vrrp_script keepalived_check {
      script "/root/keepalived_scripts/keepalived_check.sh"
      interval 1
      timeout 5
      rise 3
      fall 3
}
vrrp_instance VI_1 {
        state MASTER
        interface ens18
        virtual_router_id 51
        priority 255
        advert_int 1
        authentication {
              auth_type PASS
              auth_pass 56789
        }
        virtual_ipaddress {
              192.168.100.240/24
        }
        track_script {
                keepalived_check
        }
        notify_master "/root/keepalived_scripts/to_master.sh"
        notify_backup "/root/keepalived_scripts/to_backup.sh"
        notify_stop "/root/keepalived_scripts/to_stop.sh"
        notify_fault "/root/keepalived_scripts/to_stop.sh"
}
```

* Backup
```
vrrp_instance VI_1 {
        state BACKUP
        interface ens18
        virtual_router_id 51
        priority 254
        advert_int 1
        authentication {
              auth_type PASS
              auth_pass 56789
        }
        virtual_ipaddress {
              192.168.100.240/24
        }
        notify_master "/root/keepalived_scripts/to_master.sh"
        notify_backup "/root/keepalived_scripts/to_backup.sh"
        notify_stop "/root/keepalived_scripts/to_stop.sh"
}
```
