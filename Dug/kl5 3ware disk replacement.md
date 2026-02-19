[[dug]]

* If disk scsi ID doesn't change upon disk replacement, run
```
echo 1 > /sys/block/sdx/device/delete
```