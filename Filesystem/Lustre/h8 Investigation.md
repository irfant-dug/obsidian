## Collecting Logs

```
#in it-houston
pdsh -g "filesystem=h8" '/bin/unbuffer /bin/iostat -xmd 1 10 | tee -a ${HOSTNAME}_iostat_$(date +%Y%m%d_%H%M) & /usr/bin/zpool iostat -r 1 10 | tee -a ${HOSTNAME}_zpool_iostat_r_$(date +%Y%m%d_%H%M) & /usr/bin/zpool iostat -v 1 10 | tee -a ${HOSTNAME}_zpool_iostat_v_$(date +%Y%m%d_%H%M) & /usr/bin/zpool iostat -vr 1 10 | tee -a ${HOSTNAME}_zpool_iostat_vr_$(date +%Y%m%d_%H%M)'

#in it-kl adm_irfant
cd ~/stuff/h8
for i in h8dat0003 h8dat0002 h8dat0015 h8dat0009 h8dat0008 h8dat0006 h8dat0010 h8dat0011 h8dat0004 h8dat0016 h8dat0001 h8dat0014 h8dat0013 h8dat0012 h8dat0007 h8dat0005 h8dat0018 h8dat0017 h8met0000 h8dat0019; do rsync -av adm_irfant@$i:~/* .; done

#in it-houston
pdsh -g "filesystem=h8" 'rm ~/* -f'
```

