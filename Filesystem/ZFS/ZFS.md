
**Recordsize**
* In ZFS, `recordsize` is the maximum logical size of a block of data. When ZFS writes a file, it bundles the data into a block matching your `recordsize` setting.
* zfs reads and write on record basis. To read something, it will read the entire record (write amplification).
* Higher recordsize will result in smaller amount of block pointers (metadata)
* Good recordsize will

**Parity**
* Parity is calculated on stripe basis, including an uncomplete stripe (stripe that doesn't involve all disks in a pool)

**Padding
* 

