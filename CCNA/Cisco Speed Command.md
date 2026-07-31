# Cisco Speed Command

## 🚀 Cisco `speed` Command Explained

The Cisco **`speed`** command is a **physical layer control** parameter configured primarily on **Ethernet interfaces** (FastEthernet, GigabitEthernet, etc.).

---

### What it Does

It **sets the actual physical data transmission rate** of the interface's hardware.

> Unlike the `bandwidth` command, the `speed` command **directly influences the physical behavior** of the interface and must be supported by the interface hardware.

---

### How and Why it is Used

1. **Setting the Transmission Rate (Physical Layer)**:
    
    - It forces the interface to operate at a specific, supported physical rate, such as **10**, **100**, **1000** (Mbps), or higher, depending on the interface type.
        
    - This command is almost always paired with the `duplex` command (e.g., `duplex full`).
        
    - **Example**: `speed 100` forces the interface to send and receive data at 100 Mbps.
        
2. **Controlling Auto-Negotiation**:
    
    - By default, Cisco Ethernet ports are set to `speed auto` and `duplex auto` to negotiate the highest possible speed and full duplex with the connected device.
        
    - The `speed` command is used to **manually override** this process. This is necessary when:
        
        - Connecting to an older or non-standard device that doesn't support auto-negotiation.
            
        - **Troubleshooting** a link where auto-negotiation is failing, leading to mismatched settings (e.g., one side at 100 Mbps/Full Duplex and the other stuck at 10 Mbps/Half Duplex, which causes severe performance issues).
            
3. **The Key Difference from `bandwidth`**:
    
    - **`speed`** sets the **actual physical rate** of data transfer on the wire (hardware setting).
        
    - **`bandwidth`** is a **logical/informational value** used by routing protocols (EIGRP/OSPF) and QoS for metric calculation and policy reference (software setting).
        
    - For modern Ethernet interfaces where the physical speed is fixed and known (e.g., a 1 Gbps port), setting the `speed` often automatically updates the logical `bandwidth` value, but `bandwidth` can still be manually set lower to affect routing decisions without changing the wire speed.
        

---

The command is typically configured on Ethernet interfaces that support multiple speeds (e.g., a 10/100/1000 Mbps port). On dedicated fixed-rate interfaces (like a 10GE fiber port), the `speed` command may not be available or may only allow setting to the fixed speed.

You can learn more about configuring the speed and duplex settings on a Cisco switch in this video: [How to change Port Speed on Cisco Switch, IOS](https://www.youtube.com/watch?v=UwXcloUeS9A).

[

![](https://www.gstatic.com/images/branding/productlogos/youtube/v9/192px.svg)

How to change Port Speed on Cisco Switch, IOS - YouTube







](https://www.youtube.com/watch?v=UwXcloUeS9A)