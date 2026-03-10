[[dug]]
```
find /h7/dug/houston/teamlonestar/hilcorp/terrebMPFW_012 -type l | while read l; do r=$(readlink "$l"); if <<<"$r" grep -q ^/h7/dug/houston/teamalamo/hilcorp/terrebMPFW_012; then n=$(echo "$r" | sed 's/^\/h7\/dug\/houston\/teamalamo\/hilcorp\/terrebMPFW_012\//\/h7\/dug\/houston\/teamlonestar\/hilcorp\/terrebMPFW_012\//g'); rm -f "$l"; ln -sfn "$n" "$l"; fi; done
```