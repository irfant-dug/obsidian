[[dug]]

* Get 
```
ls -1 *gpu_temperature* | sort -V | awk -F '_' '{print $1}' | uniq | while read line; do ls -1 $line*gpu_temperature* | tr '\n' ' ' | sed 's/ $/\n/' | while read line; do echo $line | awk -F '_' '{print $1}';  which_gpu_is_throttling.sh $line; echo; done; done
```