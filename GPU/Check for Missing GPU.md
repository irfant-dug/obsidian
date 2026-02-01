[[gpu]]
* Show serial number and missing GPU
```
pdsh -g "H200&&office=houston" 'declare -A BUS_TO_IPMI=( ["01:00.0"]="GPU11" ["11:00.0"]="GPU12" ["61:00.0"]="GPU10" ["71:00.0"]="GPU9" ["81:00.0"]="GPU3" ["91:00.0"]="GPU4" ["e1:00.0"]="GPU2" ["f1:00.0"]="GPU1" );bus_ids="$(nvidia-smi -q | grep -iE "^ +serial|bus id")";echo "${bus_ids}" ; for bus in "${!BUS_TO_IPMI[@]}"; do if ! echo "${bus_ids}" | grep -qi $bus; then echo "${BUS_TO_IPMI[$bus]} is missing" ;fi ; done;' | dshbak -c
```

* Show only missing GPU
```
pdsh -g "H200&&office=houston" 'declare -A BUS_TO_IPMI=( ["01:00.0"]="GPU11" ["11:00.0"]="GPU12" ["61:00.0"]="GPU10" ["71:00.0"]="GPU9" ["81:00.0"]="GPU3" ["91:00.0"]="GPU4" ["e1:00.0"]="GPU2" ["f1:00.0"]="GPU1" );bus_ids="$(nvidia-smi --query-gpu=pci.bus_id)"; for bus in "${!BUS_TO_IPMI[@]}"; do if ! echo "${bus_ids}" | grep -qi $bus; then echo "${BUS_TO_IPMI[$bus]} is missing" ;fi ; done;' | dshbak -c
```
