[[How RDMA handle packet loss]]  [[Linux Networking]] 

* RDMA is an application-layer (user-space) functionality, in a sense that the application prepare for RDMA
* In the context of Lustre, LNet use ko2iblnd.ko (or just called o2ib) module for RDMA networks. In normal network, LNet uses socklnd, a standard TCP driver
* 

**On the Client Side:**

1. **Application Memory:** The client application generates data in user-space memory.
2. **Hardware Handoff (Bypassing Kernel):** The Lustre client application talks _directly_ to the physical NIC hardware, passing it the memory keys and the data's location. **No context switch. No kernel involvement. No CPU memory copy.**

	With RoCEv2, the InfiniBand/RDMA data is encapsulated inside standard networking protocols so that it can travel across your regular Ethernet network. **the NIC's silicon chip does all the packaging**, completely bypassing the server's CPU and Linux kernel.

	 The NIC wraps the RDMA package inside a standard UDP header, and subsequently into IPv4/IPv6 header

3. **Direct Transfer:** The client NIC's DMA engine pulls the data straight from user-space memory, adds RDMA hardware routing headers, and fires it over the wire.

**On the Server Side :** 

4. **Hardware Reception & Validation:** The server NIC receives the data and checks the RDMA hardware headers to verify the memory keys are valid. 
5. **Zero-Copy Placement (The Magic Step):** The server NIC's DMA engine takes the payload and writes it **directly into the Lustre application's user-space buffer in RAM**. It completely skips the RX Ring Buffer, the socket buffers, and the Linux kernel. 
6. **Lightweight Notification:** Once the entire block of data is fully written into the application's RAM, the NIC sends a single lightweight notification to the Lustre daemon saying, _"Your data is ready."_ 
7. **Storage I/O:** Lustre writes the data to the backend disk.