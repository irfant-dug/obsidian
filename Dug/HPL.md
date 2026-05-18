## Run HPL on many H200 Nodes
```
cd /data/kl7/dug/IT/h200_hpl/ultimate
sbatch --nodelist=knod1-1-1,knod2-7-19,knod3-3-1,knod4-4-19 --ntasks=32 ultimate_hpl_h200.job
```