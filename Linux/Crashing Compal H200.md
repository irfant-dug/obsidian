```
V=172.18.255.20
N=${1:-16}
for i in $(seq 1 $N); do
  mkdir -p /mnt/r$i
  # odd loops WALK (no nosharecache); even loops KILL (private sb, guaranteed last ref)
  if [ $((i % 2)) -eq 0 ]; then O=ro,vers=4.2,nosharecache; else O=ro,vers=4.2; fi
  ( while :; do
      mount -t nfs4 -o $O $V:/epic2/sw /mnt/r$i 2>/dev/null
      umount /mnt/r$i 2>/dev/null
    done ) &
done
wait
```

Makedump
```
makedumpfile --dump-dmesg /proc/vmcore /tmp/vmcore-dmesg.txt
{ echo "###VMCORE-BEGIN###"; tail -n 250 /tmp/vmcore-dmesg.txt; echo "###VMCORE-END###"; } > /dev/ttyS0
{ echo "###FULL-BEGIN###"; cat /tmp/vmcore-dmesg.txt; echo "###FULL-END###"; } > /dev/ttyS0
```


knod1-5-1
```
2026-09-17T22:51:28+08:00 knod1-5-1 systemd[1]: session-c98.scope: Deactivated successfully.
2026-09-17T22:51:28+08:00 knod1-5-1 systemd[1]: sw-hpc.mount: Deactivated successfully.
2026-09-17T22:51:29+08:00 knod1-5-1 kernel: Lustre: Unmounted epic20-client
2026-09-17T22:51:29+08:00 knod1-5-1 systemd[1]: home-junxiao.li.mount: Deactivated successfully.
2026-09-17T22:51:29+08:00 knod1-5-1 systemd[1]: sw-epic2.mount: Deactivated successfully.
2026-09-17T22:51:29+08:00 knod1-5-1 systemd[1]: data-epic20.mount: Deactivated successfully.
```
knod3-2-22
```
2026-09-17T22:37:17+08:00 knod3-2-22 systemd[1]: session-c47.scope: Deactivated successfully.
2026-09-17T22:37:17+08:00 knod3-2-22 systemd[1]: sw-hpc.mount: Deactivated successfully.
2026-09-17T22:37:17+08:00 knod3-2-22 kernel: Lustre: Unmounted epic20-client
2026-09-17T22:37:17+08:00 knod3-2-22 rsyncd[399968]: connect from UNDETERMINED (172.18.50.153)
2026-09-17T22:37:17+08:00 knod3-2-22 rsyncd[399968]: Invalid uid root
2026-09-17T22:37:18+08:00 knod3-2-22 systemd[1]: home-junxiao.li.mount: Deactivated successfully.
2026-09-17T22:37:18+08:00 knod3-2-22 systemd[1]: sw-epic2.mount: Deactivated successfully.
2026-09-17T22:37:18+08:00 knod3-2-22 systemd[1]: data-epic20.mount: Deactivated successfully.
2026-09-17T22:37:26+08:00 knod3-2-22 rsyncd[399991]: connect from UNDETERMINED (172.28.0.43)
2026-09-17T22:37:26+08:00 knod3-2-22 rsyncd[399991]: Invalid uid root
```
knod4-3-20
```
2026-09-18T08:19:24+08:00 knod4-3-20 systemd[1]: session-c11393.scope: Deactivated successfully.
2026-09-18T08:19:24+08:00 knod4-3-20 systemd[1]: sw-hpc.mount: Deactivated succe
ssfully.
2026-09-18T08:19:24+08:00 knod4-3-20 systemd[1]: sw-epic2.mount: Deactivated successfully.
2026-09-18T08:19:25+08:00 knod4-3-20 kernel: Lustre: Unmounted epic20-client
2026-09-18T08:19:25+08:00 knod4-3-20 systemd[1]: home-junxiao.li.mount: Deactivated successfully.
2026-09-18T08:19:25+08:00 knod4-3-20 systemd[1]: data-epic20.mount: Deactivated successfully.
2026-09-18T08:19:26+08:00 knod4-3-20 systemd[1]: Stopping User Manager for UID 0...
2026-09-18T08:19:26+08:00 knod4-3-20 systemd[4146297]: Activating special unit Exit the Session...
2026-09-18T08:19:26+08:00 knod4-3-20 systemd[4146297]: Stopped target Main User Target.
2026-09-18T08:19:26+08:00 knod4-3-20 systemd[4146297]: Stopped target Basic System.
2026-09-18T08:19:26+08:00 knod4-3-20 systemd[4146297]: Stopped target Paths.
2026-09-18T08:19:26+08:00 knod4-3-20 systemd[4146297]: Stopped target Sockets.
2026-09-18T08:19:26+08:00 knod4-3-20 systemd[4146297]: Stopped target Timers.
2026-09-18T08:19:26+08:00 knod4-3-20 systemd[4146297]: Stopped Daily Cleanup of User's Temporary Directories.
2026-09-18T08:19:26+08:00 knod4-3-20 systemd[4146297]: Closed D-Bus User Message Bus Socket.
2026-09-18T08:19:26+08:00 knod4-3-20 systemd[4146297]: Stopped Create User's Volatile Files and Directories.
2026-09-18T08:19:26+08:00 knod4-3-20 systemd[4146297]: Removed slice User Application Slice.
2026-09-18T08:19:26+08:00 knod4-3-20 systemd[4146297]: Reached target Shutdown.
2026-09-18T08:19:26+08:00 knod4-3-20 systemd[4146297]: Finished Exit the Session.
```


not in petronas
```
knod1-1-23,knod1-2-[1,23],knod1-3-[1,23],knod1-4-23,knod1-5-[20-21],knod1-6-[20-21],knod1-7-21,knod1-8-[20-21],knod2-1-23,knod2-2-[22-23],knod2-3-[22-23],knod2-4-21,knod2-5-21,knod2-6-21,knod2-7-[20-21],knod2-8-[20-21],knod3-1-[22-23],knod3-2-23,knod3-3-22,knod3-4-[22-23]
```