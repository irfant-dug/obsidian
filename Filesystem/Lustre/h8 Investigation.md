```
pdsh -g "filesystem=h8" '/bin/unbuffer /bin/iostat -xmd 1 10 | tee -a ${HOSTNAME}_iostat_$(date +%Y%m%d_%H%M%S)'


```