
## Router

✅ **Summary of summary:**  
Routers forward packets across networks using Layer 3 logic. They store configs in various memories, boot through defined stages, and are managed via IOS commands.  
Knowing the above command set covers ~90 % of what CCNA expects for router operation, verification, and troubleshooting.


## ⚙️ TL;DR — Router Role and Architecture (CCNA Summary)

A **router** is a **Layer 3 device** that forwards packets between different IP networks based on destination addresses.  
It connects LANs to WANs, isolates broadcast domains, applies routing logic and policies (ACL, NAT, QoS, VPN), and decides the **best path** using static or dynamic routing.

### 🔹 Function

- Builds and maintains the **routing table** (RIB → FIB/CEF).    
- Forwards packets according to **longest prefix match**.    
- Handles **control traffic** (routing protocols, ARP/ND, ICMP).    
- Provides **services** (DHCP relay, NTP, SNMP, Syslog, etc.).
    
### 🔹 Classification

| Type                     | Example          | Use               |
| ------------------------ | ---------------- | ----------------- |
| **Access/Branch Router** | ISR 1000/4000    | SOHO, branch      |
| **Edge Router**          | ASR 1000         | Internet/WAN edge |
| **Core Router**          | NCS series       | Backbone          |
| **Virtual Router**       | CSR 1000v / IOSv | Lab / Cloud       |

### 🔹 Hardware Components

| Part                   | Purpose                                |
| ---------------------- | -------------------------------------- |
| **CPU**                | Runs IOS / routing processes           |
| **RAM**                | Running config, routing & ARP tables   |
| **ROM**                | POST, bootstrap, ROMMON                |
| **Flash**              | Stores IOS image(s)                    |
| **NVRAM**              | Holds startup-config                   |
| **Backplane / Bus**    | Internal high-speed data path          |
| **Interfaces (ports)** | Physical I/O — GigE, SFP, Serial, Mgmt |
### 🔹 Software / Operation

- OS: **Cisco IOS / IOS XE / NX-OS**    
- Planes: **Control / Data / Management*    
- Forwarding: **CEF** (hardware-based, line rate)   
- Boot sequence: **POST → Bootstrap → IOS → Config**    
- Configuration register controls boot mode (0x2102 = normal).    
- Interface types: physical, sub-interface, loopback, tunnel, SVI.    
- Management: console, SSH, HTTP(S), SNMP.
    

✅ In short:  
Routers = **intelligent Layer 3 forwarders** combining specialized hardware (CPU + memory + ASICs) and IOS software to interconnect, control, and secure IP networks.

💻 Cisco Router CLI Summary (with short meaning)

```bash
# ─── Basic Info and System ────────────────────────────────
show version                      # IOS version, uptime, hardware, config-register
show interfaces                   # Interface status, IPs, errors
show ip interface brief            # Quick interface summary (up/down, IPs)
show running-config                # Current config in RAM
show startup-config                # Saved config in NVRAM
copy running-config startup-config # Save changes
erase startup-config               # Clear saved config (factory reset)
reload                             # Reboot router

# ─── File System & IOS Management ─────────────────────────
dir flash:                         # List IOS images/files
show file systems                  # View available storage
boot system flash:<image>          # Define IOS boot image
show boot                          # Display boot variables
copy tftp: flash:                  # IOS upgrade from TFTP
delete flash:<file>                # Remove file from flash

# ─── Routing & Forwarding ─────────────────────────────────
show ip route                      # View routing table (RIB)
show ip cef                        # View hardware forwarding table (FIB)
show adjacency                     # Layer-2 next-hop info
ip route <dest> <mask> <next-hop>  # Add static route
no ip route ...                    # Remove static route
show ip protocols                  # See active routing protocols
traceroute <IP>                    # Trace path through routers
ping <IP>                          # Test connectivity

# ─── Interfaces & VLANs ───────────────────────────────────
interface g0/0                     # Enter interface config
 ip address 192.168.1.1 255.255.255.0
 no shutdown                       # Enable interface
description <text>                 # Label interface
interface g0/0.10                  # Create sub-interface
 encapsulation dot1q 10            # VLAN tag for Router-on-a-Stick
 clock rate 64000                  # Set DCE clock (serial links)

# ─── Management & Security ────────────────────────────────
line console 0                     # Console settings
 password <pw> ; login             # Enable console login
line vty 0 4                       # Remote (SSH/Telnet) lines
 login local ; transport input ssh # Enable SSH access only
username admin secret <pw>         # Create local user
enable secret <pw>                 # Set privileged exec password
service password-encryption        # Encrypt plain-text passwords
banner motd ^C Authorized Access Only ^C

# ─── Misc / Troubleshooting ───────────────────────────────
show processes cpu                 # CPU usage
show processes memory              # Memory usage
show arp                           # IPv4-to-MAC mappings
show logging                       # Syslog buffer
debug ip packet                    # Debug packet forwarding (use with caution)

```



---
---
---
## 🧭 Router — Role and Function

A **router** is a **Layer 3 (Network Layer)** device responsible for **forwarding packets between different networks** using logical addressing (IP). It determines the **best path** to reach a destination network based on its **routing table** and **protocols** (RIP, OSPF, EIGRP, BGP).

Routers form the **core of internetworks (WAN, MAN, enterprise)** and are used to:

- Connect LANs to WANs or the Internet.    
- Segment broadcast domains.    
- Apply policies like **ACLs, QoS, NAT, VPN**, etc.    
- Provide redundancy and load balancing (HSRP, VRRP, GLBP).
    
## ⚙️ Functional Classification

|Type|Description|Typical Use|
|---|---|---|
|**Branch / Access Router**|Compact, integrated switch ports and WAN links (e.g., ISR 4000 Series)|SOHO, branch office|
|**Edge Router**|Border between enterprise and ISP|BGP peering, NAT, VPN|
|**Core Router**|High throughput, connects multiple distribution routers|Enterprise backbone|
|**Data-Center / Aggregation Router**|Optimized for 10/40/100 Gb E and virtualization|DC fabric (e.g., NCS Series)|
|**Service Provider Router**|High-performance MPLS/BGP|ISP core/edge|
|**Virtual Router**|Software instance (CSR 1000v, GNS3 IOSv, etc.)|Labs, NFV, cloud routing|

## 🧩 Internal Architecture (“Router Anatomy”)

|Component|Function|
|---|---|
|**CPU / Processor**|Executes the Cisco IOS XE or NX-OS kernel, routing protocols, management processes.|
|**RAM / DRAM**|Stores the running config, routing tables, ARP cache, and packet buffers. Volatile.|
|**ROM**|Holds bootstrap program and basic diagnostic code (POST).|
|**Flash Memory**|Non-volatile storage for IOS image(s), configuration backups, and logs.|
|**NVRAM**|Non-volatile, stores _startup-config_ file (copied into running-config at boot).|
|**Backplane / Bus**|High-speed internal data path connecting CPU and interfaces (measured in Gbps or Tbps).|
|**Interfaces (Ports)**|Network connectivity; each is a distinct Layer 3 interface with IP configuration.|
|**ASICs / NPUs**|Hardware acceleration for packet forwarding and QoS—found in ISR 4k, ASR, etc.|
|**Power Supplies & Fans**|Redundant modules ensure continuous uptime in modular routers.|

## 🔌 Port and Interface Types

|Category|Examples|Notes|
|---|---|---|
|**LAN**|RJ-45 Ethernet (10/100/1000), SFP/SFP+|For local connections|
|**WAN Serial**|Smart-Serial, V.35, T1/E1 (legacy)|Used for leased lines (HDLC, PPP)|
|**Optical**|SFP, SFP+, QSFP, XFP|Fiber modules, long-haul links|
|**Management**|Console (RJ-45 RS-232), Aux (Modem), Mgmt Ethernet|Out-of-band access|
|**High-speed / Modular Slots**|NM, HWIC, EHWIC, SPA slots|Expansion interfaces|

## 🧠 Software Architecture

- **Operating System:** Cisco IOS, IOS XE, NX-OS (depending on platform).    
- **Control Plane:** Handles routing protocols, ARP, ICMP, management.    
- **Data Plane (Forwarding Plane):** Hardware-based packet switching (CEF – Cisco Express Forwarding).    
- **Management Plane:** CLI, SNMP, SSH, RESTCONF, NETCONF, etc.
    - **Startup Sequence:**
    
    1. **POST** from ROM.        
    2. **Bootstrap** loads IOS from Flash (or TFTP).        
    3. IOS loads **startup-config** from NVRAM.        
    4. Router enters operational state.


## 💡 Router Performance Attributes

|Attribute|Description|
|---|---|
|**Throughput / PPS**|Rate of packet forwarding; key performance metric.|
|**Number of Routes Supported**|Defines routing-table capacity; higher for core routers.|
|**Interface Density / Speed**|Number and bandwidth of ports (1G – 400G).|
|**QoS / Buffer Memory**|For traffic shaping and congestion management.|
|**Redundancy**|Dual power, route processor failover (ISSU, NSF).|
|**Virtualization**|VRF, MPLS, logical routers for multitenancy.|

## 🧱 Router “Secrets” (Real-World Insights)

- The **backplane speed** determines if interfaces can forward at line-rate simultaneously; oversubscription occurs when sum > backplane bandwidth.    
- **CEF (Cisco Express Forwarding)** builds FIB and adjacency tables in hardware (TCAM), allowing wire-speed routing.    
- **NVRAM vs. Flash:** on newer platforms, both may exist in unified storage (nvram: and flash: merged).    
- **IOS XE** uses a **Linux kernel + IOSd** process — enabling programmability (NETCONF/RESTCONF) and modular restarts.    
- **ISR 4K series** uses multicore CPUs with dedicated data-plane ASICs (ESP – Embedded Services Processor).    
- **High-end routers (ASR 9K, NCS)** separate control and line cards; each line card has its own forwarding engine and memory.    
- **Boot variables:** `BOOT=flash:c1900-universalk9-mz.SPA.155-3.M3.bin` define which image loads; misconfiguration causes _rommon>_ mode.    
- **Config registers** (e.g., 0x2102) control boot behavior and password recovery.
    


## 🧭 Summary Diagram


        ┌───────────────────────────┐
        │        Control Plane      │
        │  - Routing Protocols      │
        │  - ARP, ICMP, Management  │
        ├───────────────────────────┤
        │        Data Plane         │
        │  - CEF, ASICs, TCAM       │
        │  - Forwarding Packets     │
        ├───────────────────────────┤
        │      Management Plane     │
        │  - SSH, SNMP, API, CLI    │
        └───────────────────────────┘
                 ↓
        ┌───────────────────────────┐
        │   Interfaces & Backplane  │
        │   (GigE, SFP, Serial, etc.)│
        └───────────────────────────┘


## Router Operation and Forwarding Logic

## 🧭 Router Operation – Step-by-Step

Routers are the **traffic directors** of an IP network. Their job:  
**receive packets, decide the best path, and forward them out the correct interface.**

Let’s break that process down precisely.

---

### 🔹 1. Packet Arrival (Ingress)

When a frame enters a router interface:

- The **interface hardware (Layer 1–2)** receives the electrical/optical signal.
    
- The **router strips the Layer 2 header/trailer**, exposing the **Layer 3 (IP) packet**.
    
- It checks the **destination IP address** inside the packet.
    

If the packet’s destination **matches one of the router’s own IP addresses**, the packet is for the **router itself** (control-plane traffic).  
Otherwise, it will be **routed (forwarded)** toward another network.

---

### 🔹 2. Routing Decision (Layer 3 Logic)

The router checks its **Routing Table (RIB)**, which contains:

- **Directly connected networks** (learned via interface configuration)
    
- **Static routes** (manually configured)
    
- **Dynamic routes** (learned via routing protocols like OSPF, EIGRP, BGP)
    

Each entry in the routing table lists:

```bash
Network / Prefix   Next-Hop or Exit Interface   Administrative Distance   Metric

```

The router uses **longest-prefix match** — the route with the most specific subnet mask wins.

If **no match** is found:

- The router drops the packet.
    
- Optionally, it sends an **ICMP Destination Unreachable** message.
    

If a **default route (0.0.0.0/0)** exists, it is used as a “catch-all.”

---

### 🔹 3. Next-Hop Resolution (ARP / ND)

Once the best route is chosen:

- If the **next-hop IP** is in the same subnet as the outgoing interface,  
    the router must find the **Layer 2 address (MAC)** for that next hop.
    
- It uses **ARP** (IPv4) or **Neighbor Discovery (IPv6)** to map IP → MAC.
    
- The result is stored in the **ARP table** or **adjacency table** (CEF).
    

---

### 🔹 4. Frame Rebuild (Egress)

The router now:

1. **Encapsulates** the packet into a new Layer 2 frame.
    
    - Source MAC = router’s outgoing interface MAC.
        
    - Destination MAC = next-hop device MAC.
        
2. **Recalculates Frame Check Sequence (FCS)**.
    
3. **Sends** the frame out of the chosen physical or logical interface.
    

Every hop along the path repeats this process.


## 🧩 Control Plane vs Data Plane vs Management Plane

|Plane|Role|Handled By|Examples|
|---|---|---|---|
|**Control Plane**|Builds routing and adjacency tables|CPU|OSPF, EIGRP, BGP, ARP, ICMP|
|**Data Plane (Forwarding)**|Moves packets according to FIB|ASIC / TCAM|CEF forwarding|
|**Management Plane**|Device administration|CPU (separate process)|SSH, Telnet, SNMP, RESTCONF|


## 🔍 Internal Tables and Their Purpose

|Table|Description|Built By|
|---|---|---|
|**RIB (Routing Information Base)**|Full routing table (all sources)|Control Plane|
|**FIB (Forwarding Information Base)**|Optimized subset used for actual forwarding|CEF|
|**Adjacency Table**|Layer 2 next-hop info|CEF / ARP|
|**ARP Table**|IPv4 → MAC mapping|ARP protocol|
|**Interface Table**|Lists interface states, IPs, speeds|IOS Kernel|
|**MAC Address Table**|Only relevant if switch ports are integrated (multilayer switch)|Switch ASIC|

## 🧠 Example – Routing Decision Flow

```bash
1. Packet arrives on G0/0
   Src IP: 192.168.10.10
   Dst IP: 8.8.8.8

2. Router strips L2 header
   Looks up 8.8.8.8 in routing table

3. Finds:
   0.0.0.0/0 → Next hop 203.0.113.1 via G0/1

4. Checks ARP for 203.0.113.1
   - If missing → sends ARP request
   - Once resolved → builds new frame

5. Re-encapsulates:
   Src MAC = router G0/1
   Dst MAC = next-hop’s MAC

6. Sends out G0/1
```

## ⚡ Forwarding Optimization (CEF Secret Sauce)

- **Traditional Process Switching:** Every packet analyzed by CPU (slow).
    
- **Fast Switching:** First packet analyzed; results cached (legacy).
    
- **CEF:** Hardware-based FIB lookup in ASIC/TCAM. Every packet at line rate.
    

Modern Cisco platforms (ISR 4k, ASR 9k, Catalyst 9k) use CEF exclusively.

## 🛡️ Additional Features Routers Handle

|Feature|Layer|Purpose|
|---|---|---|
|**NAT / PAT**|3–4|Translate internal IPs to public|
|**ACLs**|3–4|Filter traffic (security / QoS)|
|**QoS / Traffic Shaping**|2–7|Prioritize packets|
|**VPN (IPsec / GRE)**|3|Secure tunneling|
|**DHCP / DNS / NTP**|7|Auxiliary network services|
|**HSRP / VRRP / GLBP**|3|Redundant gateway|
|**SNMP / Syslog / NetFlow**|7|Monitoring and management|

🧭 Visualization: Router Packet Path

```bash
        ┌──────────────────────────────┐
        │         CONTROL PLANE        │
        │ Routing Protocols (OSPF, etc)│
        └─────────────┬────────────────┘
                      │
               builds RIB & ARP
                      ↓
        ┌──────────────────────────────┐
        │        DATA PLANE (CEF)      │
        │ Uses FIB + Adjacency Table   │
        │ Hardware forwarding (ASIC)   │
        └─────────────┬────────────────┘
                      │
             Packet Forwarding
                      ↓
        ┌──────────────────────────────┐
        │   OUTGOING INTERFACE PORT    │
        │   Encapsulation → Transmission │
        └──────────────────────────────┘

```

## In Summary

- Routers **operate primarily at OSI Layer 3**, using IP addresses to make forwarding decisions.
    
- The **routing table** defines how destinations are reached.
    
- **CEF and ASIC-based forwarding** allow line-rate routing.
    
- **Control plane** logic (protocols) builds routes; **data plane** hardware moves packets.
    
- Management access is always isolated logically or physically to protect the control plane.

## 🧭 ROUTING TABLE STRUCTURE (RIB)

Each router maintains a **Routing Information Base (RIB)** — the complete list of all routes it knows about, from every source (connected, static, dynamic).

You can view it with:  `show ip route
`
🧩 Structure Example

```bash
R1# show ip route
Codes: C - connected, S - static, R - RIP, O - OSPF, D - EIGRP, B - BGP

Gateway of last resort is 203.0.113.1 to network 0.0.0.0

C    192.168.10.0/24 is directly connected, GigabitEthernet0/0
S    192.168.20.0/24 [1/0] via 10.0.0.2, GigabitEthernet0/1
O    10.10.0.0/16 [110/20] via 10.0.0.2, 00:00:23, GigabitEthernet0/1
D    172.16.0.0/16 [90/2681856] via 10.0.0.3, 00:00:05, GigabitEthernet0/2
```

### 🧠 Breakdown

|Field|Meaning|
|---|---|
|**Code**|Source of route (C, S, O, D, R, B, etc.)|
|**Network/Prefix**|Destination network|
|**[AD/Metric]**|Administrative Distance and routing metric|
|**Next Hop**|IP of next router|
|**Interface**|Outgoing interface used to reach next hop|
|**Age**|Time since last update (dynamic protocols only)|

## 🧭 ROUTE SOURCES (How Routes Appear)

|Code|Type|Description|
|---|---|---|
|**C**|Connected|Directly attached networks (auto added)|
|**L**|Local|IP address of interface itself (/32)|
|**S**|Static|Manually configured (`ip route ...`)|
|**R**|RIP|Distance-vector, AD = 120|
|**O**|OSPF|Link-state, AD = 110|
|**D**|EIGRP|Hybrid protocol, AD = 90|
|**B**|BGP|Path-vector, AD = 20 (external)|
|**iBGP**|Internal BGP|AD = 200|
|**S***|Default static route|Matches all (0.0.0.0/0)|

## ⚙️ ADMINISTRATIVE DISTANCE (AD)

The **AD** represents _trustworthiness_ of the source.  
Lower = more trusted.

|Route Source|AD|Comment|
|---|---|---|
|Connected|0|Most reliable|
|Static|1|Manual, overrides dynamic|
|EIGRP (internal)|90|Cisco proprietary, fast convergence|
|OSPF|110|Link-state|
|RIP|120|Old, less preferred|
|External EIGRP|170|Learned via redistribution|
|iBGP|200|Inside same AS|
|Unknown / Unreachable|255|Route not used|
If two routes exist for the same destination, **the one with the lower AD wins**.

---

## 🧮 ROUTE METRICS (Path Quality Within Same Protocol)

If multiple routes have the same AD (e.g., both OSPF), the **metric** decides which is better.

|Protocol|Metric Basis|
|---|---|
|**RIP**|Hop count (1–15)|
|**EIGRP**|Composite (bandwidth + delay + reliability + load)|
|**OSPF**|Cost = ReferenceBandwidth / InterfaceBandwidth|
|**BGP**|Path attributes (AS_PATH length, local-pref, etc.)|

## 🧩 LONGEST PREFIX MATCH RULE

After AD and metric comparisons, Cisco applies the **longest-prefix match** rule when multiple entries match a destination.

Example:

```bash
show ip route | include 10.1.0.0
C 10.1.0.0/16 is directly connected, G0/0
O 10.1.1.0/24 [110/20] via 10.0.0.2

```
A packet to `10.1.1.10` matches both /16 and /24,  
but /24 is longer (more specific), so that route is chosen.

---

## 🧱 DEFAULT ROUTES

Default routes are used when no other entry matches.

`ip route 0.0.0.0 0.0.0.0 203.0.113.1
`
- Stored as **S*** in table.
    
- Often called the **Gateway of Last Resort**.
    
- Used in edge routers connecting to ISPs.
    

---

## 🔄 ROUTE REDUNDANCY & BACKUP

If two routes have:

- Same **prefix**
    
- Same **AD**, but different **metrics**
    

→ Router installs **equal-cost paths** → **load balancing** occurs (per-packet or per-destination).

If one route has **higher AD**, it becomes a **floating static route** (backup).

Example:

```
ip route 192.168.50.0 255.255.255.0 10.0.0.2 1
ip route 192.168.50.0 255.255.255.0 10.0.0.3 200
```

## 🧭 ROUTE INSTALLATION PROCESS

1. Router learns routes (connected/static/dynamic).
    
2. Each protocol sends its routes to the **RIB**.
    
3. The router compares **ADs** and **metrics**.
    
4. The _best_ route per destination prefix is kept in the RIB.
    
5. The **FIB (CEF)** copies the best entries into hardware.
    
6. Forwarding occurs using FIB + adjacency.

🧩 VISUAL FLOW

```bash
           +-----------------------------+
           | Control Plane (CPU)         |
           | - OSPF, EIGRP, RIP, etc.    |
           +-------------+---------------+
                         |
                         v
           +-----------------------------+
           | Routing Information Base    |
           +-------------+---------------+
                         |
          (Best Route by AD/Metric)
                         |
                         v
           +-----------------------------+
           | FIB (CEF Hardware Table)    |
           +-------------+---------------+
                         |
                         v
           [ Packet Forwarding at Line Rate ]

```

## 🧭 ROUTE SELECTION ORDER (Cisco Logic Summary)

1. **Longest Prefix Match**
    
2. **Lowest Administrative Distance**
    
3. **Lowest Metric (within same AD)**
    
4. **Equal-Cost Multi-Path (ECMP) Load Sharing**
    

---

## 🧠 Quick Example

|Destination|Source|AD|Metric|Result|
|---|---|---|---|---|
|10.0.0.0/24 via OSPF|OSPF|110|20|Used|
|10.0.0.0/24 via EIGRP|EIGRP|90|156160|✅ **Used (lower AD)**|
|10.0.0.0/8 via RIP|RIP|120|2|Ignored (higher AD)|
|0.0.0.0/0 static|Static|1|0|Used only for other destinations|
## 🧩 KEY COMMANDS

|Task|Command|
|---|---|
|Display routing table|`show ip route`|
|Display OSPF routes only|`show ip route ospf`|
|Display route to host|`show ip route 8.8.8.8`|
|Display CEF entry|`show ip cef 8.8.8.8 detail`|
|Display adjacency table|`show adjacency`|
|Display all learned routes|`show ip protocols`|

## 🧭 Summary

✅ Routing table holds all reachable networks.  
✅ AD defines _trust_, metric defines _quality_.  
✅ Longest prefix match decides _specificity_.  
✅ FIB mirrors the RIB’s best routes for hardware forwarding.  
✅ Floating static routes provide automatic failover.

## 🧩 1. Router Boot Process

Routers boot through **four main stages**, each of which can be verified and influenced:

|Step|Description|Commands / Files Involved|
|---|---|---|
|**1. POST (Power-On Self Test)**|Hardware check (CPU, RAM, interfaces, fans).|Happens automatically from ROM.|
|**2. Bootstrap**|Mini-bootloader that locates and loads the IOS image.|Stored in **ROM**.|
|**3. Load IOS**|The system loads the IOS image from Flash (or TFTP).|Controlled by `BOOT` variable or config register.|
|**4. Load Configuration**|The router loads **startup-config** from NVRAM → running-config in RAM.|If missing, router enters _setup mode_.|
> ⚙️ If IOS or config can’t be found → router drops into **ROMMON mode** for recovery.

**Verify boot order:**

```
show version
show boot
```

Change boot image manually:

```bash
boot system flash:c1900-universalk9-mz.SPA.155-3.M3.bin
```

## ⚙️ 2. Configuration Register

A small value in ROM controlling how the router boots and behaves.

|Common Value|Meaning|
|---|---|
|**0x2102**|Default — load IOS and startup-config|
|**0x2142**|Ignore NVRAM (used for password recovery)|
|**0x2101**|Boot from ROM|
|**0x2120**|Boot from TFTP (network boot)|
Check/Set:

```bash
show version
config-register 0x2102
```

## 💾 3. Memory Types Recap (Exam Favorite)

|Memory|Volatile|Content|
|---|---|---|
|**RAM / DRAM**|✅|Running config, routing tables, ARP cache, packet buffers|
|**NVRAM**|❌|Startup configuration|
|**Flash**|❌|IOS image(s), log files|
|**ROM**|❌|Bootstrap, POST, mini-IOS (ROMMON)|

## 🔌 4. Interface Types and Modes

|Interface Type|Description|Typical Commands|
|---|---|---|
|**Physical**|Actual ports (GigabitEthernet0/0, Serial0/0/0)|`ip address`, `no shutdown`|
|**Subinterface**|Logical split on a trunk (Router-on-a-Stick)|`interface g0/0.10`, `encapsulation dot1q 10`|
|**Loopback**|Virtual internal interface (always up)|`interface loopback0`|
|**SVI (Switched Virtual Interface)**|For L3 switch VLANs|`interface vlan 10`|
|**Tunnel**|Virtual Layer 3 interface for VPNs|`interface tunnel0`|
|**Management**|Out-of-band access (Mgmt0, FastEthernet0)|`ip address`, `ip ssh version 2`|

## 🧱 5. Hardware Form Factors

|Platform|Description|Examples|
|---|---|---|
|**Fixed**|All interfaces built-in|Cisco ISR 900, 1000 series|
|**Modular**|Slots for Interface Cards (HWIC, EHWIC, NIM)|ISR 4000, ASR 1000|
|**Chassis-Based**|Separate line cards + route processors|ASR 9000, NCS, CRS|
|**Virtual**|Software router (CSR 1000v, IOSv, Cloud Services)|Labs, NFV, Cloud|
## 🔐 6. Management & Access Methods

|Access Type|Layer|Purpose|
|---|---|---|
|**Console Port**|Out-of-band, Serial (RS-232)|First-time local config|
|**AUX Port**|Out-of-band, via modem|Remote dial-in|
|**SSH / Telnet**|In-band|Remote CLI access|
|**HTTP / HTTPS**|7|GUI management (Cisco CCP / Web UI)|
|**SNMP / Syslog**|7|Monitoring and logging|
|**TFTP / FTP / SCP**|7|IOS or config transfer|

```bash
line vty 0 4
 login local
 transport input ssh
```

## ⚙️ 7. Router File System

Routers have a small, UNIX-like file system.

|Command|Function|
|---|---|
|`dir flash:`|List files in flash memory|
|`copy running-config startup-config`|Save config|
|`copy tftp: flash:`|IOS upgrade|
|`delete flash:filename.bin`|Remove file|
|`boot system flash:filename.bin`|Define boot image|
|`show file systems`|List all available storage areas|
## 🧩 8. Packet Switching Methods

|Method|Description|Speed|Status|
|---|---|---|---|
|**Process Switching**|CPU handles every packet|Slow|Legacy|
|**Fast Switching**|First packet cached; next use cache|Medium|Obsolete|
|**CEF (Cisco Express Forwarding)**|Hardware-based FIB lookup|Fastest|Default|

## 🛡️ 10. Router Security Fundamentals

|Feature|Purpose|Command|
|---|---|---|
|**Local user accounts**|Authentication|`username admin secret <pw>`|
|**Console and VTY passwords**|Access control|`line console 0`, `password`, `login`|
|**SSH only**|Secure remote login|`transport input ssh`|
|**Banner MOTD**|Legal notice|`banner motd ^C Unauthorized access forbidden ^C`|
|**Service password-encryption**|Obfuscate cleartext|`service password-encryption`|
|**AAA (with RADIUS/TACACS+)**|Centralized auth|`aaa new-model`|

## 🧠 11. Router Performance Factors

- **CPU Utilization** (`show processes cpu`)
    
- **Memory Usage** (`show processes memory`)
    
- **Interface Load** (`show interfaces`)
    
- **CEF statistics** (`show ip cef summary`)
    
- **Backplane Bandwidth** (depends on platform)
    
- **Redundancy & Failover** (dual RP, ISSU, NSF)
    

> High CPU during routing updates or ACL processing indicates **software switching** — check for missing CEF.

---

## 🔍 12. Real-World “Dirty Secrets”

💡 These often appear in troubleshooting or simulation scenarios:

|Topic|Insider Detail|
|---|---|
|**IOS Images**|Different editions (IP Base, IP Services, Advanced Security) unlock features.|
|**Startup Failures**|`System bootstrap, Version x.x.x` means no IOS found — use TFTP or ROMMON.|
|**Unified Memory**|On new routers, NVRAM and Flash may share one partition.|
|**Password Recovery**|Use config-register 0x2142 to bypass startup-config, reset passwords, then restore.|
|**Serial Interfaces**|DCE side must provide clock signal: `clock rate 64000`.|
|**IPv6 Routing**|Must be explicitly enabled: `ipv6 unicast-routing`.|
|**Backup via TFTP**|`copy running-config tftp:` – often used in labs for versioning.|
|**IOS Licensing**|Newer devices use “Smart Licensing” – not all features active by default.|

## ✅ SUMMARY: Router Mastery Checklist

|Category|Covered|
|---|---|
|Role & function (Layer 3)|✅|
|Internal hardware (CPU, RAM, Flash, etc.)|✅|
|Data/control/management planes|✅|
|Forwarding logic & CEF|✅|
|Routing table logic (AD, metrics, prefix)|✅|
|Boot process & config register|✅|
|Interface types & modes|✅|
|Management & file system|✅|
|Switching methods|✅|
|Security & redundancy basics|✅|