[[pxe]]

`option pxelinux.magic f1:00:74:7e;`
- Earlier versions of PXELINUX required this to be set to F1:00:74:7E (241.0.116.126) for PXELINUX to recognize any special DHCP options whatsoever.  As of PXELINUX 3.55, this option is deprecated and is no longer required.

```
site-option-space "pxelinux";
```
* By using site-option-space, administrators can efficiently manage and apply distinct DHCP configurations based on the requirements of different sites
  
```
use-host-decl-names on;
```
* the name provided for the host declaration will be supplied to the client as its hostname.
  
```
*     group {
      use-host-decl-names on;

     host joe {
        hardware ethernet 08:00:2b:4c:29:32;
        fixed-address joe.fugue.com;
      }
    }

is equivalent to

     host joe {
        hardware ethernet 08:00:2b:4c:29:32;
        fixed-address joe.fugue.com;
        option host-name "joe";
      }
```
```
filename "dugboot-2024.02.1-dug.5/efi64/grubx64.efi";
```
* Bootloader file pull using TFTP
```
next-server 172.18.255.8;
```
* TFTP server

```
option pxelinux.configfile "desktop9.4-5.14.0-427.35.1.1.dug.el9  
/pxemenu_kul"
```
* Pxemenu location