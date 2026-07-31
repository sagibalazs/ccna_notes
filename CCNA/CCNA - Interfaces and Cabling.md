

  # # **NE – CCNA – Physical Media Overview Table**

**Compare physical interface and cabling types (1.3.a)**  
**Single-mode fiber | Multimode fiber | Copper (Coax → UTP/STP)**

| Attribute                               | **Copper (Coax → Twisted Pair)**                      | **Multimode Fiber (MMF)**                | **Single-Mode Fiber (SMF)**                 |
| --------------------------------------- | ----------------------------------------------------- | ---------------------------------------- | ------------------------------------------- |
| **Chronological appearance**            | Coax (1970s), UTP 1980s → today                       | 1980s → today                            | 1980s → today, dominant in WAN/Backbone     |
| **Medium type**                         | Electrical (copper conductor)                         | Optical (LED light)                      | Optical (Laser light)                       |
| **Signal type**                         | Electrical voltage                                    | Light (850 nm LED)                       | Light (1310–1550 nm laser)                  |
| **Core size**                           | N/A                                                   | 50/62.5 µm                               | ~9 µm                                       |
| **Transmission distance**               | Up to 100 m (UTP)                                     | 300 m – 2 km depending on standard       | 10–80 km typical; >100 km with amplifiers   |
| **Common connectors**                   | RJ45, BNC (legacy)                                    | LC, SC                                   | LC, SC                                      |
| **Common cable categories / standards** | Cat5e, Cat6, Cat6A, Cat7                              | OM1, OM2, OM3, OM4, OM5                  | OS1, OS2                                    |
| **Bandwidth capacity**                  | Up to 10 Gbps (Cat6A), 40 Gbps in data centers (Cat8) | Up to 100 Gbps (short-range)             | 100 Gbps+ (long-range), DWDM capacity       |
| **Immunity to EMI/RFI**                 | Low (UTP), Medium (STP)                               | Very high                                | Very high                                   |
| **Power delivery**                      | Supports PoE/PoE+/UPOE                                | No                                       | No                                          |
| **Installation cost**                   | Low                                                   | Medium                                   | High                                        |
| **Termination difficulty**              | Easy (RJ45)                                           | Medium (polish/clean required)           | High (precision)                            |
| **Typical use cases**                   | Access layer, PoE endpoints                           | Data center, campus backbone short-range | ISP links, MAN/WAN, long-haul backbone      |
| **Security**                            | Susceptible to tapping, crosstalk, induction          | Very secure                              | Very secure (hard to tap without detection) |
| **CCNA focus**                          | Ethernet interfaces (1G/10G), PoE                     | Fiber uplinks between switches           | WAN links, enterprise backbone              |

# **Connections Overview (1.3.b)**

**Shared Media vs. Point-to-Point (quick summary table)**

|Attribute|**Shared Media Ethernet**|**Point-to-Point Ethernet**|
|---|---|---|
|**Topology**|Bus / hub-based legacy|Switch-to-host / switch-to-switch|
|**Collision domain**|Shared (hubs), uses CSMA/CD|Single device per link (no collisions)|
|**Duplex**|Half-duplex historically|Full-duplex|
|**Performance**|Poor scalability, not used today|Standard today (all modern Ethernet)|
|**Medium types**|Coax (10Base-5/2), UTP (10Base-T hubs)|UTP/Fiber switch ports|
|**CCNA relevance**|Historical only|Modern enterprise networks|


# **1. COMPARISON TABLE – Physical Media + Connection Types**

## **A. Physical Media Comparison (Copper / MMF / SMF)**

| Attribute                   | **Copper (Cat3–Cat8)**       | **Multimode Fiber (OM1–OM5)**            | **Single-Mode Fiber (OS1/OS2)**      |
| --------------------------- | ---------------------------- | ---------------------------------------- | ------------------------------------ |
| **Introduction**            | 1990s                        | 1990s                                    | 1980s                                |
| **Medium type**             | Electrical                   | Optical (LED/VCSEL)                      | Optical (Laser)                      |
| **Core size**               | —                            | 50–62.5 µm                               | ~8–9 µm                              |
| **Max typical distance**    | 100 m                        | 100–500 m                                | 10–80+ km                            |
| **Max Ethernet speeds**     | 40 Gbps (Cat8)               | 100 Gbps (SR optics)                     | 400–800 Gbps (DWDM long-haul)        |
| **Interference immunity**   | Low (UTP) / Medium (STP)     | Very high                                | Extremely high                       |
| **PoE support**             | Yes                          | No                                       | No                                   |
| **Installation complexity** | Easy                         | Medium                                   | High (precision required)            |
| **Cost (material)**         | Low                          | Medium                                   | Medium                               |
| **Cost (transceivers)**     | Very low                     | Medium                                   | High                                 |
| **Use cases**               | Access layer, PoE endpoints  | Short-range fiber uplinks, DC leaf–spine | WAN, MAN, building-to-building, core |
| **Security**                | Susceptible to tapping & EMI | Very secure                              | Highly secure; hard to tap           |
| **Connector types**         | RJ45                         | LC, SC                                   | LC, SC                               |
| **Best feature**            | Cheap, PoE                   | Easy fiber for 10G+                      | Long-distance, highest speed         |
| **Weakest point**           | EMI, distance                | Limited distance                         | Expensive optics                     |

**B. Connection Type Comparison (Shared vs. Point-to-Point)**

| Attribute            | **Shared Media Ethernet**       | **Point-to-Point Ethernet**     |
| -------------------- | ------------------------------- | ------------------------------- |
| **Topology**         | Bus (coax) or hub-based star    | Switch-based P2P links          |
| **Collision domain** | One big domain                  | One per link (effectively none) |
| **Duplex**           | Half-duplex only                | Full-duplex                     |
| **CSMA/CD needed?**  | Yes                             | No                              |
| **Performance**      | Very low scalability            | Predictable, scalable           |
| **Common speeds**    | 10 Mbps                         | 10 Mbps → 400+ Gbps             |
| **Media types**      | Coax, early UTP                 | UTP/STP, MMF, SMF               |
| **Primary devices**  | Hubs, coax taps                 | Switches                        |
| **Security**         | One station can see all traffic | Traffic isolated per port/VLAN  |
| **Current status**   | Fully obsolete                  | Universal in modern networks    |

# **2. OVERALL DIAGRAM – Evolution of Ethernet Media & Connections**

This diagram shows the chronological development of:

- **Media** (Coax → Twisted Pair → MMF → SMF)
    
- **Connection types** (Shared → Point-to-Point)
    
- **Topologies** (Bus → Hub → Switch)
    

```less
1970s–1980s      1990–2000s           2000–Today             Now → Future
━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━━━━━━━━━
Coaxial         │Twisted Pair     │Multimode Fiber         │Single-Mode Fiber
10BASE-5/2      │Cat3–Cat8        │OM1 → OM5               │OS1 / OS2
(bus)           │RJ45             │LC/SC                   │LC/SC
10 Mbps         │10M–40G          │1G–100G (short)         │10G–800G (long-haul)
```

**B. Evolution of Connection Types**


```less
1980s                   1990s                               2000s–Today
━━━━━━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━━━━━━━━━
Shared Media            │Shared Star                      │Point-to-Point
Bus (coax)              │Hubs (multiport repeaters)       │Switched Ethernet
CSMA/CD                 │Still collisions                 │Full-duplex
Half-duplex             │Half-duplex                      │No CSMA/CD
Single collision domain │Shared bandwidth                 │Dedicated bandwidth
                        │                                 │Per-port isolation
```

**C. Combined Architectural Evolution**

```less
COAX BUS  →  HUB (shared star)  →  SWITCH (point-to-point)
   │               │                        │
   │               │                        ├─ Twisted Pair (access)
   │               │                        ├─ MMF (campus / DC)
   │               └─ Still shared           └─ SMF (long-distance backbone)
   │
   └─ Obsolete
```

**D. Complete "At-a-Glance" Diagram**

```less
                           ┌───────────────────────────┐
                           │   Modern Ethernet Today    │
                           │   (Point-to-Point Only)    │
                           └──────────────┬────────────┘
                                          │
          ┌────────────────────────────────────────────────────┐
          │       Physical Media Evolution (Chronological)     │
          └────────────────────────────────────────────────────┘
                      │                     │                 │
        1. COAX  →     2. TWISTED PAIR  →    3. MMF  →       4. SMF
      (10BASE-5/2)      (10BASE-T → Cat8)      (OM1–OM5)       (OS1/OS2)
      Bus topology      Star/hub → switches    Short-range      Long-range
      Shared media      P2P begins             P2P fiber        Carrier fiber
      Obsolete          Dominant access layer  DC/Campus        Backbone

          ┌────────────────────────────────────────────────────┐
          │           Connection Architecture Evolution        │
          └────────────────────────────────────────────────────┘
                Shared bus → Shared star → Switched P2P → Fiber P2P
                  (CSMA/CD)    (Hubs)         (Switches)          (Core/WAN)
```














---

## **A. Evolution of Physical Media**


# **1. COAXIAL ETHERNET (10BASE-5 & 10BASE-2)**

**The first widely deployed physical medium for Ethernet networks**

---

## **1. Title & Date**

- **10BASE-5 (Thick Ethernet / Thicknet)** – introduced **1983** (IEEE 802.3 original standard)    
- **10BASE-2 (Thin Ethernet / Thinnet)** – introduced **1985**    

These two built the foundation for local networking before twisted pair and switches existed.

---

## **2. Visualization**

### **10BASE-5**

```less
[Device]—(Transceiver tap)—=== 10mm thick yellow coax ===—(tap)—[Device]
```

10BASE-2

```less
[Device]—BNC T-Connector—== thin coax ==—BNC T—[Device]
                |                               |
             Terminator                     Terminator
```

Both systems used a **shared bus topology**, one long cable where every node listened to the same wire.

![[Pasted image 20251211233737.png]]





---

## **3. Description in Bullet Points**

### Common attributes:

- Physical **shared bus medium** for early Ethernet.
    
- Used **electrical signals** over copper coax.
    
- Required **precise cable length and termination**.
    
- Only supported **10 Mbps Ethernet**, **half-duplex**.
    
- Nodes accessed the medium using **CSMA/CD**.
    

### Differences:

|Feature|10BASE-5|10BASE-2|
|---|---|---|
|Cable thickness|Thick (10mm)|Thin (5mm)|
|Max segment length|500 m|185 m|
|Connectors|Vampire tap + AUI transceiver|BNC T-connector|
|Installation|Very difficult|Much easier|
|Use|Backbone|Workgroup LANs|

## **4. Involved Devices, Media, Protocols**

### Devices:

- NIC with **AUI** port (Attachment Unit Interface)
    
- MAU/transceiver
    
- Repeaters (early hubs)
    

### Media:

- RG-8 coax (10BASE-5)
    
- RG-58 coax (10BASE-2)
    

### Protocols/Mechanisms:

- **CSMA/CD** (Carrier Sense Multiple Access with Collision Detection)
    
- **Manchester encoding**
    
- Ethernet II framing (same as today)
    

---

## **5. How It Works – Step by Step**

### Example: a device sends a frame on 10BASE-2

1. **Carrier Sense**  
    NIC checks if the bus is silent.
    
2. **Transmit**  
    Device sends the electrical frame onto the coax bus.
    
3. **Shared Medium**  
    Every host receives the frame, but only the NIC with the matching MAC processes it.
    
4. **Collision Detection**  
    If two devices transmit simultaneously, voltages collide → both detect and stop.
    
5. **Backoff**  
    Devices wait random time and retry.
    
6. **Terminators** absorb signals  
    The ends of the bus required **50-ohm terminators** to prevent signal reflection.
    

---

## **6. Best Practices**

- Maintain correct **cable lengths** (no too short segments).
    
- Always use **proper 50-ohm terminators** on both ends.
    
- Avoid electrical interference sources (motors, fluorescent lights).
    
- Keep **BNC connectors clean and tight**.
    

---

## **7. No-Goes**

- Never leave the bus unterminated.
    
- Never create loops or star shapes.
    
- Never mix 10BASE-2 and video coax types.
    
- Do not add/remove devices while the network is active (causes downtime).
    

---

## **8. Importance**

Coax Ethernet introduced:

- **CSMA/CD**
    
- **Bus topology concepts**
    
- The first physical layer of Ethernet
    
- The foundation for all future LAN development
    

Coax cabling is now **obsolete**, but understanding it is **mandatory for CCNA history/evolution**.

---

## **9. Pros and Cons**

### **Pros**

- Simple technology for its time.
    
- Long distances (500 m per segment).
    
- Inexpensive (10BASE-2).
    

### **Cons**

- **Highly unreliable** – one bad connector brings down everything.
    
- **Entire network was one collision domain**.
    
- **Half-duplex only**.
    
- Hard to expand/maintain.
    
- Thicknet required drilling clamps into the cable.



# **2. TWISTED PAIR COPPER (Cat3 → Cat8)**

**The medium that transformed Ethernet into the modern switched networks we use today.**

This includes **UTP/STP**, categories **Cat3, Cat5, Cat5e, Cat6, Cat6A, Cat7, Cat8**, and the transition from **shared hubs** to **point-to-point switched Ethernet**.

Structure follows the same NE-standard as before.

---

# **1. Title & Date**

- **Telephone twisted pair** existed since the early 20th century.
    
- **Ethernet over Twisted Pair** began with **10BASE-T (1990)**.
    
- Rapid evolution through the 1990s–2020s enabled speeds from **10 Mbps → 40 Gbps**.
    

---

# **2. Visualization**

### **UTP structure**

```less
+------------------------------------------------+
|  PVC Jacket                                     |
|  +------------------------------------------+   |
|  |  4 Twisted Pairs (color-coded)           |   |
|  |  Each pair tightly twisted to reduce EMI |   |
|  +------------------------------------------+   |
+------------------------------------------------+
```

**Point-to-point Ethernet topology**

```less
[PC]──(RJ45)──UTP──(RJ45)──[Switch]
```

**Key:** Each link is its own **collision domain → full-duplex possible → no CSMA/CD needed**.

---

# **3. Description in Bullet Points**

### Common attributes:

- Uses **electrical signaling** over copper wire pairs.
    
- Main connector: **RJ45 (8P8C)**
    
- Two shielding types:
    
    - **UTP** (Unshielded Twisted Pair) – most common
        
    - **STP/FTP/SFTP** (various shielded variants) – used in high-EMI environments
        
- Supports **PoE** delivery (a major advantage over fiber).
    
- Supports **auto-negotiation**, full/half duplex, auto-MDI/MDI-X.
    

### Evolution of Categories:

|Category|Date|Max Speed|Max Distance|Typical Use|
|---|---|---|---|---|
|**Cat3**|~1990|10 Mbps|100 m|Early 10BASE-T|
|**Cat5**|~1995|100 Mbps|100 m|100BASE-TX|
|**Cat5e**|~1999|1 Gbps|100 m|Standard today|
|**Cat6**|~2002|1 Gbps / 10 Gbps (55m)|100 m|New installs|
|**Cat6A**|~2008|10 Gbps|100 m|Modern enterprise|
|**Cat7/Cat7A**|2010s|10–40 Gbps|100 m|Shielded environments|
|**Cat8**|2016|25–40 Gbps|30 m|Data center top-of-rack|

# **4. Involved Devices, Media, Protocols**

### Devices:

- NICs (10/100/1G/10GBASE-T)
    
- Switches
    
- Routers
    
- PoE injectors / PoE switches
    
- Patch panels, keystones
    

### Media:

- UTP/STP cable, RJ45 connectors, punch-down blocks
    

### Protocols/Mechanisms:

- **Auto-Negotiation**
    
- **Auto-MDI/MDI-X**
    
- **PoE standards (802.3af/at/bt)**
    
- Electrical signaling:
    
    - MLT-3 (Fast Ethernet)
        
    - PAM (Gigabit and faster)
        

---

# **5. How It Works – Step by Step**

Example: **1 Gbps Ethernet over Cat5e**

1. **Link Initialization**  
    Devices perform auto-negotiation (speed & duplex).
    
2. **Auto-MDI/MDI-X** adjusts wire pairs  
    No cross-cable needed.
    
3. **Signal Encoding**  
    4 pairs used simultaneously with **PAM-5** encoding.
    
4. **Full Duplex** communication  
    There are **no collisions**, CSMA/CD is not used at all.
    
5. **Frame Transmission**  
    Bits are encoded, sent across all 4 twisted pairs, and decoded by the receiver.
    
6. **PoE (optional)**  
    Power is sent over spare pairs or data pairs depending on the standard.
    

---

# **6. Best Practices**

- Keep cable length **≤ 100 m**.
    
- Maintain **proper bend radius**.
    
- Avoid parallel runs next to electrical cables.
    
- Use **Cat6A** for new long-term installations.
    
- Test cables using a **certifier** (not just continuity testers).
    
- Ground shielded cable correctly (if used).
    

---

# **7. No-Goes**

- Never mix categories in the same run.
    
- Do not exceed 100 m total channel distance.
    
- Do not untwist pairs more than 13 mm during termination.
    
- Never run UTP through environments with high EMI (engines, industrial machinery).
    
- Do not sharply bend, crush, or stretch cables.
    
- Do not install shielded cable without proper grounding.
    

---

# **8. Importance**

Twisted Pair Ethernet:

- Enabled **star topology** instead of shared bus.
    
- Made **switching** possible → each link isolated → modern LANs.
    
- Added **full duplex**, eliminating collisions.
    
- Allowed **PoE**, enabling VoIP phones, cameras, APs.
    
- Scaled from **10 Mbps → 40 Gbps** without changing connector format.
    

This is the **dominant physical medium for access-layer networking today**.

---

# **9. Pros and Cons**

### **Pros**

- Cheap and easy to install.
    
- Supports PoE.
    
- Robust for 100 m distances.
    
- Flexible, modular, easy troubleshooting.
    
- Backwards compatible.
    

### **Cons**

- Limited distance (100 m).
    
- Susceptible to EMI (UTP).
    
- Higher latency and power consumption at 10G+ compared to fiber.
    
- Thick bundles reduce airflow in racks.

```vbnet
Twisted Pair Copper is the dominant Ethernet medium.
Cat3→Cat8 evolution enabled 10 Mbps up to 40 Gbps.
Star topology + switches eliminated collisions.
Supports PoE, auto-negotiation, auto-MDI-X.
Distance limited to 100 m; EMI can be an issue.
```


# **3. MULTIMODE FIBER (MMF – OM1 → OM5)**

**The first widely adopted optical medium for Enterprise LAN backbones and data centers.**

---

## **1. Title & Date**

- **Multimode Fiber (MMF)** introduced commercially in the **late 1970s**, became widely used for Ethernet in the **1990s** with Fast Ethernet and Gigabit Ethernet optical standards.
    
- Category evolution:
    
    - **OM1 (62.5 µm)** – early LAN fiber
        
    - **OM2 (50 µm)** – improved bandwidth
        
    - **OM3 / OM4** – laser-optimized for 10G/40G/100G
        
    - **OM5** – wideband multimode for short-range wavelength multiplexing
        

---

## **2. Visualization**

### Fiber structure (simplified)

```less
[Core: 50 or 62.5 µm] --- Light travels in multiple paths (modes)
[Cladding: 125 µm]
[Coating/Jacket]
```

**Light propagation in Multimode**

```less
 |‾‾‾‾‾‾‾‾‾‾‾‾‾|
 | \   |   /    |
 |  \  |  /     |
 |   \ | /      |
 |    \|/       |
 | Multiple paths (modes)
 |_______________|
```

Because light bounces along multiple paths, **modal dispersion** limits distance.

---

## **3. Description in Bullet Points**

- Optical medium using **short-wavelength (850 nm) LEDs or VCSEL lasers**.
    
- Larger **core** than SMF → easier to connect, cheaper electronics.
    
- Supports high bandwidth over **short distances (hundreds of meters)**.
    
- Connectors: **LC (most common), SC (older)**.
    
- Used mostly inside **buildings**, data centers, campus short backbones.
    
- Faster adoption came with **OM3/OM4** for 10G/40G/100G Ethernet.
    

---

## **4. Involved Devices, Media, Protocols**

### Devices:

- Fiber NICs, SFP/SFP+/QSFP transceivers
    
- Switch uplinks / aggregation layer devices
    
- Media converters
    

### Media:

- OM1 (62.5 µm)
    
- OM2 (50 µm)
    
- OM3 (50 µm laser optimized)
    
- OM4 (improved OM3)
    
- OM5 (wideband MMF)
    

### Relevant Ethernet Standards:

|Speed|Standard|Wavelength|Typical MMF Distance|
|---|---|---|---|
|1 Gbps|1000BASE-SX|850 nm|220–550 m|
|10 Gbps|10GBASE-SR|850 nm|300–400 m|
|40/100 Gbps|40GBASE-SR4 / 100GBASE-SR4|850 nm|100–150 m|

## **5. How It Works – Step by Step**

Example: **10GBASE-SR over OM3**

1. **Transmitter** uses a **VCSEL laser at 850 nm**.
    
2. Light enters the **50 µm core** and propagates along many modes.
    
3. **Loss increases** with distance due to:
    
    - Modal dispersion
        
    - Attenuation
        
    - Connector/splice loss
        
4. The receiver detects the optical pulses and converts them into electrical signals.
    
5. Fiber requires **proper polishing, cleaning, alignment** for low-loss signal transmission.
    

---

## **6. Best Practices**

- Use **LC connectors** for new deployments.
    
- Prefer **OM4** for long-term future-proofing.
    
- Clean fiber connectors **every time** before plugging.
    
- Avoid macrobending (tight loops).
    
- Use proper **patch panel management** to prevent microbending.
    
- Use **transceiver–fiber matching** (don’t mix OM1 with 10G expectations).
    

---

## **7. No-Goes**

- Never mix **62.5 µm OM1** with **50 µm OM2/3/4** fiber.
    
- Do not exceed maximum modal bandwidth distance.
    
- Avoid physical bending beyond manufacturer radius.
    
- Never touch or blow dust into connectors (serious signal loss).
    
- Never look into a fiber end — invisible IR laser can damage eyes.
    

---

## **8. Importance**

Why MMF matters:

- Enabled **high-speed Ethernet** in enterprise buildings far earlier than copper could.
    
- Allowed scaling from **1G → 10G → 40G → 100G**.
    
- Cheaper than single-mode for short distances.
    
- Easier physical handling and termination.
    

MMF is the **dominant short-range fiber** for:

- Access → Distribution uplinks
    
- Distribution → Core (short range)
    
- Data center spine/leaf
    
- Server → TOR switch connections at high speeds
    

---

## **9. Pros and Cons**

### **Pros**

- Lower cost electronics than SMF.
    
- High bandwidth for short distances.
    
- Flexible and easy to install.
    
- Good for dense intra-building cabling.
    

### **Cons**

- Limited distance due to modal dispersion.
    
- Not suitable for MAN/WAN.
    
- Multiple fiber types require careful matching.
    
- Connector cleanliness is critical.


### 10. TL;DR

```less
Multimode Fiber = 50/62.5 µm optical cable for high-speed, short-range LAN connectivity.
Uses 850 nm LED/laser, supports 1G–100G Ethernet.
Ideal for data centers and campus buildings.
Limited distance (100–500 m) due to modal dispersion.
Cheaper optics than single-mode, but physically less future-proof.
```

# **4. SINGLE-MODE FIBER (SMF – OS1 / OS2)**

**The backbone medium for enterprise, MAN/WAN, and ISP networks.**  
This follows the same NE-structure you use.

---

# **1. Title & Date**

- **Single-Mode Fiber (SMF)**
    
- Commercial introduction late **1970s–1980s**, widespread use from **1990s → today**
    
- Standardized types:
    
    - **OS1** – indoor, tighter buffered (older)
        
    - **OS2** – outdoor/loose tube, modern, low-loss, high-performance
        

---

# **2. Visualization**

### Single-Mode Fiber structure

Core is extremely small → allows **only one propagation mode**.

```bash
[Core: ~8–9 µm]  → Single, straight light path (no bouncing)
[Cladding: 125 µm]
[Coating / Jacket]
```

**Comparison to MMF**

```less
MMF: Many light paths → dispersion
SMF: One light path → long distance, high bandwidth
```

# **3. Description in Bullet Points**

- Uses **lasers** (1310 nm / 1550 nm wavelength), not LEDs.
    
- Very small core → almost no modal dispersion.
    
- Supports extremely long distances:
    
    - **10 km – 80 km** typical
        
    - 100+ km with amplification (EDFA)
        
- Extremely high bandwidth capacity
    
- Common connectors: **LC (modern), SC (older)**
    
- Used in:
    
    - Enterprise backbone (distribution ↔ core)
        
    - MAN/WAN connections
        
    - ISP long-haul fiber
        
    - FTTH (GPON, XGS-PON)
        

---

# **4. Involved Devices, Media, Protocols**

### Devices:

- Laser-based transceivers:
    
    - SFP / SFP+ / SFP28 / QSFP+ / QSFP28 / QSFP56 / QSFP-DD
        
- Routers, core switches, carrier equipment
    
- DWDM systems for wavelength multiplexing
    

### Media:

- **OS1** (older indoor, higher attenuation)
    
- **OS2** (modern, low-loss, outdoor, long-haul)
    

### Ethernet standards (SMF distances)

|Speed|Standard|Wavelength|Typical Distance|
|---|---|---|---|
|1G|1000BASE-LX|1310 nm|5–10 km|
|10G|10GBASE-LR|1310 nm|10 km|
|10G|10GBASE-ER|1550 nm|40 km|
|40G|40GBASE-LR4|1310 nm|10 km|
|100G|100GBASE-LR4|1310 nm|10 km|
|DWDM|Multiple|1550 nm|80–100+ km|

# **5. How It Works – Step by Step**

Example: **10GBASE-LR over OS2**

1. **Laser activation**  
    Laser diode (typically DFB or FP) emits coherent light at 1310 nm.
    
2. **Light enters the 8–9 µm core**  
    The extremely small core forces **single-mode propagation**.
    
3. **Transmission**  
    Light travels straight with extremely low attenuation and almost no dispersion.
    
4. **Long-distance reach**  
    Amplifiers (EDFA) and DWDM can extend into 100–1000 km ranges.
    
5. **Reception**  
    Receiver photodiode converts incoming light back to electrical pulses.
    

This architecture eliminates the limitations of MMF and copper.

---

# **6. Best Practices**

- Use **OS2** for all modern installations.
    
- Follow **LC** connector standardization.
    
- Clean fiber ends meticulously before every insertion.
    
- Keep bend radius large to avoid macrobending loss.
    
- Label wavelengths when working with CWDM/DWDM optics.
    
- Store unused fiber in dust caps only.
    
- Use proper fusion splicing for long-distance runs.
    

---

# **7. No-Goes**

- Never mix **OS1 and OS2** in a long-distance link (loss mismatch).
    
- Avoid dirty connectors — most SMF failures are contamination-related.
    
- Do not exceed optical power budgets.
    
- Never run SMF without eye protection; **1550 nm lasers are invisible and dangerous**.
    
- Do not use MMF transceivers in SMF and vice versa.
    

---

# **8. Importance**

Single-Mode Fiber is the **foundation of all modern high-speed networks**, enabling:

- Carrier-grade backbones (100G / 400G / 800G)
    
- Long-distance enterprise interconnects
    
- Metro Ethernet (MAN)
    
- FTTH access networks
    
- Data center spine networks
    

SMF provides:

- Longest reach
    
- Highest bandwidth
    
- Lowest latency
    
- Most scalable and future-proof physical medium
    

It is the **endgame medium** for all large networks.

---

# **9. Pros and Cons**

### **Pros**

- Extremely long distances (10–100+ km).
    
- Highest bandwidth potential.
    
- Supports DWDM: hundreds of channels per fiber.
    
- Low loss and minimal dispersion.
    
- Very secure — tapping is extremely difficult.
    

### **Cons**

- Higher transceiver cost vs. MMF.
    
- Installation requires precision tools (fusion splicing).
    
- Smaller core → more sensitive to contamination.
    
- Not suitable for short copper-replacement scenarios where PoE is required.
    

---

# **10. TL;DR**

```less
Single-Mode Fiber = 8–9 µm laser-driven fiber for long-distance, high-bandwidth networks.
Supports 10–100+ km links with 10G/40G/100G/400G Ethernet.
OS2 is the modern standard with low attenuation.
Used in enterprise backbones, ISP networks, DWDM, FTTH.
Most scalable and future-proof medium available.
```

























# **5. SHARED MEDIA ETHERNET**

(10BASE-5, 10BASE-2, 10BASE-T hubs – CSMA/CD domain)

This was the first operational connection type in Ethernet history.  
We follow the full NE-structure exactly as before.

---

# **1. Title & Date**

**Shared Media Ethernet**

- Origin: **1983** (IEEE 802.3) with **10BASE-5 coax**
    
- Continued through **10BASE-2** (1985)
    
- Last form: **Hubs** (1990s) with 10BASE-T and 100BASE-TX
    

Shared media is **obsolete**, but absolutely essential for CCNA foundations.

---

# **2. Visualization**

### Shared bus (coax)

```less
[PC]───┬─────────────┬─────────────┬──[PC]
       |             |             |
    Tap/T-Conn    Tap/T-Conn   Tap/T-Conn
                (One collision domain)
```

**Shared star (hub)**

```less
          HUB (Repeater)
      ┌────┬────┬────┬────┐
     [PC] [PC] [PC] [PC]
   (Still one collision domain)
```

Even with a star physical shape → **logical bus** when using hubs.

---

# **3. Description in Bullet Points**

- Every device shares the same electrical medium.
    
- Only **one station can transmit at a time**.
    
- Requires **CSMA/CD** (Carrier Sense Multiple Access with Collision Detection).
    
- Bandwidth is **shared** among all devices.
    
- Collisions increase exponentially with load.
    
- Half-duplex only.
    
- Entire network segment is one **collision domain**.
    

This was acceptable for **10 Mbps** but breaks completely in modern traffic.

---

# **4. Involved Devices, Media, Protocols**

### Devices:

- 10BASE-5 / 10BASE-2 transceivers
    
- Hubs (multiport repeaters)
    
- NICs capable of CSMA/CD
    

### Media:

- Coaxial Ethernet (RG-8, RG-58)
    
- Later UTP (Cat3/Cat5) cables connected to hubs
    

### Protocols / Mechanisms:

- **CSMA/CD**
    
- **Manchester encoding**
    
- **Collision domains**
    
- **Jam signals**
    
- **Exponential backoff algorithm**
    

---

# **5. How It Works – Step by Step**

### Transmission attempt:

1. **Carrier Sense**  
    Node listens to check if the medium is idle.
    
2. **Transmit**  
    If free, it sends the frame.
    
3. **Collision detection**  
    If another station transmits simultaneously, both detect a voltage anomaly.
    
4. **Jam signal**  
    Stations send a jam signal to notify all nodes.
    
5. **Backoff**  
    Both wait a random “backoff” time and retry.
    

### With hubs:

- Hub repeats electrical signals out all ports.
    
- Every port sees everything.
    
- Still one giant collision domain.
    

---

# **6. Best Practices**

(For historical/theoretical understanding only)

- Keep user count per shared segment low.
    
- Maintain proper cable lengths (esp. coax).
    
- Use proper terminators.
    
- Monitor collision rates (>5–10% is problematic).
    
- Upgrade to **switches** whenever possible.
    

---

# **7. No-Goes**

- Do not use hubs in modern networks.
    
- Do not mix cable types improperly.
    
- Do not exceed maximum coax run lengths.
    
- Do not connect full-duplex devices to hubs.
    
- Do not expect consistent latency or throughput.
    

---

# **8. Importance**

Shared media:

- Represents the **first generation** of Ethernet.
    
- Introduced foundational concepts:
    
    - CSMA/CD
        
    - Collision domains
        
    - Repeater rules
        
- Critical to understanding **why switches replaced hubs** and **why full-duplex exists**.
    

---

# **9. Pros and Cons**

### **Pros**

- Simple and cheap for the time.
    
- Standardized early Ethernet networks.
    
- Enabled first LAN deployments globally.
    

### **Cons**

- One collision domain → very poor performance.
    
- Half-duplex only.
    
- Collisions, backoff, instability under load.
    
- Not scalable.
    
- Obsolete.
    

---

# **10. TL;DR**

```less
Shared media Ethernet = all devices share one cable or hub.
Only one can talk at a time → collisions → CSMA/CD.
Half duplex, low performance, not scalable.
Historically important but fully obsolete in modern networks.
```

# **6. POINT-TO-POINT ETHERNET**

**(Full-duplex, switched Ethernet — the architecture of all modern networks)**

This is the connection type used everywhere today: access layer, distribution/core, data centers, fiber uplinks, copper links, 1G/10G/40G/100G/400G, etc.

Same NE-structure as always.

---

# **1. Title & Date**

**Point-to-Point Ethernet**

- First introduced with early switching (~1990s).
    
- Became universal once switches replaced hubs.
    
- Today: all Ethernet interfaces (copper/fiber) operate **point-to-point**, **full-duplex**, **no collisions**.
    

---

# **2. Visualization**

### Typical switch-to-host link

```less
[PC]───(RJ45 or LC)───[Switch Port]
   |<---- Full Duplex ---->|
  Dedicated link; no other device shares this medium
```

**Switch-to-switch fiber uplink**

```less
[Switch A] ⇄ [SFP/SFP+] ⇄ Fiber ⇄ [SFP/SFP+] ⇄ [Switch B]
Full-duplex, separate Rx/Tx fibers
```

This is the **default** connection model in all Ethernet designs today.

---

# **3. Description in Bullet Points**

- Each link connects **exactly two devices** → point-to-point.
    
- Operates **full-duplex** → no CSMA/CD, no collisions.
    
- Each link is its own **collision domain** → effectively eliminated.
    
- Bandwidth is **not shared**; link speed is dedicated.
    
- Switches perform:
    
    - MAC learning
        
    - Forwarding decisions
        
    - Loop prevention (STP)
        
    - VLAN segmentation
        
- Fiber and copper both use point-to-point architectures.
    
- Auto-negotiation handles duplex, speed, PoE, flow control.
    

---

# **4. Involved Devices, Media, Protocols**

### Devices:

- Switches (access/distribution/core)
    
- Servers, PCs
    
- Routers & firewalls
    
- SFP/SFP+/QSFP transceiver modules
    

### Media:

- Copper UTP/STP (Cat5e–Cat8)
    
- Fiber (MMF OM1–OM5; SMF OS1/OS2)
    

### Relevant protocols & mechanisms:

- **Auto-negotiation**
    
- **Auto-MDI/MDI-X**
    
- **802.3x flow control**
    
- **LLDP/CDP** for neighbor discovery
    
- **STP/MSTP/RSTP** for redundant switched topologies
    
- **PoE (802.3af/at/bt)**
    
- **Pause frames** (Ethernet flow control)
    

---

# **5. How It Works – Step by Step**

Example: **1G copper point-to-point link (Cat6)**

1. **Auto-negotiation starts**  
    Devices exchange fast link pulses to agree speed & duplex.
    
2. **Auto-MDI-X** determines Tx/Rx pairs  
    Crossover cables no longer needed.
    
3. **Full-duplex enabled**  
    Tx and Rx occur simultaneously → no collisions.
    
4. **Switch learns MAC**
    
    - Switch sees source MAC on port → adds MAC entry.
        
    - Unicast traffic is forwarded only where needed.
        
5. **Dedicated bandwidth**  
    A 1 Gbps link always gives full throughput to that host.
    
6. **Link reliability**  
    CRC checks, link pulses, and autoneg detect cable faults.
    

---

# **6. Best Practices**

- Use **switches**, never hubs.
    
- Prefer **full-duplex** operation at all times.
    
- Use proper category-rated cabling for link speed (Cat6A for 10G).
    
- Keep link lengths ≤100 m for copper.
    
- Use **fiber uplinks** for building-to-building or core links.
    
- When redundancy exists → configure STP or EtherChannel.
    
- Label ports & cables, document VLAN assignments.
    

---

# **7. No-Goes**

- Do not force duplex settings unless absolutely necessary.  
    (Duplex mismatch → massive packet loss.)
    
- Never mix PoE and non-PoE injectors incorrectly → may damage devices.
    
- Do not use cheap or unknown SFP modules in critical infrastructure.
    
- Do not exceed cable distances.
    
- Do not run copper near EMI sources (motors, fluorescent lights).
    
- Never use hubs in any modern network.
    

---

# **8. Importance**

Point-to-point Ethernet is the foundation of **all modern Ethernet networks** because:

- No collisions
    
- Predictable performance
    
- Full-duplex, dedicated bandwidth
    
- Scalable from 10 Mbps → 400 Gbps
    
- Works with VLANs, STP, QoS, and all enterprise features
    
- Simple troubleshooting (one link = one problem domain)
    

It is the **only connection type** used in:

- Corporate LANs
    
- Data centers
    
- Campus networks
    
- Internet backbone links (fiber point-to-point)
    

---

# **9. Pros and Cons**

### **Pros**

- No collisions → maximum throughput.
    
- Full duplex → simultaneous Tx/Rx.
    
- Highly scalable.
    
- Simplified troubleshooting.
    
- Works with all modern Ethernet standards.
    
- Supports PoE.
    

### **Cons**

- Requires more physical ports than shared media networks.
    
- Distance limitations on copper.
    
- Fiber equipment increases cost.
    

---

# **10. TL;DR**

```less
Point-to-point Ethernet = modern switched full-duplex links.
No collisions, no CSMA/CD, dedicated bandwidth, high performance.
Basis of all modern LAN/MAN/WAN Ethernet.
Copper or fiber, scalable from 10 Mbps → 400 Gbps+.
```





