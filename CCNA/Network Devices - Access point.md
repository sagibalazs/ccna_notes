

## Network Devices - Access points

# 🧭 TL;DR — Cisco Wireless LAN Controller (WLC)

| **Topic**                   | **Key Summary**                                                                                                                                                                                                                                                                  |
| --------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Purpose**                 | Central management system that controls multiple lightweight APs via **CAPWAP** tunnels (Control and Provisioning of Wireless Access Points). Handles configuration, security, and client sessions.                                                                              |
| **OSI Layer**               | Operates at **Layer 3** (control plane management, tunnel termination) but interacts with Layer 2 frames (SSID ↔ VLAN mapping).                                                                                                                                                  |
| **Control vs Data Tunnel**  | - **Control Tunnel**: UDP 5246 – configuration, management, and keepalives.  <br>- **Data Tunnel**: UDP 5247 – carries client traffic when in “central switching mode.”                                                                                                          |
| **AP Modes**                | - **Local Mode:** APs send all traffic via WLC.  <br>- **FlexConnect:** APs locally switch data (remote sites).  <br>- **Monitor:** Passive scanning.  <br>- **Sniffer:** Packet capture.  <br>- **Bridge / Mesh:** Wireless backhaul.                                           |
| **SSID ↔ VLAN Mapping**     | Each SSID = one WLAN interface, mapped to a VLAN on the wired switch trunk.                                                                                                                                                                                                      |
| **Authentication**          | Supports WPA2/WPA3, 802.1X (RADIUS), local EAP, or PSK. WLC communicates with **AAA server** for Enterprise authentication.                                                                                                                                                      |
| **WLC Interfaces**          | - **Management Interface:** Control & GUI/CLI.  <br>- **AP Manager:** Handles CAPWAP joins (merged with management on modern WLCs).  <br>- **Dynamic Interface:** Maps SSIDs to VLANs.  <br>- **Virtual Interface:** Used for guest mobility, DHCP relay, and WebAuth redirects. |
| **Redundancy**              | N+1, SSO (stateful switchover), or clustering on newer models (e.g. 9800).                                                                                                                                                                                                       |
| **Software**                | Runs **Cisco AireOS** (legacy) or **IOS-XE** (new Catalyst 9800 series).                                                                                                                                                                                                         |
| **Licensing**               | Managed by AP count (e.g., 25/50/100 APs). In DNA Center-managed environments, licensing integrates automatically.                                                                                                                                                               |
| **Common Deployment Types** | - **Centralized:** WLC in datacenter, APs in all branches.  <br>- **Embedded:** AP acts as WLC (EWC).  <br>- **Cloud:** Managed by Meraki Dashboard.  <br>- **FlexConnect:** Remote APs switch traffic locally to VLANs.                                                         |
| **Advantages**              | Centralized policy, simplified management, consistent security, client roaming control.                                                                                                                                                                                          |
| **Disadvantages**           | Controller dependency (failure = outage if no HA), cost, latency for tunneled traffic.                                                                                                                                                                                           |

# 🧩 Typical WLC CLI Commands

_(Cisco AireOS / Catalyst 9800 — both used in CCNA and real-world enterprise)_

---

### 🔧 Basic System Configuration

```bash
# Set hostname, domain, and admin credentials
config system name WLC1
config system country-code AT
config country AT both
config system timezone CET
config passwd admin mySecurePass123
```

🖧 Management and Interfaces

```bash
# Show all interfaces and VLAN bindings
show interface summary

# Configure management IP
config interface address management 192.168.10.10 255.255.255.0 192.168.10.1

# Bind dynamic interface to VLAN
config interface create dynamic WLAN10 vlan10
config interface address WLAN10 192.168.10.11 255.255.255.0 192.168.10.1
```

📡 WLAN (SSID) Creation

```bash
# Create WLAN profile
config wlan create 1 CCNA-LAB CCNA-LAB
# Enable WLAN
config wlan enable 1
# Map WLAN to interface
config wlan interface 1 WLAN10
# Assign security (WPA2 + PSK)
config wlan security wpa akm psk set-key ascii MyWLANpass123 1
config wlan security wpa enable 1
```

🔐 Security and Authentication

```bash
# Set authentication type
config wlan security 802.1X enable 1
# Define RADIUS server
config radius auth add 1 192.168.20.10 1812 ascii radiuskey123
# Assign server to WLAN
config wlan radius_server auth 1 1
```

🌐 AP Join and CAPWAP Control

```bash
# Check AP join status
show ap summary
show ap config general <AP_NAME>

# Manually define controller IP for APs
capwap ap controller ip address 192.168.10.10

# Debug CAPWAP join issues
debug capwap events enable
debug capwap errors enable
debug lwapp events enable
```

🎛️ Monitoring and Troubleshooting

```bash
# Show all connected clients
show client summary
# Display details for one client
show client detail <MAC>
# Check WLAN operational status
show wlan summary
# Show RF/channel details
show ap auto-rf 802.11a summary
# Show system resource usage
show cpu
show memory
```

💾 Save and Backup

```bash
# Save running config
save config
# Export backup
transfer upload datatype config
transfer upload mode tftp
transfer upload serverip 192.168.10.20
transfer upload filename WLC-Backup.cfg
```

# ⚙️ TL;DR CLI Mini-Cheat Sheet

| **Task**            | **Command / Path**                                          |
| ------------------- | ----------------------------------------------------------- |
| Create SSID         | `config wlan create <ID> <SSID> <Profile>`                  |
| Enable WLAN         | `config wlan enable <ID>`                                   |
| Map to VLAN         | `config wlan interface <ID> <interface>`                    |
| Set WPA2 Key        | `config wlan security wpa akm psk set-key ascii <key> <ID>` |
| Show clients        | `show client summary`                                       |
| Show APs            | `show ap summary`                                           |
| Show interfaces     | `show interface summary`                                    |
| Show RADIUS servers | `show radius summary`                                       |
| Debug CAPWAP        | `debug capwap events enable`                                |
| Save config         | `save config`                                               |


---
----






## 🧠 1. Role and Function of an Access Point (AP)

An **Access Point (AP)** is a **Layer 2 device** that connects **wireless clients** (stations) to a **wired LAN**.  
It converts 802.11 wireless frames ↔ 802.3 Ethernet frames, bridging the two media types.

**Core functions**

|Function|Description|
|---|---|
|**Wireless bridge**|Forwards traffic between WLAN and LAN segments.|
|**Association & authentication**|Manages client connections, authentication (WPA2-Enterprise, 802.1X), and encryption.|
|**RF management**|Transmits/receives on specific channels, controls power, monitors signal quality.|
|**Roaming coordination**|Works with WLAN controller to enable seamless client roaming.|
|**QoS**|Classifies and prioritizes wireless frames (WMM).|
|**Security enforcement**|WPA2/3 encryption, MAC filtering, rogue AP detection.|

## 🧩 2. Classification

|Type|Description|Example Use|
|---|---|---|
|**Standalone (Autonomous AP)**|Configured locally via CLI/Web; operates independently.|Small offices, home networks.|
|**Controller-based (Lightweight AP)**|Managed centrally by a **WLC (Wireless LAN Controller)** via CAPWAP (UDP 5246/5247).|Enterprise networks.|
|**Mesh AP**|Uses wireless backhaul links between APs instead of Ethernet cabling.|Outdoor or hard-wired areas.|
|**Cloud-Managed AP**|Managed through vendor cloud platform (e.g. Cisco Meraki, Aruba Instant On).|Distributed branch networks.|
|**Repeater/Extender**|Extends coverage by rebroadcasting existing signal.|Home, temporary coverage.|
|**Workgroup Bridge (WGB)**|Connects wired devices (printers, IP cameras) to WLAN.|Legacy or industrial equipment.|

## ⚙️ 3. Hardware Components and Attributes

|Component|Description|
|---|---|
|**CPU / Processor**|Handles packet processing, encryption, CAPWAP tunneling. Usually ARM or MIPS SoC, ~600 MHz–1.6 GHz.|
|**RAM**|Volatile memory (128 MB–2 GB) for active configs, frame buffers.|
|**Flash / NVRAM**|Non-volatile storage for IOS / firmware, startup-config.|
|**Backplane / Bus**|Internal data path; defines switching capacity (e.g. 1–2 Gbps for SMB APs, >5 Gbps for Wi-Fi 6 E).|
|**Ports**|1× Gigabit Ethernet (PoE+ 802.3af/at/bt), sometimes 2× Gigabit or 2.5 Gbps Uplink, USB, console (RJ-45 or micro-USB).|
|**Radio modules**|Usually dual-band (2.4 GHz/5 GHz); tri-radio or 6 GHz Wi-Fi 6E models. Supports multiple spatial streams (MIMO 2×2, 4×4, 8×8).|
|**Antennas**|Internal or external (omni / directional / sector). Gain 2–9 dBi typical.|
|**Power**|48 V DC via PoE or external adapter. Power draw 7–25 W typical.|
|**Cooling**|Passive; high-end units may include temperature sensors for environmental monitoring.|

## 🧩 4. Software / OS Features

|Category|Details|
|---|---|
|**Operating system**|Cisco IOS XE (Catalyst APs), AireOS (LWAPP), Meraki Cloud OS, or vendor-specific Linux variants.|
|**Management interfaces**|CLI, Web GUI, SNMP, REST API, or CAPWAP via controller.|
|**Security**|WPA2-PSK, WPA2-Enterprise (802.1X), WPA3-SAE/Enterprise, 802.11i, EAP-TLS/PEAP, rogue detection, ACLs.|
|**QoS / WMM**|Implements IEEE 802.11e for traffic prioritization.|
|**RF Management**|DCA (Dynamic Channel Assignment), TPC (Transmit Power Control), CleanAir (RF interference analysis).|
|**Mobility / Roaming**|Fast Secure Roaming (802.11r / k / v).|
|**Monitoring**|SNMP v3, Syslog, NetFlow, NMS integration (Prime Infrastructure / DNA Center).|
|**Firmware Upgrade**|Local TFTP/HTTP or OTA via WLC/cloud.|

## ⚖️ 5. L2 vs L3 Operation and Differences to Switches

|Feature|**Access Point (L2 device)**|**L2 Switch**|**L3 Switch**|
|---|---|---|---|
|Primary role|Bridge between wired LAN and wireless LAN|Segment wired LAN ports|Route between VLANs/subnets|
|OSI layer|2 (Data Link) – wireless bridge|2 (Data Link)|2 + 3 (Network)|
|Interface type|Radio + Ethernet|Ethernet only|Ethernet, VLAN interfaces (SVIs)|
|Control|Wireless controller or standalone|Local config|Local or dynamic routing protocols|
|Management|CAPWAP/WLC|CLI/SNMP|CLI/SNMP + routing tables|
|Traffic path|Converts 802.11↔802.3|Forwards Ethernet frames|Routes IP packets|
|Typical use|Mobile client access|LAN segmentation|Inter-VLAN routing / core routing|

## 🏢 6. Use Cases, Pros and Cons

|Use Case|Pros|Cons|
|---|---|---|
|**Small Office / Home AP (SOHO)**|Easy setup, integrated DHCP/router|Limited capacity, low security|
|**Enterprise Controller-based AP**|Central mgmt, scalability, RF optimization|Requires WLC, license cost|
|**Mesh / Outdoor AP**|Rapid deployment, flexible|Higher latency, throughput loss|
|**Cloud-Managed AP**|Zero-touch deployment, remote updates|Cloud dependency, subscription|

## 🧰 7. CLI Commands (Cisco IOS / CAPWAP)

**Standalone AP basic setup**

```bash
enable
configure terminal
hostname AP1
interface gigabitEthernet0
 ip address 192.168.10.2 255.255.255.0
 no shutdown
exit
interface dot11radio0
 ssid CCNA-Lab
  authentication open
  guest-mode
 exit
 channel 6
 speed 11
 no shutdown
exit
ip default-gateway 192.168.10.1
wr mem
```

Lightweight AP join controller

```bash
capwap ap ip address 192.168.10.10 255.255.255.0 192.168.10.1
capwap ap controller ip address 192.168.10.100
show capwap client config
```

Useful show/debug

```bash
show dot11 associations
show interface dot11radio0
show ip interface brief
debug capwap events
debug dot11 authentication
```

## 🧮 8. Summary (TL;DR)

|Category|Key Points|
|---|---|
|**Role**|Bridges wireless and wired networks (Layer 2)|
|**OSI Layer**|Primarily L2 (Data Link), some L3 mgmt|
|**Managed by**|WLC (CAPWAP) or standalone|
|**HW**|Radios (2.4/5/6 GHz), CPU, RAM, Flash, PoE Ethernet port|
|**SW features**|WPA2/3, 802.1X, QoS, VLAN tagging, RF mgmt, roaming|
|**Difference to Switch**|Provides **wireless access**, not wired switching; usually a bridge, not a router|
|**Use Cases**|SOHO, Enterprise, Mesh, Cloud, Outdoor|
|**CLI Focus**|Interface setup, SSID definition, CAPWAP join, monitoring|

## 🧱 1. Cisco Catalyst 9120AX (Enterprise Wi-Fi 6)

_(High-end controller-based or cloud-managed AP, used in large organizations)_

|**Feature**|**Specification**|**Notes / Secrets**|
|---|---|---|
|**Model**|Cisco Catalyst 9120AXI / AXE|Part of the Cisco 9100 Series Wi-Fi 6 (802.11ax) line|
|**Architecture**|**Controller-based (CAPWAP)** or **EWC (Embedded WLC)** mode|EWC can manage ≈ 100 APs without external WLC|
|**Radios**|2 × Wi-Fi 6 (2.4 GHz 4×4:4 MIMO + 5 GHz 4×4:4 MIMO) + 1 × BLE/Zigbee IoT radio|IoT radio can be repurposed for scanning|
|**CPU**|Quad-core ARM A53 @ 1.8 GHz|Handles CAPWAP, encryption, CleanAir, telemetry|
|**RAM**|2 GB DDR4||
|**Flash/NVRAM**|4 GB eMMC Flash|Stores IOS-XE image + config|
|**Backplane / Throughput**|≈ 5 Gbps aggregate (Wi-Fi + wired)|Needs 2.5 G uplink for full Wi-Fi 6 capacity|
|**Ethernet Ports**|1 × 2.5 Gb Multi-Gig RJ-45 (PoE+) + 1 × RJ-45 console|Some variants add USB 2.0|
|**Power**|PoE+ (802.3at Class 4) ≈ 20–25 W draw|Optional DC 48 V adapter (~25 W)|
|**Operating System**|Cisco IOS-XE Wireless|Same OS base as Catalyst 9800 WLC|
|**Management**|DNA Center / CLI / WebUI (EWC)||
|**Extras**|CleanAir Pro (RF analysis), OFDMA, 1024-QAM, 802.11r/k/v, 160 MHz channels|Supports 802.1X, WPA3-Enterprise, Fast Roaming|

🧩 **Use case:** Core enterprise AP for hospitals, universities, airports – thousands of concurrent users, controller-based automation, telemetry, and integration with Cisco DNA Center.

---

## 🏠 2. Ubiquiti UniFi U6 Lite (SMB / Home Wi-Fi 6)

_(Simple PoE-powered standalone/cloud AP — popular in small offices and home labs)_

|**Feature**|**Specification**|**Notes / Secrets**|
|---|---|---|
|**Model**|UniFi U6 Lite|Entry-level Wi-Fi 6 (802.11ax) AP|
|**Architecture**|Standalone (UniFi Network Controller or Cloud Key)|Controller runs on PC, VM, or Cloud Key|
|**Radios**|Dual-band (2.4 GHz 2×2:2 MIMO + 5 GHz 2×2:2 MIMO)|Up to 1.5 Gbps aggregate throughput|
|**CPU**|Dual-core ARM Cortex-A53 @ 1 GHz|SoC-based (QCA chipset)|
|**RAM**|256 MB DDR3||
|**Flash/NVRAM**|32 MB SPI Flash|Stores firmware + config|
|**Backplane**|1 GbE|Limited by single 1 G uplink|
|**Ports**|1 × RJ-45 Gigabit (PoE)||
|**Power**|802.3af (Class 3) ≈ 6.5 W typical draw|Compatible with low-cost PoE switches|
|**OS**|UniFi Network OS||
|**Management**|Web / Mobile App / UniFi Controller||
|**Extras**|VLAN tagging, guest portal, WPA3, band steering|Very quiet, passive cooling, no console port|

🧩 **Use case:** Home lab, café, small office – 10–50 clients, cloud monitoring, silent operation, very low power (fits cheap PoE switches).

---

## ⚡ 3. PoE Standards & Classes (Power over Ethernet)

|**Standard**|**Year**|**Alias / Marketing**|**Power Class**|**Max Power at PSE (Switch)**|**Power Available at PD (Device)**|**Typical Use / Devices**|
|---|---|---|---|---|---|---|
|**802.3af**|2003|PoE|Class 0-3|15.4 W|12.95 W|Basic APs (U6 Lite), IP phones, cams|
|**802.3at**|2009|PoE+|Class 4|30 W|25.5 W|High-end APs (Catalyst 9100), PTZ cams|
|**802.3bt Type 3**|2018|PoE++ / UPoE|Class 5-6|60 W|51 W|Wi-Fi 6 E APs, thin clients, POS|
|**802.3bt Type 4**|2018|High Power PoE / UPoE+|Class 7-8|90–100 W|71–90 W|Lighting controllers, industrial switches, panels|

## ⚙️ TL;DR

|Aspect|**Enterprise AP (Cisco 9120AX)**|**SMB AP (Ubiquiti U6 Lite)**|
|---|---|---|
|**Performance**|Multi-Gig, 4×4 MIMO, 2.5 G uplink|2×2 MIMO, 1 G uplink|
|**Power**|802.3at PoE+ ≈ 20–25 W|802.3af PoE ≈ 6 W|
|**Management**|Controller / EWC / DNA Center|Cloud / Local Controller|
|**Use Case**|Enterprise / Campus|Small Office / Home|
|**Cost Range**|€700–€1200|€90–€120|

## 🧭 1. WLAN Architecture Components (CCNA Core)

|Component|Function|
|---|---|
|**WLC (Wireless LAN Controller)**|Centralized management for lightweight APs; handles authentication, channel/power allocation, roaming, and statistics.|
|**CAPWAP Protocol**|Control and Provisioning of Wireless Access Points — tunnel between AP and WLC (UDP 5246 control, 5247 data).|
|**LWAPP (legacy)**|Cisco’s pre-standard version of CAPWAP — still seen in older devices.|
|**RADIUS Server**|Provides 802.1X authentication for WPA2-Enterprise/WPA3-Enterprise.|
|**AAA (Authentication, Authorization, Accounting)**|Defines how users authenticate and what they can access; managed on WLC or external server.|

## 🔐 2. WLAN Security Models and Encryption

|Security Mode|Key Features|CCNA Note|
|---|---|---|
|**Open**|No encryption; anyone can connect.|Used for guest or testing SSIDs.|
|**WEP (Deprecated)**|RC4-based; easily cracked.|_Never use!_|
|**WPA / WPA2 (802.11i)**|TKIP (WPA), AES-CCMP (WPA2).|WPA2 is still common in enterprise.|
|**WPA3 (2018)**|SAE for personal; 192-bit AES-GCMP for enterprise.|Required for Wi-Fi 6 certification.|
|**Enterprise Mode (802.1X)**|Uses RADIUS; user credentials verified per session.|Seen in corporate deployments.|

## 📡 3. RF & Wireless Concepts to Remember

|Concept|Description|
|---|---|
|**Channel Widths**|20, 40, 80, 160 MHz (Wi-Fi 5/6). Wider = faster but more interference.|
|**MIMO / MU-MIMO**|Multiple Input/Output antennas; parallel data streams increase throughput.|
|**Beamforming**|Directional signal focusing for improved range and SNR.|
|**RSSI & SNR**|Signal strength vs. noise; key for placement and troubleshooting.|
|**Band Steering**|Encourages capable clients to use 5 GHz instead of 2.4 GHz.|
|**DFS Channels**|5 GHz channels shared with radar; AP must vacate if radar detected.|
|**Roaming**|802.11k (neighbor report), 802.11v (network assist), 802.11r (fast transition).|

## 🧮 4. VLAN & Trunking Integration

- Each **SSID** maps to a **VLAN** (logical separation).
    
- APs use **802.1Q trunk links** to carry multiple SSIDs to the switch.
    
- Example (CLI on switch):

```bash
interface gig0/1
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30
 spanning-tree portfast trunk

```

## 🧰 5. Troubleshooting Commands (CLI)

|Command|Description|
|---|---|
|`show dot11 associations`|List connected clients.|
|`show controllers dot11radio 0`|Hardware and channel diagnostics.|
|`debug capwap events`|Lightweight AP join process.|
|`debug dot11 authentication`|Client connection process.|
|`show wlan summary`|SSID and security overview (on WLC).|

## ⚡ 6. Exam Focus Summary

✅ Know:

- AP types (autonomous vs lightweight vs mesh).
    
- CAPWAP ports (UDP 5246/5247).
    
- PoE power classes and cable requirements.
    
- WLAN components (AP, WLC, RADIUS).
    
- Security standards (WPA2/WPA3, 802.1X).
    
- RF fundamentals and 2.4 GHz vs 5 GHz differences.
    
- VLAN mapping and trunk operation.

![[Pasted image 20251112230028.png]]

You’ve now covered the **Access Point** topic fully and at a **CCNA-professional depth** — both theory and practical.  
Only a few **optional advanced or adjacent areas** remain if you want to go _beyond the exam_ or prepare for real-world enterprise environments:

---

## 🧩 Optional Advanced Areas

|Area|Why It Matters|Short Description|
|---|---|---|
|**RF Site Survey & Planning**|Real-world deployment accuracy|Tools like Ekahau / Cisco Prime to measure RSSI, SNR, and interference. Channel plan, power levels, AP placement.|
|**Cisco CleanAir / Spectrum Analysis**|Troubleshooting & optimization|Identifies non-Wi-Fi interference (microwave, Bluetooth, etc.) using integrated hardware sensors.|
|**Wireless QoS (WMM / 802.11e)**|Voice/video over WLAN|Defines four access categories (Voice, Video, Best Effort, Background). Important for VoIP over Wi-Fi.|
|**Fast Secure Roaming (802.11r/k/v)**|Seamless mobility|Reduces authentication delay when clients move between APs.|
|**Multigigabit Ethernet (2.5G / 5GBASE-T)**|Wi-Fi 6+ backhaul|Needed when AP throughput exceeds 1 Gbps. Supported by Catalyst 9000 switches.|
|**WPA3 Enterprise (192-bit mode)**|Security compliance|Mandated in high-security environments; uses EAP-TLS and GCMP-256.|
|**IoT / BLE Integration**|Converged infrastructure|New APs include BLE/Zigbee radios for sensors or asset tracking.|
|**Cloud-managed WLANs**|Simplified ops|Cisco Meraki, Aruba Central, TP-Link Omada — zero-touch provisioning and telemetry.|
|**Wireless Troubleshooting Tools**|Field readiness|Wireshark (radiotap), AirMagnet, NetScout AirCheck for capturing and decoding frames.|

## 📘 CCNA Exam Tip Summary

For the **CCNA 200-301**, make sure you can:

- Explain the **difference between autonomous and controller-based APs**.
    
- Identify **CAPWAP ports** (UDP 5246/5247).
    
- Map **SSID ↔ VLAN** relationships and understand **802.1Q trunks**.
    
- Describe **WPA2 vs WPA3** and **802.1X / RADIUS authentication**.
    
- Understand **PoE standards (af / at / bt)** and **power negotiation**.
    
- Read or interpret simple **WLAN architecture diagrams** (like the one you made).
