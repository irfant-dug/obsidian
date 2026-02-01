[[bash]]

* Replace delimiter except for last line
```
tr '\n' ' ' < afile.txt | sed 's/ $/\n/'
```
