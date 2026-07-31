# Cisco Clock Rate Command

## ⏰ Cisco `clock rate` Command Explained

The Cisco **`clock rate`** command is a **physical layer timing control** parameter used exclusively on **serial interfaces**.

---

### What it Does

It sets the **actual physical transmission speed** (in bits per second, bps) of the serial link by generating a **clocking signal**.

- This signal tells both the sending and receiving devices on the link how fast to transmit and sample the data (i.e., when one bit ends and the next begins), ensuring synchronization.
    
- **Crucially, this command only works on the DCE side** (Data Communications Equipment) of the serial cable.
    

> The `clock rate` command directly determines the maximum transmission capacity of the serial line, unlike the `bandwidth` command, which is informational.

---

### How and Why it is Used

1. **Setting the Physical Link Speed (Synchronization)**:
    
    - The command is used to physically control the data rate on the wire. If you set `clock rate 64000`, the maximum speed of the link is fixed at 64 Kbps.
        

- In a **real-world WAN** connection, the **Service Provider's equipment** (e.g., a CSU/DSU) is the DCE and provides the clocking signal. The customer's router is the **DTE** (Data Terminal Equipment) and **does not** use the `clock rate` command.
    
- - The command is primarily used in **lab or back-to-back router setups** where one router must be manually configured as the DCE to supply the necessary clock signal.
        
- **DCE vs. DTE Roles**:
    
    - **DCE (Data Communications Equipment)**: Provides the clocking signal and is the side where the `clock rate` command is configured.
        

- **DTE (Data Terminal Equipment)**: Receives the clocking signal from the DCE side and adjusts its timing accordingly.
    

- **Relation to `bandwidth`**:
    
    - Setting the `clock rate` on the DCE side sets the **actual physical speed**.
        

- The **`bandwidth`** command on _both_ sides (DCE and DTE) should be manually configured to match this physical speed. This is necessary because the default `bandwidth` (e.g., 1544 Kbps for a T1) often does not match the actual subscribed serial speed, leading to incorrect routing metric calculations by EIGRP or OSPF.
    

---

You can see a demonstration of configuring the clock rate and the difference between DTE and DCE interfaces in this video: [What is Clock Rate in Cisco Routers? How to configure it into Cisco Packet Tracer? |CCNA|Networking](https://www.youtube.com/watch?v=76SnSKKgmnc). This video explains the `clock rate` command, its use with DCE devices/serial ports, and how to configure it in Cisco Packet Tracer.