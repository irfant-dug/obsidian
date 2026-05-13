[[Slurm]] [[dug]]

1. Do dynamic change using scontrol
2. Choose a remote node. It must have localData, so that the file is accessible by root. SSH into that node
3. Run Debug to get the current Slurm configuration along with dynamic change
```
sudo DEBUG=1 /d/sw/slurm/etc/sanity_slurm_conf.sh && sudo mv $SLURM_FILE.* $SLURM_FILE; sudo chmod 644 $
```

```
[adm_irfant@kud46 slurm_temp]$ sudo DEBUG=1 /d/sw/slurm/etc/sanity_slurm_conf.sh
[adm_irfant@kud46 slurm_temp]$ sudo mv $SLURM_FILE.* $SLURM_FILE                   
[adm_irfant@kud46 slurm_temp]$ ls                                                  slurm_kl.conf
[adm_irfant@kud46 slurm_temp]$ sudo chmod 644 $SLURM_FILE                          
[adm_irfant@kud46 slurm_temp]$ sudo chown root:root $SLURM_FILE

```