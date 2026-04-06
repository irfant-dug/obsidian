[[ZFS]]

```
[adm_irfant@kpetronaslustre47 ~]$ sudo zdb -Lbbbs lustre

Traversing all blocks ...

 244T completed (50241MB/s) estimated time remaining: 0hr 00min 01sec
        bp count:             848906351
        ganged count:                 0
        bp logical:      281926961099776      avg: 332106
        bp physical:     220824437767680      avg: 260128     compression:   1.28
        bp allocated:    268567283695616      avg: 316368     compression:   1.05
        bp deduped:                   0    ref>1:      0   deduplication:   1.00
        Normal class:    268567271292928     used: 61.04%
        Embedded log class              0     used:  0.00%

        additional, non-pointer bps of type 0:  110092452
         number of (compressed) bytes:  number of bps
                         17:      3 *
                         18:      3 *
                         19:      0
                         20:      1 *
                         21:      0
                         22:      0
                         23:      0
                         24:      0
                         25:      0
                         26:      0
                         27:      0
                         28:      2 *
                         29:     29 *
                         30:  83558 *
                         31: 151145 *
                         32:  21018 *
                         33: 42011607 ****************************************
                         34:  85152 *
                         35: 170424 *
                         36: 1194791 **
                         37: 718668 *
                         38: 1576849 **
                         39: 828748 *
                         40: 478484 *
                         41: 1430966 **
                         42: 502913 *
                         43: 271018 *
                         44: 276989 *
                         45: 410039 *
                         46: 305819 *
                         47: 1158795 **
                         48: 258443 *
                         49: 426650 *
                         50: 359170 *
                         51: 574085 *
                         52: 134564 *
                         53: 234964 *
                         54: 232679 *
                         55: 208418 *
                         56: 259504 *
                         57: 242864 *
                         58: 223829 *
                         59: 267368 *
                         60: 140498 *
                         61: 142066 *
                         62: 188096 *
                         63: 198154 *
                         64: 203877 *
                         65: 316017 *
                         66: 216594 *
                         67: 386088 *
                         68: 644241 *
                         69: 472170 *
                         70: 199311 *
                         71: 270572 *
                         72: 451168 *
                         73: 676009 *
                         74: 622423 *
                         75: 460342 *
                         76: 202472 *
                         77: 168630 *
                         78: 549267 *
                         79: 1926673 **
                         80: 1257538 **
                         81: 2571716 ***
                         82: 1107450 **
                         83: 440798 *
                         84: 388022 *
                         85: 933573 *
                         86: 811230 *
                         87: 3210815 ****
                         88: 1398859 **
                         89: 1097728 **
                         90: 645739 *
                         91: 869597 *
                         92: 680300 *
                         93: 439521 *
                         94: 521055 *
                         95: 1421541 **
                         96: 1345175 **
                         97: 553830 *
                         98: 932598 *
                         99: 1128352 **
                        100: 2663018 ***
                        101: 881730 *
                        102: 2444402 ***
                        103: 916006 *
                        104: 3335608 ****
                        105: 1303269 **
                        106: 647008 *
                        107: 2939895 ***
                        108: 1031753 *
                        109: 5528342 ******
                        110: 1821782 **
                        111: 764997 *
                        112: 524978 *
        Dittoed blocks on same vdev: 3048655

Blocks  LSIZE   PSIZE   ASIZE     avg    comp   %Total  Type
     -      -       -       -       -       -        -  unallocated
     2    32K      8K     96K     48K    4.00     0.00  object directory
     1   128K     28K    144K    144K    4.57     0.00      L1 object array
   401   200K    200K   18.8M   47.9K    1.00     0.00      L0 object array
   402   328K    228K   18.9M   48.1K    1.44     0.00  object array
     1    16K      4K     48K     48K    4.00     0.00  packed nvlist
     -      -       -       -       -       -        -  packed nvlist size
     -      -       -       -       -       -        -  bpobj
     -      -       -       -       -       -        -  bpobj header
     -      -       -       -       -       -        -  SPA space map header
 22.8K   364M     91M   1.07G     48K    4.00     0.00      L1 SPA space map
 97.6K  12.2G   6.92G   28.5G    299K    1.76     0.01      L0 SPA space map
  120K  12.6G   7.01G   29.6G    252K    1.79     0.01  SPA space map
     -      -       -       -       -       -        -  ZIL intent log
     2   256K      8K     64K     32K   32.00     0.00      L5 DMU dnode
     2   256K      8K     64K     32K   32.00     0.00      L4 DMU dnode
     2   256K      8K     64K     32K   32.00     0.00      L3 DMU dnode
     3   384K     16K    144K     48K   24.00     0.00      L2 DMU dnode
    78  9.75M   4.01M   11.3M    149K    2.43     0.00      L1 DMU dnode
 70.5K  1.10G    313M   2.52G   36.6K    3.60     0.00      L0 DMU dnode
 70.6K  1.11G    317M   2.53G   36.7K    3.59     0.00  DMU dnode
     3    12K     12K    112K   37.3K    1.00     0.00  DMU objset
     -      -       -       -       -       -        -  DSL directory
     -      -       -       -       -       -        -  DSL directory child map
     -      -       -       -       -       -        -  DSL dataset snap map
     6    34K   8.50K    144K     24K    4.00     0.00  DSL props
     -      -       -       -       -       -        -  DSL dataset
     -      -       -       -       -       -        -  ZFS znode
     -      -       -       -       -       -        -  ZFS V0 ACL
   242  30.2M    980K   7.59M   32.1K   31.61     0.00      L3 ZFS plain file
 48.8K  6.10G    231M   1.61G   33.9K   27.06     0.00      L2 ZFS plain file
 1.34M   172G   34.8G    117G   87.0K    4.93     0.05      L1 ZFS plain file
  807M   256T    201T    244T    310K    1.28    99.92      L0 ZFS plain file
  808M   256T    201T    244T    309K    1.28    99.97  ZFS plain file
   340  42.5M   1.70M   11.6M   35.0K   24.95     0.00      L1 ZFS directory
 8.67K   139M   66.8M    534M   61.7K    2.08     0.00      L0 ZFS directory
 9.00K   181M   68.5M    546M   60.7K    2.64     0.00  ZFS directory
     2     1K      1K     64K     32K    1.00     0.00  ZFS master node
     -      -       -       -       -       -        -  ZFS delete queue
     -      -       -       -       -       -        -  zvol object
     -      -       -       -       -       -        -  zvol prop
     -      -       -       -       -       -        -  other uint8[]
     -      -       -       -       -       -        -  other uint64[]
     -      -       -       -       -       -        -  other ZAP
     -      -       -       -       -       -        -  persistent error log
     1   128K      4K     48K     48K   32.00     0.00  SPA history
     -      -       -       -       -       -        -  SPA history offsets
     -      -       -       -       -       -        -  Pool properties
     -      -       -       -       -       -        -  DSL permissions
     -      -       -       -       -       -        -  ZFS ACL
     -      -       -       -       -       -        -  ZFS SYSACL
     -      -       -       -       -       -        -  FUID table
     -      -       -       -       -       -        -  FUID table size
     -      -       -       -       -       -        -  DSL dataset next clones
     -      -       -       -       -       -        -  scan work queue
     6  52.5K     16K     96K     16K    3.28     0.00  ZFS user/group/project used
     -      -       -       -       -       -        -  ZFS user/group/project quota
     -      -       -       -       -       -        -  snapshot refcount tags
     -      -       -       -       -       -        -  DDT ZAP algorithm
     -      -       -       -       -       -        -  DDT statistics
 1.32M   677M    677M   42.3G     32K    1.00     0.02  System attributes
     -      -       -       -       -       -        -  SA master node
     2     3K      3K     64K     32K    1.00     0.00  SA attr registration
     4    64K     16K    128K     32K    4.00     0.00  SA attr layouts
     -      -       -       -       -       -        -  scan translations
     -      -       -       -       -       -        -  deduplicated block
     -      -       -       -       -       -        -  DSL deadlist map
     -      -       -       -       -       -        -  DSL deadlist map hdr
     -      -       -       -       -       -        -  DSL dir clones
     -      -       -       -       -       -        -  bpobj subobj
     -      -       -       -       -       -        -  deferred free
     -      -       -       -       -       -        -  dedup ditto
     1   128K      8K     64K     64K   16.00     0.00      L1 other
   134   420K     45K   1.39M   10.6K    9.33     0.00      L0 other
   135   548K     53K   1.45M   11.0K   10.34     0.00  other
     2   256K      8K     64K     32K   32.00     0.00      L5 Total
     2   256K      8K     64K     32K   32.00     0.00      L4 Total
   244  30.5M    988K   7.66M   32.1K   31.61     0.00      L3 Total
 48.8K  6.10G    231M   1.61G   33.9K   27.06     0.00      L2 Total
 1.36M   172G   34.9G    118G   86.4K    4.93     0.05      L1 Total
  808M   256T    201T    244T    309K    1.28    99.95      L0 Total
  810M   256T    201T    244T    309K    1.28   100.00  Total

Block Size Histogram

  block   psize                lsize                asize
   size   Count   Size   Cum.  Count   Size   Cum.  Count   Size   Cum.
    512:  1.32M   677M   677M  1.32M   677M   677M      0      0      0
     1K:      5  7.50K   677M      5  7.50K   677M      0      0      0
     2K:      1     2K   677M      1     2K   677M      0      0      0
     4K:   440M  1.72T  1.72T   437M  1.71T  1.71T      0      0      0
     8K:  6.73M  60.9G  1.78T  2.16M  17.3G  1.73T      0      0      0
    16K:  5.21M   109G  1.89T  1.52M  24.4G  1.75T   440M  6.87T  6.87T
    32K:  5.43M   214G  2.09T  5.01M   160G  1.91T  16.1M   597G  7.45T
    64K:  5.18M   446G  2.53T  1.48M  94.7G  2.00T  5.43M   447G  7.89T
   128K:  7.08M  1.31T  3.84T  1.91M   244G  2.24T  7.07M  1.23T  9.12T
   256K:  15.9M  5.85T  9.69T   294K  73.6G  2.31T  13.0M  4.77T  13.9T
   512K:  93.6M  67.3T  77.0T   153K  76.6G  2.38T  83.3M  67.1T  81.0T
     1M:   124M   124T   201T   254M   254T   256T   140M   163T   244T
     2M:      0      0   201T      0      0   256T      0      0   244T
     4M:      0      0   201T      0      0   256T      0      0   244T
     8M:      0      0   201T      0      0   256T      0      0   244T
    16M:      0      0   201T      0      0   256T      0      0   244T

                            capacity   operations   bandwidth  ---- errors ----
description                used avail  read write  read write  read write cksum
lustre                     244T  156T 1.67K     0 7.17M     0     0     0     0
  raidz3                   244T  156T 1.67K     0 7.17M     0     0     0     0
    /dev/disk/by-id/dm-uuid-mpath-35000cca2c05d772c                  88     0  387K     0     0     0     0
    /dev/mapper/mpatho                   79     0  340K     0     0     0     0
    /dev/disk/by-id/dm-uuid-mpath-35000cca2c05e6a6c                  64     0  268K     0     0     0     0
    /dev/disk/by-id/dm-uuid-mpath-35000cca2c05e9f98                 108     0  475K     0     0     0     0
    /dev/mapper/mpathp                   88     0  388K     0     0     0     0
    /dev/disk/by-id/dm-uuid-mpath-35000cca2c05ebdf4                  79     0  340K     0     0     0     0
    /dev/disk/by-id/dm-uuid-mpath-35000cca2c05ec160                  64     0  269K     0     0     0     0
    /dev/disk/by-id/dm-uuid-mpath-35000cca2c05ec2e0                 108     0  475K     0     0     0     0
    /dev/disk/by-id/dm-uuid-mpath-35000cca2c05ec3bc                  88     0  388K     0     0     0     0
    /dev/disk/by-id/dm-uuid-mpath-35000cca2c05ec460                  80     0  341K     0     0     0     0
    /dev/disk/by-id/dm-uuid-mpath-35000cca2c05ec5ec                  64     0  268K     0     0     0     0
    /dev/disk/by-id/dm-uuid-mpath-35000cca2c05ec730                 108     0  474K     0     0     0     0
    /dev/disk/by-id/dm-uuid-mpath-35000cca2c05ec750                  88     0  387K     0     0     0     0
    /dev/disk/by-id/dm-uuid-mpath-35000cca2c05ec794                  79     0  340K     0     0     0     0
    /dev/disk/by-id/dm-uuid-mpath-35000cca2c05ecae8                  64     0  267K     0     0     0     0
    /dev/mapper/mpathn                  108     0  474K     0     0     0     0
    /dev/disk/by-id/dm-uuid-mpath-35000cca2c05ede0c                  88     0  386K     0     0     0     0
    /dev/disk/by-id/dm-uuid-mpath-35000cca2c05edea8                  79     0  339K     0     0     0     0
    /dev/disk/by-id/dm-uuid-mpath-35000cca4128b3e7c                  64     0  267K     0     0     0     0
    /dev/disk/by-id/dm-uuid-mpath-35000cca412975b60                 108     0  473K     0     0     0     0
```