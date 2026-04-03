[[RDMA]] [[Linux Networking]]

**How RDMA handle packet loss**
* If the application level does not provide transport layer functionality, RDMA mode called **RC (Reliable Connection)** will be used

1. **Reliable Connection** - A TCP-like functionality for RDMA
* Packet Sequence Numbers (PSN):** The sending NIC injects a hardware-level sequence number into every RDMA packet header.
* ***Hardware ACKs:** When the receiving NIC gets the packet, its silicon automatically generates an Acknowledgment (ACK) packet and shoots it back to the sender. The OS kernel never sees this happen.
*  **Hardware Buffering:** The sending NIC keeps a copy of the transmitted packet in its own memory until it receives the hardware ACK.


2. **Go-back-N retransmission**
```
Time    Sender (Lustre Client)                  Receiver (Server eth2)
 |      |                                       |
 |      |------- Packet #98 ------------------->| (Writes to RAM)
 |      |<------ ACK #98 -----------------------|
 |      |                                       |
 |      |------- Packet #99 ------------------->| (Writes to RAM)
 |      |<------ ACK #99 -----------------------|
 |      |                                       |
 |      |------- Packet #100 ------[DROPPED X]  | (Never arrives!)
 |      |                                       |
 |      |------- Packet #101 ------------------>| (Arrives! But wait...)
 |      |                                       | ---> ERROR: Expected #100!
 |      |<------ NACK #100 ---------------------| ---> Drops #101. Stops writing.
 |      |                                       |
 |      |------- Packet #102 ------------------>| ---> Drops #102.
 |      |------- Packet #103 ------------------>| ---> Drops #103.
 |      |                                       |
 |    (Receives NACK)                           |
 |    (Rewinds Hardware Buffer)                 |
 |      |                                       |
 |      |------- Packet #100 (Retransmit) ----->| (Writes to RAM)
 |      |------- Packet #101 (Retransmit) ----->| (Writes to RAM)
 |      |------- Packet #102 (Retransmit) ----->| (Writes to RAM)
 |      |------- Packet #103 (Retransmit) ----->| (Writes to RAM)
 V      |                                       |
```

3. **Flow Control and ECN**
* Because RDMA uses that aggressive "Go-Back-N" retransmission strategy, **packet loss is absolutely devastating to RDMA performance.**
* **PFC (Priority Flow Control):** If a switch port is getting congested and its buffer is about to overflow, it sends a hardware "Pause Frame" back down the cable to the server's NIC. It tells the NIC hardware, _"Stop transmitting for exactly 3 microseconds, or I will be forced to drop a packet."_ The NIC instantly pauses.
* **ECN (Explicit Congestion Notification) & DCQCN:** Switches mark packets that are traversing congested routes. When the receiving NIC sees this mark, it sends a message to the transmitting NIC to proactively slow down its sending rate _before_ the switch has to use a Pause Frame.