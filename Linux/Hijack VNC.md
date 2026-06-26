```
❯ [adm_irfant@hrud3-17-39 ~]$ loginctl
  SESSION  UID USER             SEAT  TTY    STATE   IDLE SINCE
    23040 4530 adm_irfant             pts/11 active  no
       73 3674 rockwave_connorb       n/a    closing no
       c2  993 lightdm          seat0 n/a    active  no
```
```
loginctl show-session 73
ps -u rockwave_connorb -o pid,stat,cmd --no-header | head -30

❯ [adm_irfant@hrud3-17-39 ~]$ loginctl show-session 73
  ps -u rockwave_connorb -o pid,stat,cmd --no-header | head -30
  Id=73
  User=3674
  Name=rockwave_connorb
  Timestamp=Wed 2026-03-18 03:56:42 CDT
  TimestampMonotonic=34845675224
  VTNr=0
  Remote=yes
  RemoteHost=127.0.0.1
  Service=sshd
  Scope=session-73.scope
  Leader=510425
  Audit=73
  Type=tty
  Class=user
  Active=yes
  State=closing
  IdleHint=no
  IdleSinceHint=0
  IdleSinceHintMonotonic=0
  LockedHint=no
   236002 Ss+  bash
   267885 Sl+  /d/sw/azStorageExplorer/1.38.0/StorageExplorerExe /d/sw/azStorageExplorer/1.38.0/resources/app/out/app/node/NodeProcessHostProxy @storage-explorer/activity-log --nodeModuleRoots=/d/sw/azStorageExplorer/1.38.0/resources,/d/sw/azStorageExplorer/1.38.0/resources/app/node_modules/@storage-explorer/activity-log
   269003 Sl+  /d/sw/azStorageExplorer/1.38.0/StorageExplorerExe --type=renderer --enable-crash-reporter=a8f25d67-8f73-4df2-a673-05997797fc84,no_channel --user-data-dir=/d/home/rockwave/rockwave_connorb/.config/StorageExplorer --app-path=/d/sw/azStorageExplorer/1.38.0/resources/app --no-sandbox --no-zygote --enable-blink-features --disable-blink-features --no-sandbox --js-flags=--expose_gc --disable-gpu-compositing --lang=en-US --num-raster-threads=4 --enable-main-frame-before-activation --renderer-client-id=17 --time-ticks-at-unix-epoch=-1773789356585517 --launch-time-ticks=6277816089225 --shared-files=v8_context_snapshot_data:100 --field-trial-handle=3,i,16956950519438549185,10961542021928582262,262144 --disable-features=SpareRendererForSitePerProcess --variations-seed-version
   510576 Ss   /usr/lib/systemd/systemd --user
   510577 S    (sd-pam)
   510666 Ss   /usr/bin/perl /d/sw/fastx3/3.1.40/lib/fastx/3/scripts/link --json=%7B%22daemon%22%3Atrue%2C%22url%22%3A%22http%3A%2F%2Flocalhost%3A3300%2Flocal%2Flink%22%7D
   511364 Ss   /usr/bin/dbus-broker-launch --scope user
   511365 S    dbus-broker --log 4 --controller 11 --machine-id 32d3b64016974fb690332af0a8f54ecb --max-bytes 100000000000000 --max-fds 25000000000000 --max-matches 5000000000
   511366 Ssl  /usr/libexec/at-spi-bus-launcher
   511371 S    /usr/bin/dbus-broker-launch --config-file=/usr/share/defaults/at-spi2/acces
   511372 S    dbus-broker --log 4 --controller 9 --machine-id 32d3b64016974fb690332af0a8f54ecb --max-bytes 100000000000000 --max-fds 6400000 --max-matches 5000000000
   511379 Ssl  /usr/libexec/gvfsd
   511384 Sl   /usr/libexec/gvfsd-fuse /run/user/3674/gvfs -f
   511458 Ssl  /usr/libexec/dconf-service
   511463 SLl  gnome-keyring-daemon --start
   511514 Ssl  /usr/bin/pipewire
   511516 Ssl  /usr/bin/wireplumber
   511518 Ssl  /usr/bin/pipewire-pulse
   511741 Sl   /usr/libexec/geoclue-2.0/demos/agent
   511746 Ssl  /usr/libexec/gvfs-udisks2-volume-monitor
   511794 Ssl  /usr/libexec/xdg-desktop-portal
   511802 Ssl  /usr/libexec/xdg-document-portal
   511803 Ssl  /usr/libexec/gvfs-gphoto2-volume-monitor
   511813 Ssl  /usr/libexec/xdg-permission-store
   511827 Ssl  /usr/libexec/gvfs-mtp-volume-monitor
   511851 Sl   /usr/libexec/gvfsd-trash --spawner :1.10 /org/gtk/gvfs/exec_spaw/0
   512042 Ssl  /usr/libexec/gvfsd-metadata
   542106 S    /bin/bash /d/sw/Insight/6.1-512241/_insight -Ddug.launcher.pid=783034 --memc_rwave/2025_0291_DOGGER_BANK_3DUUHRS_REPRO/2025_0291_DOGGER_BANK_3D_PROD_01
  -Dsun.java2d.uiScale.enabled=true -Dsun.java2d.uiScale=3
   542301 S    /bin/bash /d/sw/Insight/6.1-512241/_insight -Ddug.launcher.pid=783034 --memMax=50000 -DprojectDir=/h7/mcc_rwave/2025_0291_DOGGER_BANK_3DUUHRS_REPRO/2025_0291_DOGGER_BANK_3D_PROD_01 -Dsun.java2d.uiScale.enabled=true -Dsun.java2d.uiScale=3
   542302 S    /bin/bash /d/sw/Insight/6.1-512241/_insight -Ddug.launcher.pid=783034 --memc_rwave/2025_0291_DOGGER_BANK_3DUUHRS_REPRO/2025_0291_DOGGER_BANK_3D_PROD_01-Dsun.java2d.uiScale.enabled=true -Dsun.java2d.uiScale=3
   
   
❯ [adm_irfant@hrud3-17-39 ~]$ sudo cat /proc/542106/environ | tr '\0' '\n' | grep -E 'DISPLAY|XAUTHORITY'
  XAUTHORITY=/d/home/rockwave/rockwave_connorb/.fastx_server/hrud3-17-39/sessions/FX3-181c115227bf1439727e593f51910916/authority
  DISPLAY=hrud3-17-39:101
  
sudo -u rockwave_connorb x11vnc \
  -display :101 \
  -auth /d/home/rockwave/rockwave_connorb/.fastx_server/hrud3-17-39/sessions/FX3-181c115227bf1439727e593f51910916/authority \
  -rfbport 5902 \
  -localhost -forever -shared -nopw
  
  ❯ [adm_irfant@it-kl ~]$ ssh -L 5902:localhost:5902 adm_irfant@hrud3-17-39
  ● BOOTED=2026-03-17T18:15:57 RELEASE=9.4 IMAGE=desktop9.4-5.14.0-427.35.1.1.dug.el9(20250826090718) TEMPLATE=hcas9
  Last login: Fri Jun 26 11:10:33 2026 from 172.18.255.61
  [adm_irfant@hrud3-17-39 ~]$
  
  ❯ [adm_irfant@it-kl ~]$ vncviewer localhost:5902
  libjawt.so path: /d/sw/java64/jdk-9.0.4/lib/amd64
  CConn: connected to host localhost port 5902
  Connection closed
```