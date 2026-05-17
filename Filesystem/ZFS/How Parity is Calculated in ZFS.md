* For Raidz3, there are 3 types of parity: P,Q and R. 
* Unlike traditional hardware RAID, ZFS does not have fixed, rigid stripes or dedicated parity disks.
* To balance the input/output load, ZFS rotates which disks hold the data and which hold the parity for every single stripe it writes

**Considering a situation where we have 6-disks RAIDZ3 pool**.
* If you write a large file, ZFS will write a "full" stripe: 4 data blocks + 1 P block + 1 Q block (spanning all 6 disks).
* If you write a tiny piece of data (e.g., a 4KB file), ZFS might only use 1 data block. Because it is RAIDZ3, it _must_ still write 3 parity blocks to maintain redundancy: 1 Data + P + Q
## P Parity
* The simplest of them all
* Calculated using a logical operation called **XOR** (Exclusive OR)
* XOR compares the bits of your data blocks. If there is an odd number of 1s, the parity bit is 1. If there is an even number of 1s (or all 0s), the parity bit is 0.
* $$P = D_1 \oplus D_2 \oplus D_3 \oplus D_4$$

## Q Parity
* ZFS uses **Reed-Solomon coding** over a **Galois Field**
* $$Q = (g^1 \cdot D_1) \oplus (g^2 \cdot D_2) \oplus (g^3 \cdot D_3) \oplus (g^4 \cdot D_4)$$
### PS
* Even when data is only stripe to one disk, parity P,Q and R still will be calculated, causing large parity overhead