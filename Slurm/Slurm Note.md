[[Slurm]]

```
$ sinfo -p h200 -o "%D %t"| column -tNODES  STATE3      resv1      mix11     alloc15     idle$ squeue -aw $(sinfo -p h200 -h -o %N) -tr -o "%P %Q %u %j" | sort | uniq -c  | column -tN  PARTITION  PRIORITY  USER                 NAME9  kl         949       amirm                3001_01_MPFWI_Invert_30HzMPInvel_model_44Hz_shallow_pass_GPU_worker1  petronas   100       shafizularieffaba.d  vllmtest1  petronas   100       sharif.rahman        preseis-hpc1  petronas   999       mohameds             6000_9999_Angle25_30_LowerFreq_Handle
```

* Create reservation
```
scontrol create ReservationName="#317966 applying graphite" StartTime=now Nodes="knod1-5-1,knod1-6-1,knod2-6-19,knod2-7-19,knod2-8-19,knod3-1-1,knod3-2-1,knod4-2-19,knod4-5-19" duration=1000000 User=adm_fiqrim FLAGS=MAINTENANCE,IGNORE_JOBS
```

* Delete node from reservation
```
 2. To remove only one node (leaving the reservation active for others):
  You must update the reservation with a new list of nodes that excludes the one you want to remove.

   1 # 1. Get the current list of nodes
   2 scontrol show res "#317966 applying graphite"
   3
   4 # 2. Update the reservation with the new (shortened) list
   5 scontrol update ReservationName="#317966 applying graphite" Nodes="knod1-5-1,knod1-6-1,..."
```

* Cancel Reservation
```

```