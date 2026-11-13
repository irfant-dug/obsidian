```
**Disable the Onboard LAN Ports:**

- Arrow down to **`Load Onboard LAN 1 Option ROM`** $\rightarrow$ change from `[Enabled]` to **`[Disabled]`**.
    
- Arrow down to **`Load Onboard LAN 2 Option ROM`** $\rightarrow$ change from `[Enabled]` to **`[Disabled]`**.
  
- Notice that **`Above 4G Decoding`** is already set to **`[Disabled]`**—leave it disabled to prevent the `PXE-EC8` error.
    
- Ensure **`CPU1 Slot1 PCI-E 3.0 x16 OPROM`** and **`CPU1 Slot5 PCI-E 3.0 x8 OPROM`** remain **`[Enabled]`**.
  - **`Network stack`:** Keep **`[Disabled]`**.
    
- **`Launch Storage OpROM policy`:** Keep **`[Legacy only]`**.
```