# Cisco Bandwidth Command

## 💾 Cisco `bandwidth` Command Explained

The Cisco **`bandwidth`** command is an **informational** parameter configured on a router interface.

---

### What it Does

It **communicates the configured speed** (in kilobits per second, Kbps) of the interface to higher-level protocols and applications.

> **Crucially, it does NOT change the _actual_ physical transmission speed** of the interface. For instance, setting `bandwidth 1000` (1 Mbps) on a 100 Mbps FastEthernet port will not slow the port down; the port will still operate at its hardware-determined speed.

---

### How and Why it is Used

1. **Influencing Routing Protocol Metrics (The Main Reason)**:
    
    - Routing protocols like **OSPF** and **EIGRP** use the interface bandwidth value (often along with delay) as a key component in calculating the **metric** (cost) of a route.
        
    - By changing the `bandwidth` value, a network engineer can **influence a routing protocol's decision** to choose a specific path over another, effectively allowing for route manipulation.
        
2. **Accurate Network Monitoring**:
    
    - Network monitoring systems often use this configured bandwidth as the **denominator** to calculate interface utilization percentages. Setting the correct value ensures that utilization reports accurately reflect the fraction of the _actual_ available circuit speed that is being used, especially on leased or fractional WAN circuits where the physical interface speed is higher than the provisioned circuit speed.
        
3. **Quality of Service (QoS) and TCP**:
    
    - Some **Quality of Service (QoS)** mechanisms or features may use this value as a reference for policing or shaping calculations.
        
    - **TCP** (Transmission Control Protocol) on the Cisco device itself may adjust its initial retransmission and window-sizing parameters based on this configured bandwidth value.
        

The command is typically used when the **actual physical speed** of the circuit connected to the interface (e.g., a fractional T1 link) is **less than the default bandwidth** value assumed by the interface hardware.

You can learn more about how this command is used to manipulate routing metrics in this video: [Cisco Bandwidth command](https://www.youtube.com/watch?v=CmLRRMR29rU). This video provides a general idea of the bandwidth command, specifically within the context of EIGRP.

[

![](https://www.gstatic.com/images/branding/productlogos/youtube/v9/192px.svg)

Cisco Bandwidth command - YouTube







](https://www.youtube.com/watch?v=CmLRRMR29rU)