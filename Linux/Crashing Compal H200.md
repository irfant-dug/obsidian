```
V=172.18.255.20
N=${1:-16}
for i in $(seq 1 $N); do
  mkdir -p /mnt/r$i
  # odd loops WALK (no nosharecache); even loops KILL (private sb, guaranteed last ref)
  if [ $((i % 2)) -eq 0 ]; then O=ro,vers=4.2,nosharecache; else O=ro,vers=4.2; fi
  ( while :; do
      mount -t nfs4 -o $O $V:/epic2/sw /mnt/r$i 2>/dev/null
      umount /mnt/r$i 2>/dev/null
    done ) &
done
wait
```