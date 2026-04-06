[[lustre]]

* Mount lustre
```
mount -t lustre -o rw,flock kpetronaslustre45:/irfanfs /irfanfs
```

* If mounting lustre caused pc to hang
```
sudo umount -fl /mnt/lustre
```
