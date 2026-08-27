[[Lustre]]

https://lwn.net/Articles/1025268/
https://jira.whamcloud.com/browse/LU-11554
https://www.youtube.com/watch?v=VRR4ukGb19w
https://doc.lustre.org/lustre_manual.xhtml#TuningClientReadahead

A bit of background on LSoM
* There is two syscall used by Linux to get file information (eg. size, block, ownership): 1. stat() and 2. statx(). Kernels 4.10 and later have used advanced statx() interface that can specify flag/bitmask to fetch attributes of files. 
* Almost all utils related to file (stat, ls, du) used statx().
* 

Why ls -l is slow


```
sudo strace -f -p "$(pgrep -x nautilus)"   -e trace=openat,read,statx  -y -T -ttt -s 256 -o /tmp/naut.trace
```
```
sudo sh -c 'echo 3 > /proc/sys/vm/drop_caches'
```
```
strace -c -w -e trace=statx ls -l /lustest/ost0/30K >/dev/null
```
```
strace -T -e trace=statx ls -l /lustest/ost0/30K/file.9999 >/dev/null
```