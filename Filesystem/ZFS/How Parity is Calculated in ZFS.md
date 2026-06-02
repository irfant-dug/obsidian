* For Raidz3, there are 3 types of parity: P,Q and R. 
* Unlike traditional hardware RAID, ZFS does not have fixed, rigid stripes or dedicated parity disks ().
* To balance the input/output load, ZFS rotates which disks hold the data and which hold the parity for every single stripe it writes

## Considering a situation where we have 8-disks RAIDZ3 pool
* If you write a large file, ZFS will write a "full" stripe: 5 Data + P + Q + R (spanning all 8 disks).
* If you write a tiny piece of data (e.g., a 4KB file), ZFS might only use 1 data block. Because it is RAIDZ3, it _must_ still write 3 parity blocks to maintain redundancy: 1 Data + P + Q + R
### The Array Layout
- **$D_1$:** `0000 0001` 
- **$D_2$:** `1100 0000` 
- **$D_3$:** `0000 0010`
- **$D_4$:** **`0000 0101`** 
- **$D_5$:** **`0000 0011`**
### P Parity
* The simplest of them all
* Calculated using a logical operation called **XOR** (Exclusive OR)
* XOR compares the bits of your data blocks. If there is an odd number of 1s, the parity bit is 1. If there is an even number of 1s (or all 0s), the parity bit is 0.
* $$P = D_1 \oplus D_2 \oplus D_3 \oplus D_4 \oplus D_5$$
```
  0000 0001 (D1) 
  1100 0000 (D2) 
  0000 0010 (D3)
  0000 0101 (D4)
⊕ 0000 0011 (D5)
------------------------ 
  1100 0101 (P Parity)
```

### Q Parity
* ZFS uses **Reed-Solomon coding** over a **Galois Field**
* $$Q = (2^0 \cdot D_1) \oplus (2^1 \cdot D_2) \oplus (2^2 \cdot D_3) \oplus (2^3 \cdot D_4) \oplus (2^4 \cdot D_5)$$
* **D1​ (Shift 0):** `0000 0001`
* **D2​ (Shift 1):**  Shifting `1100 0000` left triggers an overflow, which gives us `1 1000 0000` wrapping it to **`1001 1101`**.
```
  1000 0000 (Shifted Result) 
⊕ 0001 1101 (Polynomial) ------------------ 
  1001 1101 (Final safely wrapped block for D2)
```
- **D3​ (Shift 2):** `0000 0010` becomes **`0000 1000`**.
- **D4​ (Shift 3):** `0000 0101` shifts left safely 3 times to become **`0010 1000`**.
- **D5​ (Shift 4):** `0000 0011` shifts left safely 4 times to become **`0011 0000`**.

```
  0000 0001 (D1) - no shift
  1100 0000 (D2) 
  0000 0010 (D3)
  0000 0010 (D4)
⊕ 0000 0010 (D5)
-----------------------------
  0000 0010 (D5)
```
### PS
* Even when data is only stripe to one disk, parity P,Q and R still will be calculated, causing large parity overhead


