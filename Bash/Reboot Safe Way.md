[[bash]]

```
sudo sh -c 'echo 1 > /proc/sys/kernel/panic; echo 1 > /proc/sys/kernel/sysrq; echo b > /proc/sysrq-trigger'
```