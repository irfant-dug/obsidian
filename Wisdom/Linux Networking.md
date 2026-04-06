[[Linux Networking]]  [[linux]]

![[Pasted image 20260402151833.png|697]]

Client Side
* **Application Memory:** The client application generates data and stores it in user-space memory.
- **System Call:** The application asks the Linux kernel to send the data. This requires a "context switch" (the CPU stops running the app and switches to OS kernel mode).
- **Memory Copy 1:** The CPU physically copies the data from the user-space buffer into the kernel's Socket Buffer (`sk_buff`).
- **Protocol Processing:** The CPU calculates TCP sequences, adds IP routing headers, and adds Ethernet frames.
- **Hardware Handoff:** The CPU tells the NIC's DMA engine to pull the packet from the kernel buffer and send it over the wire.

Server Side
* **Hardware Reception:** The server NIC receives the packet and uses DMA to write it into the RX Ring Buffer in RAM.
  
  Interrupt Coalescing: When the first packet of a burst arrives, the NIC's DMA engine quietly writes it into your server's RAM. Instead of immediately tapping the CPU on the shoulder, the NIC starts a timer and a counter.

  The CPU acknowledges the interrupt, wakes up the `ksoftirqd` process, and instantly **tells the NIC to disable all future interrupts for that specific queue.** The CPU keeps polling until it completely empties the RX Ring Buffer (or hits a budget limit)

* **Hardware Interrupt:** The NIC sends an interrupt to the CPU.
* **Kernel Processing:** The CPU wakes up (`ksoftirqd`), reads the data from the ring buffer, strips the Ethernet/IP/TCP headers, and verifies checksums. _(This is where your `rx_dropped` bottleneck is currently happening).
* **Memory Copy 2:** The CPU physically copies the naked payload from the kernel's socket buffer into the Lustre application's user-space buffer.
* **Storage I/O:** The Lustre daemon finally has the data and writes it to the backend disk (OST)

***Non-DMA***
*  

