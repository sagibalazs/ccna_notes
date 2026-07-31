
## L2 Switch

### **Layer 2 Switch – Role, Function, and Architecture (CCNA / CompTIA N+ Summary)**

---

#### **1. Role of a Layer 2 Switch**

A **Layer 2 switch** operates at the **Data Link Layer (OSI Layer 2)**.  Its main job is to:
- Connect multiple devices (hosts, printers, access points, routers) within the same **LAN**.    
- Forward Ethernet **frames** based on **MAC addresses** (not IP).    
- Provide **collision domain isolation** per port (each port = one collision domain).    
- Extend bandwidth and reduce network congestion.   
🧠 **Think:** “A switch learns who is where and sends frames only where needed.”

#### **2. Core Functions**

|Function|Description|
|---|---|
|**MAC Learning**|Switch listens to incoming frames and builds a MAC address table (source MAC → ingress port).|
|**Forwarding**|When destination MAC is known, the switch forwards the frame only to that port.|
|**Flooding**|If MAC unknown, switch floods frame out all ports except source.|
|**Filtering**|Prevents loops and duplicates with **Spanning Tree Protocol (STP)**.|
|**Segmentation**|Each port is an independent collision domain, improving efficiency.|

#### **3. Classification of Switches**

|Type|Description|Example|
|---|---|---|
|**Unmanaged Switch**|Plug-and-play, no configuration, used in SOHO.|TP-Link, Netgear Desktop|
|**Managed Switch**|CLI/Web/SNMP management, VLANs, QoS, STP, security.|Cisco Catalyst 2960X|
|**Smart Switch**|Limited management (web-based), small networks.|Cisco Small Business SG300|
|**Multilayer (L3) Switch**|Performs L2 switching + L3 routing (SVIs, static/dynamic routes).|Cisco Catalyst 3850|
|**Data Center Switch**|High throughput, modular, fabric-ready (NX-OS).|Cisco Nexus 9000|

#### **4. Hardware Architecture**

|Component|Description|
|---|---|
|**Backplane (Fabric)**|High-speed internal bus connecting all ports. Defines total switching capacity (Gbps/Tbps).|
|**ASICs (Application-Specific Integrated Circuits)**|Specialized chips that perform switching and forwarding in hardware — extremely fast.|
|**CPU (Processor)**|Handles control-plane tasks (CLI, STP, ARP, SNMP, etc.). Not used for data forwarding.|
|**RAM (Dynamic Memory)**|Stores runtime info (MAC table, ARP cache, running config). Volatile.|
|**Flash Memory**|Stores IOS system image and configuration files. Non-volatile.|
|**NVRAM**|Stores startup configuration (`startup-config`).|
|**Ports**|RJ-45 (copper), SFP/SFP+/QSFP (fiber), console, AUX.|
|**Power Supply**|May include redundant PSU, fans, or PoE modules.|
|**PoE Components**|Provide electrical power to devices (802.3af/at/bt).|

#### **5. Software and Firmware Attributes**

|Software Element|Description|
|---|---|
|**Cisco IOS / NX-OS**|Operating systems managing configuration and operation.|
|**Control Plane**|CPU-based functions like STP, CDP, LLDP, ARP, VLAN database.|
|**Data Plane (Forwarding Plane)**|ASIC-based real-time frame forwarding.|
|**Management Plane**|SSH, SNMP, Syslog, AAA, configuration access.|

#### **6. Port Types**

|Port Type|Function|
|---|---|
|**Access Port**|Connects to end devices (PCs, printers). Carries traffic for one VLAN.|
|**Trunk Port**|Connects switches or routers. Carries multiple VLANs (802.1Q tagging).|
|**SPAN / Mirror Port**|Copies traffic for monitoring/IDS.|
|**PoE Port**|Powers devices like IP phones and access points.|
|**Uplink Port**|High-bandwidth connection to core/distribution layer.|

#### **7. Performance Attributes**

|Metric|Description|
|---|---|
|**Port Speed**|10/100/1000/10G/40G/100G Mbps or more.|
|**Backplane Bandwidth**|Total switching throughput capacity.|
|**Forwarding Rate**|Measured in Mpps (millions of packets per second).|
|**Latency**|Delay between input and output (<5 µs for enterprise switches).|
|**Buffer Size**|Handles microbursts; measured in MB per port or shared pool.|

#### **8. Management & Security Features**

- **CLI, Web, SNMP, NetFlow, Syslog**
    
- **AAA (RADIUS, TACACS+)**
    
- **SSH over Telnet (secure)**
    
- **Port Security:** limit MACs per port
    
- **BPDU Guard / Root Guard:** STP protection
    
- **DHCP Snooping, DAI (Dynamic ARP Inspection)**
    
- **Private VLANs, ACLs, QoS**

#### **9. Memory Layout (Cisco Example)**

|Memory Type|Stores|Volatile|
|---|---|---|
|**RAM**|Running configuration, ARP, MAC tables|✅ Yes|
|**NVRAM**|Startup configuration|❌ No|
|**Flash**|IOS image, config backups|❌ No|
|**ROM**|Bootstrap program|❌ No|
|**CPU Registers**|Active process state|✅ Yes|

#### **10. Switch Boot Sequence (Cisco IOS)**

1. **POST (Power-On Self Test)** – Hardware check
    
2. **Bootstrap** – Loads IOS from Flash or TFTP
    
3. **IOS Load** – System image initialization
    
4. **NVRAM** – Loads startup-config
    
5. **Running-config** – Device operational

⚙️ **Switch CLI Essentials (CCNA Level)**

```bash
# Basic system setup
Switch> enable
Switch# configure terminal
Switch(config)# hostname SW1
Switch(config)# enable secret class
Switch(config)# service password-encryption
Switch(config)# banner motd #Unauthorized access prohibited#

# Interface configuration
Switch(config)# interface FastEthernet0/1
Switch(config-if)# description Link-to-PC1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10
Switch(config-if)# no shutdown

# VLAN configuration
Switch(config)# vlan 10
Switch(config-vlan)# name SALES
Switch(config-vlan)# exit

# Assign IP for management (SVI)
Switch(config)# interface vlan 1
Switch(config-if)# ip address 192.168.1.2 255.255.255.0
Switch(config-if)# no shutdown
Switch(config)# ip default-gateway 192.168.1.1

# Security
Switch(config)# line console 0
Switch(config-line)# password cisco
Switch(config-line)# login
Switch(config)# line vty 0 4
Switch(config-line)# password cisco
Switch(config-line)# login
Switch(config-line)# transport input ssh

# Save configuration
Switch# copy running-config startup-config

```

### **TL;DR Summary**

|Category|Summary|
|---|---|
|**Function**|Forwards frames based on MAC addresses (Layer 2).|
|**Main Job**|Connect hosts within LAN, reduce collisions, improve bandwidth.|
|**Key Components**|ASICs, backplane, CPU, RAM, NVRAM, Flash, ports.|
|**Forwarding**|Hardware-based, fast (ASIC).|
|**Control Plane Tasks**|STP, VLANs, CDP, LLDP.|
|**Management**|SSH, SNMP, Syslog, AAA.|
|**Common Commands**|`show mac address-table`, `show vlan`, `show interfaces`, `switchport mode`, `vlan <id>`.|

---
---
---

## 🔍 1. Inside the Switching Process — Step by Step

Every Ethernet frame entering a switch port goes through four fundamental logic steps handled by the **ASIC forwarding engine**:

|Step|Description|Hardware Table Used|
|---|---|---|
|**1️⃣ Learning**|Switch reads the **source MAC address** and remembers on which port it arrived.|**CAM Table (MAC Address Table)**|
|**2️⃣ Forwarding / Filtering**|Looks up the **destination MAC**. If known → forward out the specific port. If same port → drop (filter).|CAM Table|
|**3️⃣ Flooding (if unknown)**|If destination MAC not yet learned, the frame is sent out all ports in that VLAN (except the incoming one).|Broadcast domain|
|**4️⃣ Aging**|Each dynamic entry has a timer (default ≈ 300 s). If unused, entry is deleted.|CAM aging timer|

## ⚙️ 2. Switch Table Architecture

### Content-Addressable Memory (CAM)

- Stores: **MAC → Port → VLAN**
    
- Lookup is performed in **parallel** across all entries (hardware search, not sequential).
    
- Each frame’s destination MAC is compared simultaneously — instant result.
    

### Ternary CAM (TCAM)

- Used by multilayer switches for ACLs, QoS, and policy lookups.
    
- Stores 0, 1, and “don’t care” bits (ternary).
    
- Enables **hardware-level filtering**: ACL permit/deny, VLAN-ACL, QoS class-maps.


## 🌐 3. Flooding, Broadcasts, and Multicast

|Type|Behavior|
|---|---|
|**Unknown Unicast**|Flooded within VLAN. Entry learned on response.|
|**Broadcast (FF:FF:FF:FF:FF:FF)**|Flooded to all ports within VLAN. Used by ARP, DHCP DISCOVER, etc.|
|**Multicast**|Initially flooded; optimized later via **IGMP snooping** or **CGMP**.|

## 🌀 4. Loop Prevention — **Spanning Tree Protocol (STP)**

Without control, redundant paths cause endless frame circulation → MAC table instability.  
STP (IEEE 802.1D / 802.1w RSTP / 802.1s MSTP) elects one **Root Bridge** and blocks redundant links.

### STP Logic Summary

|Step|Description|
|---|---|
|**Root Election**|Lowest **Bridge ID = Priority + MAC** wins. Default 32768.|
|**Root Port (RP)**|On non-root switches, the port with the **lowest path cost** to the root.|
|**Designated Port (DP)**|On each segment, the port with the **lowest cost** to the root remains forwarding.|
|**Blocked Port (BP)**|All others block to break loops.|

### RSTP (802.1w) Enhancements

- Converges in **< 2 s** vs 30–50 s (STP).
    
- Adds **Alternate and Backup port roles**.
    
- Edge ports use **PortFast** (skip listening/learning).
    
- Protect edge ports with **BPDU Guard** → err-disable on BPDU receive.
    

---

## 🧩 5. Multiple VLANs and Trunking Interaction

Each VLAN has its **own STP instance** (or MST region).  
Flooding and MAC learning are isolated **per VLAN**.  
Trunks (802.1Q) tag frames with **VLAN ID (12 bits)** → up to 4094 VLANs.

⚡ 6. Switch Packet Flow Example

```bash
[PC-A]---Fa0/1(SW1)---Fa0/24(SW2)---[PC-B]
```

- PC-A sends frame → SW1 learns **src MAC** on Fa0/1.
    
- Destination unknown → SW1 floods VLAN 10 out Fa0/24 (trunk).
    
- SW2 learns **src MAC** on Fa0/24.
    
- PC-B replies; SW2 knows dest MAC → forwards only Fa0/24.
    
- SW1 now learns PC-B’s MAC on Fa0/24; future traffic unicast.

## 🧮 7. Verification and Troubleshooting CLI

|Command|Purpose|Notes|
|---|---|---|
|`show mac address-table`|Lists learned MAC entries.|Check VLAN, Port, Type (dynamic/static).|
|`clear mac address-table dynamic`|Flush dynamic entries.|Forces relearning.|
|`show interfaces status`|Port status (up/down, speed/duplex, VLAN).|Detect cabling or mismatch.|
|`show spanning-tree`|STP state per VLAN.|Identify root, blocked ports.|
|`show interfaces trunk`|Displays trunking ports, allowed VLANs, native VLAN.||
|`show vlan brief`|VLAN-to-port mapping.||
|`debug spanning-tree events`|Live STP changes (warning — CPU intensive).|Lab use only.|
|`show cdp neighbors`|Direct Cisco neighbors and interfaces.|Use LLDP for multi-vendor.|

## 🔐 8. Security and Table-Protection Mechanisms

|Feature|Function|
|---|---|
|**Port Security**|Limit MAC addresses per port (`switchport port-security mac-address`). Violations → shutdown/restrict/protect.|
|**BPDU Guard / Root Guard**|Protect STP topology.|
|**Storm Control**|Limit broadcast/multicast percentage per port.|
|**DHCP Snooping**|Prevent rogue DHCP servers.|
|**DAI (Dynamic ARP Inspection)**|Drops spoofed ARP packets using DHCP Snooping DB.|
|**IP Source Guard**|Blocks traffic with mismatched IP-MAC.|

## 🧠 9. Aging and Table Maintenance (Dirty Secrets)

- Each new frame refreshes the entry’s **aging timer**.
    
- Excessive flooding → **CAM table overflow**, can lead to **MAC flapping** or DoS.
    
- Attackers can exploit with “MAC flooding” to push frames to CPU (sniffing risk).
    
- Mitigation → Port Security + storm control + disable unused ports.
    
---
## 🧩 10. Hardware Internals Overview

|Component|Function|
|---|---|
|**Backplane/Fabric ASIC**|Connects all port ASICs; defines throughput (e.g. 320 Gbps switch fabric).|
|**Port ASIC**|Performs lookups, encapsulation/decapsulation, QoS marking.|
|**CPU (Control Plane)**|Handles management protocols (STP, ARP, SNMP, SSH).|
|**RAM (DRAM)**|Holds running-config, tables copied from ASIC to CPU view.|
|**Flash**|Stores IOS image and config backups.|
|**NVRAM**|Startup-config.|
|**ROMMON**|Bootloader / diagnostics.|

## ⚙️ 11. Performance Counters & Monitoring

|Counter|Monitored by|Use|
|---|---|---|
|**CRC Errors**|`show interfaces counters errors`|Layer 1 faults, bad cable.|
|**Collisions / Late Collisions**|Half-duplex links|Duplex mismatch detector.|
|**Input/Output Errors**|Hardware fault or congestion|Troubleshooting.|
|**Interface Utilization**|SNMP (OIDs ifInOctets, ifOutOctets)|Baselines, NetFlow analysis.|

---

---

## 🔧 12. Quick Configuration Reference (CCNA Style)

```bash
# Configure VLAN and access port
SW1(config)# vlan 10
SW1(config-vlan)# name SALES
SW1(config)# int f0/2
SW1(config-if)# switchport mode access
SW1(config-if)# switchport access vlan 10

# Configure trunk
SW1(config)# int g0/1
SW1(config-if)# switchport mode trunk
SW1(config-if)# switchport trunk allowed vlan 10,20,99
SW1(config-if)# switchport trunk native vlan 99

# Enable Port Security
SW1(config)# int f0/2
SW1(config-if)# switchport port-security
SW1(config-if)# switchport port-security maximum 2
SW1(config-if)# switchport port-security violation restrict
SW1(config-if)# switchport port-security mac-address sticky

```

## ⚡ 13. Summary Cheat Table

|Concept|Key Points|
|---|---|
|**Switch Layer**|Layer 2 (Data Link).|
|**Forwarding Basis**|MAC address lookup in CAM.|
|**Hardware Engine**|ASIC + Backplane.|
|**Flood Types**|Unknown Unicast, Broadcast, Multicast.|
|**Loop Protection**|STP/RSTP/MSTP.|
|**MAC Aging**|~300 s (default).|
|**Security Protections**|Port Security, BPDU Guard, DAI, Storm Control.|
|**Table Verification**|`show mac address-table`, `show vlan`, `show spanning-tree`.|

