[[dug]]

* Change thing in localrepo
* in kpxe0002:
```
cd /pxe/cluster/ && git pull && /pxe/bin/image_cluster 
```

* Unique value for a genders
```
nodeattr -VU BASE
```
* What have this attributes
```
[adm_irfant@hrud3-18-49 ~]$ hostlist -e $(nodeattr -q "BASE=compute7.9") | wc -l
8033
[adm_irfant@hrud3-18-49 ~]$ hostlist -e $(nodeattr -q "BASE=compute9.4-5.14.0-427.35.1.1.dug.el9") | wc -l
2855
```