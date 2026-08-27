```
hostlist -e h8dat00[01-19] | while read line; do /d/admin/scripts/lustre/whatHammeringMeV2.sh $line | tee -a job & done; wait
```
```
cat job | awk '{print $6}' | sort |uniq | awk -F '_' '{print $1}' | uniq -c | sort -nr | while read line; do echo $line; cat job | grep "$(echo $line | awk '{print $2}')" | head -1; done
```