[[bash]]

* Replace delimiter except for last line
```
tr '\n' ' ' < afile.txt | sed 's/ $/\n/'
```

* Print from match to match
```
sed -n "/$line /,/mpath/p"
```