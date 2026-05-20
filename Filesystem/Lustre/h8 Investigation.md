## Collecting Logs

```
#run in it-houston

pdsh -w "it-kl" 'mkdir -p ~/stuff/h8/h8_temp';
pdsh -g "filesystem=h8" '/bin/unbuffer /bin/iostat -xmd 1 10 | tee -a ${HOSTNAME}_iostat_$(date +%Y%m%d_%H%M) & /usr/sbin/zpool iostat -r 1 10 | tee -a ${HOSTNAME}_zpool_iostat_r_$(date +%Y%m%d_%H%M) & /usr/sbin/zpool iostat -v 1 10 | tee -a ${HOSTNAME}_zpool_iostat_v_$(date +%Y%m%d_%H%M) & /usr/sbin/zpool iostat -vr 1 10 | tee -a ${HOSTNAME}_zpool_iostat_vr_$(date +%Y%m%d_%H%M); /bin/rsync -av -e "ssh -o StrictHostKeyChecking=no" ~/* adm_irfant@it-kl:~/stuff/h8/h8_temp; rm * -f; '; 
pdsh -w "it-kl" 'ls -1 ~/stuff/h8/h8_temp/ | head -1 | awk -F '_' "{print \$(NF-1),\$NF}" | while read line; do echo $line | tr " " "_" | xargs -I {} mv ~/stuff/h8/h8_temp ~/stuff/h8/{}; done';


```