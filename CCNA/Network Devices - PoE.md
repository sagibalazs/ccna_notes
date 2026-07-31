
# Network Devices - PoE

# 1. **PoE – Comparing Table (PoE vs PoE+ vs PoE++ / IEEE Types & Classes)**

### A. Standards Overview Table (Main CCNA Focus)

| Feature            | PoE              | PoE+                     | PoE++ (Type 3)      | PoE++ (Type 4)                |
| ------------------ | ---------------- | ------------------------ | ------------------- | ----------------------------- |
| IEEE Standard      | 802.3af          | 802.3at                  | 802.3bt Type 3      | 802.3bt Type 4                |
| Max Power at PSE   | 15.4 W           | 30 W                     | 60 W                | 90–100 W                      |
| Max Power at PD    | 12.95 W          | 25.5 W                   | ~51 W               | ~71–90 W                      |
| Pairs Used         | 2                | 2                        | 4                   | 4                             |
| Typical Devices    | Phones, small AP | Modern AP, small PTZ cam | Wi-Fi 6 AP, signage | PTZ with heater, thin clients |
| Negotiation Method | Class 0–3        | Class 4                  | Class 5–6           | Class 7–8                     |
| Cable Requirement  | Cat5e+           | Cat5e+                   | Cat6 recommended    | Cat6a strongly recommended    |
| PSE Types          | Switch / Midspan | Switch / Midspan         | Switch              | Switch                        |

B. Power Class Table (All Classes 0–8)

|Class|Type|Standard|PD Power|Examples|
|---|---|---|---|---|
|0|1|802.3af|≈ 12.95 W|Generic PD (default)|
|1|1|802.3af|4.0 W|Low-power sensors|
|2|1|802.3af|7.0 W|Phones|
|3|1|802.3af|15.4 W|Entry AP, small camera|
|4|2|802.3at|25.5 W|Dual-band AP, PTZ mini|
|5|3|802.3bt|40 W|Wi-Fi 6 AP|
|6|3|802.3bt|51 W|Advanced video devices|
|7|4|802.3bt|62–71 W|Large PTZ, lighting|
|8|4|802.3bt|71–90+ W|UPOE+, thin clients|

# 2. **PoE Network Component Diagram**

Below is a **functional network diagram** showing PoE as a component within a LAN.

```less
                        +---------------------------+
                        |       DISTRIBUTION       |
                        |       L3 SWITCHES        |
                        +-----------+---------------+
                                    |
                                    | Uplink (non-PoE)
                                    |
                         -----------------------------
                         |                           |
              +----------+-----------+     +---------+-----------+
              |   ACCESS SWITCH A    |     |  ACCESS SWITCH B    |
              |   (PSE – PoE Ports)  |     |   (PSE – PoE Ports)  |
              +----------+-----------+     +---------+------------+
                         |                           |
     ---------------------                           ----------------------
     |           |         |                         |          |         |
     |           |         |                         |          |         |
 +---+---+   +---+---+  +--+--+                 +----+---+  +---+---+  +--+--+
 | IP    |   | WIFI  |  |CAM |                 | DOOR   |  | SENSOR|  | AP  |
 |PHONE  |   |  AP   |  |PTZ |                 |READER  |  |  IoT  |  | WiFi|
 +-------+   +-------+  +-----+                +--------+  +-------+  +-----+
   PD         PD         PD                       PD         PD        PD
 (Powered)   (Powered)  (Powered)                (Powered)  (Powered) (Powered)

```

**Legend**

- **PSE** = Power Sourcing Equipment (PoE Switch)
    
- **PD** = Powered Device (Phone, AP, Camera, IoT)
    
- Power + Data is carried over one Ethernet cable from Access Switch to PD.
    
- Distribution simply aggregates, does not provide PoE.
    

---

# 3. **One-Page Summary – PoE (Role & Function as a Network Component)**

Below is a **clean, CCNA-ready “one page”** summary you can put directly into your script.

---

# NE – Network Components – Power over Ethernet (PoE)

## One-Page Summary: Role & Function

### **1. Definition**

Power over Ethernet (PoE) is a function of network equipment—primarily **Access Switches**—that delivers **both electrical power and network data** over a single Ethernet cable to endpoint devices (PDs).

---

### **2. Role in the Network**

PoE extends the network’s capabilities beyond data transport. Its roles include:

1. **Powering edge devices** such as APs, IP phones, cameras, IoT sensors.
    
2. **Eliminating dependency on wall power outlets**, increasing placement flexibility.
    
3. **Centralizing power distribution**, improving reliability and uptime.
    
4. **Providing managed, secure, and monitored power delivery.**
    
5. **Enabling critical services** (voice, Wi-Fi, surveillance) that rely on constant availability.
    

---

### **3. Functions of PoE**

#### **A. Power Delivery Function (PSE Role)**

- Detect PDs
    
- Negotiate required power class
    
- Deliver safe DC power
    
- Maintain power and remove it safely
    
- Monitor load and port faults
    

#### **B. Endpoint Enablement**

PoE allows devices to operate without local AC adapters or technicians, enabling:

- Wireless coverage
    
- VoIP infrastructure
    
- Security surveillance
    
- Access control
    
- IoT deployments
    

#### **C. Network Management Function**

PoE introduces **power as a network-managed resource**:

- Port-level enable/disable
    
- Remote power reset (troubleshooting)
    
- Power priority levels
    
- Power budgeting per switch
    
- LLDP/LLDP-MED for advanced negotiation
    

#### **D. Reliability Function**

Because PoE originates from UPS-backed switches:

- Phones continue working during power outages
    
- Cameras stay online
    
- APs remain functional
    
- Access control systems retain power
    

This turns PoE into a **stability component** in enterprise networks.

#### **E. Security Support Function**

PoE indirectly strengthens security:

- PD power is controlled by the switch
    
- Rogue devices can be denied power
    
- Critical surveillance devices stay operational during outages
    
- Integration with 802.1X and port security
    

---

### **4. Placement in Network Topology**

- **Access Layer:** PoE switches power PDs.
    
- **Distribution/Core:** Aggregation only, no PoE.
    
- **Midspan Injectors:** Alternative PSE when switches lack PoE.
    

---

### **5. Pros (Why Networks Use It)**

- Single cable for power + data
    
- Reduced installation cost and complexity
    
- Increased deployment flexibility
    
- Centralized UPS-backed reliability
    
- Better manageability and remote control
    

---

### **6. Cons / Limitations**

- PoE switches cost more
    
- Limited to Ethernet 100 m distance
    
- High-power PDs require high-quality cabling
    
- Switch heat and power budget must be managed
    

---

### **7. CCNA-Relevant Conclusion**

**PoE is a key access-layer network component that enables the operation, control, and reliability of critical network endpoints by integrating power into the Ethernet infrastructure.**  
It is not just a power feature—**it is a functional part of network design, availability, and security.**





# NE – Network Components – **Power over Ethernet (PoE)**

**Role and Function**

---

# 1. **Role of PoE in a Network**

PoE is a **network power-delivery mechanism built into switches** (or injected via midspan devices) that allows a network to provide **electrical power and data over the same Ethernet cable**.

Its role within the network is:

1. **Provide centralized, controlled electrical power to endpoints** such as phones, access points, cameras, sensors.
    
2. **Enable flexible placement** of devices independent of wall power outlets.
    
3. **Increase network reliability** by powering critical endpoints through **UPS-protected switches**, not through random power sockets.
    
4. **Simplify the physical infrastructure**, reducing two separate cabling systems (Ethernet + electrical) to one.
    
5. **Allow the network itself to control power states**, including remote power restart, priority management, and load balancing.
    

In short: **PoE extends the network’s capability from “transporting data” to “powering the devices that create or use that data.”**

---

# 2. **Function of PoE as a Network Component**

PoE adds specific **functions** to a network, found mainly on **Access Switches** (but also midspans).  
These functions are:

---

## 2.1 **Power Delivery Function**

The switch acts as a **PSE (Power Sourcing Equipment)**.  
Its job is to:

- Detect whether a connected device needs PoE
    
- Negotiate power level
    
- Deliver a safe, standardized voltage
    
- Continuously maintain or remove power as required
    

**Function summary:**  
The switch becomes both a **data forwarder** and a **power provider** for endpoints.

---

## 2.2 **Endpoint Enablement Function**

PoE enables endpoints that would otherwise not be operational:

- IP Phones
    
- Wireless Access Points
    
- IP Cameras
    
- IoT sensors and controllers
    
- Door locks, badge readers
    
- Small switches (PoE-powered micro-switches)
    

**Function summary:**  
PoE allows endpoints to operate without local electrical power, making network deployment feasible in locations where powering equipment is difficult.

---

## 2.3 **Network Management & Control Function**

PoE adds **power as a manageable network resource**, giving these capabilities:

- **Port-level power on/off**
    
- **Remote power cycling** for troubleshooting
    
- **Power priority** settings
    
- **Power budgeting** across the whole switch
    
- **Monitoring**: actual watt draw, device class, faults
    

This transforms PoE from a passive mechanism into an **active network management feature**.

---

## 2.4 **Resiliency Function**

Because PoE power originates from a **central switch**, which is usually protected by:

- **UPS (Uninterruptible Power Supply)**
    
- **Dual power supplies**
    
- **Generator-backed electrical circuits**
    

…PoE endpoints remain active during:

- Power outages
    
- Local socket failures
    
- Adverse environmental conditions
    

**Function summary:**  
PoE improves the uptime of critical devices such as phones, APs, and cameras through centralized, protected power.

---

## 2.5 **Security-Relevant Function**

PoE indirectly contributes to network security because:

- The switch controls **which device receives power**.
    
- Rogue or unknown devices may not receive power or be restricted by 802.1X and Port Security.
    
- Cameras, badge readers, and APs stay online during power failures → maintaining security operations.
    
- Remote disabling of compromised endpoints is possible at the switch.
    

**Function summary:**  
PoE supports physical and network security by enabling controlled powering of security-critical endpoints.

---

## 2.6 **Infrastructure Simplification Function**

PoE reduces the need for:

- Local power adapters
    
- Electric outlets near each device
    
- Electric cable routing
    
- On-site electricians for installation
    

This directly impacts:

- Cost
    
- Deployment time
    
- Aesthetics
    
- Maintenance complexity
    

**Function summary:**  
PoE simplifies LAN infrastructure by removing the electrical-cabling dependency for endpoint devices.

---

# 3. **Where PoE Fits in the Network Topology**

## 3.1 **Access Layer**

This is the primary location of PoE.  
Access switches power:

- Access points
    
- Phones
    
- Cameras
    
- IoT devices
    

Role: **Provide both connectivity and power to edge devices.**

---

## 3.2 **Distribution Layer**

Rarely provides PoE directly, but:

- Aggregates powered devices
    
- Ensures redundancy
    
- Manages routing and policy enforcement
    

Role: **Backbone for PoE device communications.**

---

## 3.3 **Midspan (when present)**

Not common in modern designs, but serves when:

- Older switches lack PoE
    
- Only specific ports require PoE
    

Role: **Dedicated PSE providing power inline.**

---

# 4. **Why PoE is a Critical Network Component (CCNA Perspective)**

1. It allows **VoIP**, **WLAN**, and **surveillance** deployments to work reliably.
    
2. It reduces infrastructure cost and complexity.
    
3. It centralizes power to improve reliability.
    
4. It enhances network manageability.
    
5. It contributes to security and robustness.
    
6. It is a fundamental capability in modern enterprise networks.
    

---

# 5. **TL;DR – PoE Role & Function**

**Role:**  
A network component that enables switches to supply electrical power to connected devices through Ethernet cables.

**Functions:**

- Deliver power safely and intelligently
    
- Manage power consumption and priorities
    
- Increase reliability of endpoint devices
    
- Support network security and monitoring
    
- Simplify physical infrastructure
    
- Enable APs, phones, cameras, and IoT devices to operate anywhere
    

**Why it matters:**  
PoE is essential for modern enterprise access networks because it allows the network itself to power the devices that create user connectivity and security.



# NE – CCNA Preparing – Power over Ethernet (PoE)

## 1. Context & Problem Statement

Before PoE existed, network devices such as IP phones, wireless access points, security cameras, badge readers, and sensors required **separate data and power cabling**. This created:

- High installation cost (electrician required)
    
- Complex cable paths
    
- Unreliable power supplies
    
- Limited device placement (had to be near electrical outlets)
    

**PoE solved this problem** by delivering **DC power through the same Ethernet cable that carries data**, reducing complexity and enabling flexible deployments.

---

# 2. History & Evolution of PoE


|Era|Standard|Max Power at PD|Key Innovation|
|---|---|---|---|
|2003|**IEEE 802.3af (PoE)**|12.95 W|First formal PoE standard|
|2009|**IEEE 802.3at (PoE+)**|25.5 W|Higher power, Type 2 devices|
|2018|**IEEE 802.3bt (PoE++ / 4PPoE)**|Type 3: 51 W, Type 4: 71–100+ W|Power on all 4 pairs, major expansion|
|Cisco-prestandard|**Cisco UPOE, UPOE+**|60 W, ~90 W|Cisco proprietary extensions|

# 3. Terminology

**PSE** – Power Sourcing Equipment  
(e.g., PoE switch, power injector)

**PD** – Powered Device  
(e.g., access point, IP camera)

**MPS** – Maintain Power Signature  
(signal ensuring the PSE keeps supplying power)

**Modes A/B** – Different wire pairs used to deliver power  
(applies only to 802.3af/at)

---

# 4. Wire-Level Mechanisms (How PoE Works Electrically)

### Power Delivery Methods

PoE applies **48 V DC** with a tolerance range (usually 44–57 V).

802.3af/at use two pairs:

- **Mode A**: power on data pairs (1–2, 3–6)
    
- **Mode B**: power on spare pairs (4–5, 7–8)
    

802.3bt uses **all four pairs**, increasing efficiency and power.

### Discovery Process (Mandatory Safety Mechanism)

1. **Detection**
    
    - PSE applies a small voltage (~2.7–10 V)
        
    - Checks for a **25 kΩ signature resistor** on the PD
        
    - Ensures the device is designed to accept PoE
        
2. **Classification**
    
    - PD advertises its power class
        
    - PSE allocates appropriate wattage
        
3. **Power ON (Startup)**
    
    - Voltage rises to full operational level (up to ~57 V)
        
4. **MPS (Maintain Power Signature)**
    
    - PD periodically signals that it still needs power
        

This process prevents burning non-PoE devices and establishes safe negotiation.

---

# 5. PoE Standards, Types, and Power Classes

## 5.1 IEEE Standards Overview

### **802.3af – PoE (Type 1)**

- PSE output: 15.4 W
    
- PD available: 12.95 W
    
- For VoIP phones, small APs, simple cameras
    

### **802.3at – PoE+ (Type 2)**

- PSE output: 30 W
    
- PD available: 25.5 W
    
- For modern dual-band APs, PTZ mini cameras
    

### **802.3bt – PoE++ (Type 3 & Type 4)**

- **Type 3** PD power: up to 51 W
    
- **Type 4** PD power: up to 71–100 W
    
- Use cases:
    
    - High-end access points (Wi-Fi 6/6E)
        
    - PTZ cameras with heaters
        
    - Thin clients
        
    - LED lighting
        
    - PoE switches feeding other PoE devices (cascading)
        

---

### 5.2 Power Classes (CCNA must-know)

| IEEE    | Type | Class | PD Power | Typical Devices              |
| ------- | ---- | ----- | -------- | ---------------------------- |
| 802.3af | 1    | 1     | 4 W      | Low-end sensors              |
| 802.3af | 1    | 2     | 7 W      | Phones                       |
| 802.3af | 1    | 3     | 15.4 W   | Small AP                     |
| 802.3at | 2    | 4     | 25.5 W   | AP, PTZ camera               |
| 802.3bt | 3    | 5     | 40 W     | Wi-Fi 6 AP                   |
| 802.3bt | 3    | 6     | 51 W     | Video endpoints              |
| 802.3bt | 4    | 7     | 62–71 W  | Large PTZ                    |
| 802.3bt | 4    | 8     | 90–100 W | UPOE+, lighting, thin client |

Cisco’s **UPOE** maps roughly to IEEE Type 3/4.

---

# 6. PoE Architecture & Deployment Models

## 6.1 In-line Power Concepts

PoE can be delivered by:

### A. **PoE Switch (PSE)** — Most common

Switch has PoE circuits built into each port.

### B. **Midspan Injector**

Placed between switch and PD, used when switch lacks PoE.

### C. **PoE Extender / Pass-through Switch**

Powered by PoE-in and supplies PoE-out to multiple devices.

### D. **Outdoor/Industrial PoE**

Enhanced for extended temperatures and surge protection.

---

# 7. How PoE Works – Functional Flow

1. Cable plugged in
    
2. PSE sends detection voltage
    
3. PD responds with signature resistor
    
4. PD class is read
    
5. PSE allocates budget
    
6. Power ramps up
    
7. Ethernet link comes up
    
8. MPS keeps power alive
    
9. Power removed when PD disconnects
    

---

# 8. PoE Budgeting (Critical for Real Deployment)

### **PoE Budget = Total Watts available across all switch ports**

Example:  
Switch PoE budget = 370 W  
AP draws 13 W × 10 = 130 W  
Cameras draw 25 W × 8 = 200 W  
Total = 330 W → OK  
Remaining = 40 W

If over-subscribed, switches enforce **priority**:

- Critical (phones for emergency)
    
- High
    
- Low
    

Lower priority ports are shut off first.

---

# 9. Use Cases

### A. Enterprise LAN

- VoIP phones
    
- Wireless APs
    
- Security cameras
    
- Badge readers
    
- PoE lighting
    
- Digital signage
    
- VDI thin clients
    
- Remote sensors / IoT
    

### B. Industrial / OT networks

- SCADA controllers
    
- Building automation devices
    
- Outdoor cameras
    
- Access gates
    

### C. Data Center / Branch

- PoE-powered mini-switches
    
- Smart racks and environmental sensors
    

---

# 10. Advantages (Pros)

- Single cable for power and data
    
- Lower installation cost
    
- Centralized power backup (via UPS)
    
- Easy scaling and relocation
    
- Safer than wall power (low voltage DC)
    
- Standardized negotiation avoids damage
    
- Ideal for IoT and distributed environments
    

---

# 11. Disadvantages (Cons)

- PoE switch hardware is more expensive
    
- Cable distance limited to 100 m (Ethernet limit)
    
- High-power devices can stress budget
    
- Heat generation inside switch (more cooling required)
    
- PoE faults degrade both power AND data
    

---

# 12. Security Considerations (Important for CCNA & real networks)

### 12.1 Electrical / Physical Security

- **No unauthorized devices**: Rogue camera or device receiving PoE
    
- **Cable tapping risk**: Attackers may exploit unused PoE cables
    
- **Overcurrent protection** built in, but miswiring still risky
    

### 12.2 Network Security Interaction

PoE itself is not an attack vector, but it enables devices that _are_:

- Rogue AP plugging in → mitigated by port security
    
- IP phone bypass for VLAN hopping
    
- Cameras hacked → surveillance breach
    
- IoT devices with weak firmware receiving PoE
    

### SECURITY MUST-HAVES:

- Enable **802.1X** (access control)
    
- Use **MAB** (MAC Authentication Bypass) for phones
    
- Use **Port Security** to limit MACs
    
- Disable **unused PoE ports**
    
- Remote power shutoff for compromised devices
    

bb-Cisco Networks

(supports general network security principles related to LAN access control and securing network components).

---

# 13. Best Practices & No-Goes

## 13.1 Best Practices

- Use **Cat5e minimum, Cat6/Cat6a preferred** for PoE++
    
- Keep cable runs **under 90 m** (plus 10 m patch)
    
- Check **PoE budget BEFORE deployment**
    
- Use switches with **redundant power supplies**
    
- Add a **UPS** for PoE switch → guarantees PD continuity
    
- Set **port priority** (critical devices never lose power)
    
- Document each PoE port’s role
    
- Use **LLDP-MED** for phones and advanced PD negotiation
    
- Use industrial-grade PoE for outdoor/rugged environments
    

## 13.2 No-Goes

- Do NOT use cheap passive PoE adapters
    
- Do NOT pull PoE cables parallel with high-voltage power lines
    
- Do NOT exceed heat capacity in cable bundles
    
- Do NOT assume every AP or camera has identical power needs
    
- Do NOT allow user-accessible PoE ports without 802.1X
    
- Do NOT use midspans and PoE switches mixed without planning (risk of double-injection)
    

---

# 14. Troubleshooting PoE (Real-World Techniques)

Symptoms & Causes

| Symptom                 | Possible Cause                   |
| ----------------------- | -------------------------------- |
| PD does not power up    | Wrong class, insufficient budget |
| Cycling power           | Faulty cable, high resistance    |
| AP reboots under load   | Insufficient wattage (Wi-Fi 6E!) |
| Camera night mode fails | Power insufficient for IR LED    |
| Port dead               | PoE fault protection triggered   |


Commands (Cisco)

```bash
show power inline
show power inline police
show power inline interface Gi1/0/1
show lldp neighbors detail
```

# 15. Deployment Architecture Diagrams

### A. Standard PoE Switch Deployment

PoE Switch → Cat6 → AP/Camera/Phone

### B. Midspan

Switch → Midspan Injector → PD

### C. PoE Distribution in Branch

Core → Access Switch (PoE) → PDs  
UPS → PoE Switch → Ensures survivability

(If you want, I create full diagrams next.)

---

# 16. TL;DR Summary

PoE allows power + data through one cable.  
Standards: **802.3af (15 W)**, **802.3at (30 W)**, **802.3bt (60–100 W)**.  
Components: **PSE**, **PD**, negotiation required.  
Use cases: APs, phones, cameras, IoT, lighting.  
Key tasks: **power budgeting**, cable quality, port priority.  
Security: **802.1X**, port security, disable unused PoE, LLDP-MED.  
Best practice: central UPS, Cat6/Cat6a, avoid passive PoE.