
## L3 Switch

Perfect — below you’ll get the **complete Layer 3 Switch (Multilayer Switch) reference**, same structure as for router + L2 switch:

- 🔹 differences to L2 switch    
- 🔹 use-cases / pros / cons    
- 🔹 concise overall summary (TL;DR table)    
- 🔹 full CLI command section with short explanations (emphasis on L3-specific ones)    

---

## ⚖️ 1. L3 Switch vs L2 Switch — Key Differences

| Feature                       | Layer 2 Switch                            | Layer 3 Switch                                 |
| ----------------------------- | ----------------------------------------- | ---------------------------------------------- |
| **Main OSI layer**            | L2 – switching (MAC)                      | L2 + L3 – switching + routing                  |
| **Forwarding decision**       | Based on MAC address → CAM                | Based on IP + MAC → TCAM / CEF                 |
| **Routing capability**        | ❌ None (except default-gateway)           | ✅ Static & dynamic (OSPF/EIGRP/RIP/VRF-Lite)   |
| **Inter-VLAN communication**  | Needs external router (router-on-a-stick) | Internal SVI interfaces route at wire-speed    |
| **Default gateway for VLANs** | External router                           | The switch itself (SVI per VLAN)               |
| **Control plane**             | STP, VTP, LLDP, CDP only                  | STP + Dynamic Routing + CEF + ACL/QoS hardware |
| **Performance**               | Wire-speed L2 switching                   | Wire-speed L3 routing (ASIC-based)             |
| **CPU usage**                 | Handles mgmt traffic only                 | Handles protocols (OSPF, EIGRP, etc.)          |
| **Use position**              | Access layer                              | Distribution / Core / High-end Access          |
| **Cost**                      | Low                                       | Medium → High                                  |

## 🏗️ 2. Typical Use Cases

|Scenario|Why L3 Switch fits|
|---|---|
|**Enterprise campus**|Fast inter-VLAN routing without router-on-a-stick|
|**Distribution/core layer**|Aggregates VLANs from many access switches|
|**Data center leaf/spine**|Hardware routing between racks (VRF/VXLAN)|
|**VoIP and QoS edge**|Per-VLAN policing, marking, priority queuing|
|**Small MPLS / VRF-Lite core**|Segmentation between departments or tenants|

## ✅ 3. Pros and Cons

|✅ Advantages|⚠️ Limitations|
|---|---|
|Very high throughput (wire-speed routing)|Expensive compared to L2 switch|
|Fewer devices (no external router needed)|Limited WAN features (NAT, tunneling basic)|
|Simplifies inter-VLAN routing|TCAM table limits for ACL/route entries|
|Supports dynamic routing protocols|No advanced services (QoS complexity, firewall)|
|Redundancy – HSRP/VRRP/GLBP|Less flexible interface types (WAN serial rare)|
|Hardware ACL + QoS in ASIC|Harder to debug hardware forwarding paths|

## 🧠 4. TL;DR Summary

|Category|Details|
|---|---|
|**Definition**|Multilayer switch that performs both L2 switching and L3 routing in hardware|
|**Goal**|Eliminate router-on-a-stick bottlenecks; enable fast inter-VLAN communication|
|**Routing Protocols**|Static, RIP, EIGRP, OSPF, VRF-Lite, some BGP|
|**Core Hardware**|ASIC fabric with CAM/TCAM lookup engines; CPU for control plane|
|**Forwarding Method**|Cisco Express Forwarding (CEF) hardware-based|
|**Key Tables**|CAM (MAC), TCAM (ACL/QoS/routes), ARP table, Routing table|
|**Performance**|Tbps backplane, < 1 µs latency|
|**Software**|IOS XE / NX-OS with `ip routing` enabled|
|**Security**|ACLs, 802.1X, DHCP snooping, DAI, IP Source Guard|
|**Redundancy**|HSRP/VRRP/GLBP, StackWise, VSS, EtherChannel|
|**Best Placement**|Enterprise core / distribution / DC leaf|
|**Not for**|WAN edge, heavy NAT/VPN/firewall duties|

## 💻 5. CLI Command Summary (CCNA / Real Lab)

> ⚠️ Commands in bold are **L3-specific** (not available on pure L2 switches).

```bash
# --- Basic L3 Enablement ---------------------------------------
Switch(config)# ip routing                # <-- enable Layer-3 routing globally
Switch(config)# show ip route             # display routing table (CEF entries)
Switch(config)# show ip cef               # verify hardware CEF forwarding

# --- VLAN & SVI Creation ---------------------------------------
Switch(config)# vlan 10
Switch(config-vlan)# name SALES
Switch(config)# interface vlan 10         # <-- create Switch Virtual Interface
Switch(config-if)# ip address 192.168.10.1 255.255.255.0
Switch(config-if)# no shutdown

Switch(config)# interface vlan 20
Switch(config-if)# ip address 192.168.20.1 255.255.255.0
Switch(config-if)# no shutdown

# --- Assign Access Ports ---------------------------------------
Switch(config)# interface range g0/1 - 12
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 10

# --- Trunk & Uplink Configuration ------------------------------
Switch(config)# interface g0/24
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport trunk allowed vlan 10,20,30

# --- Static Routing --------------------------------------------
Switch(config)# ip route 0.0.0.0 0.0.0.0 192.168.100.1
Switch(config)# ip route 10.10.0.0 255.255.0.0 192.168.20.254

# --- Dynamic Routing (Example: OSPF) ---------------------------
Switch(config)# router ospf 1
Switch(config-router)# router-id 1.1.1.1
Switch(config-router)# network 192.168.0.0 0.0.255.255 area 0
Switch(config-router)# passive-interface default
Switch(config-router)# no passive-interface vlan10

# --- Redundancy / Gateway Protocols ----------------------------
Switch(config)# interface vlan 10
Switch(config-if)# standby 1 ip 192.168.10.254     # HSRP example
Switch(config-if)# standby 1 priority 110
Switch(config-if)# standby 1 preempt

# --- QoS & ACL (Hardware Enforced via TCAM) -------------------
Switch(config)# access-list 101 permit tcp 192.168.10.0 0.0.0.255 any eq 80
Switch(config)# interface vlan 10
Switch(config-if)# ip access-group 101 in

Switch(config)# class-map match-any VOICE
Switch(config-cmap)# match ip dscp ef
Switch(config)# policy-map QOS-POLICY
Switch(config-pmap)# class VOICE
Switch(config-pmap-c)# priority percent 30
Switch(config)# interface g0/1
Switch(config-if)# service-policy output QOS-POLICY

# --- Verification Commands ------------------------------------
Switch# show vlan brief
Switch# show ip interface brief
Switch# show interfaces status
Switch# show standby brief                # verify HSRP/VRRP/GLBP
Switch# show platform hardware capacity   # check TCAM/CAM utilization
Switch# show spanning-tree summary
Switch# show processes cpu sorted
Switch# show memory statistics
Switch# show version
```

### 🧩 L3-Only Commands Highlight

| Command                            | Description                                  |
| ---------------------------------- | -------------------------------------------- |
| `ip routing`                       | Activates the Layer-3 routing functionality. |
| `interface vlan X` + `ip address`  | Creates SVIs – virtual interfaces per VLAN.  |
| `ip route …`                       | Static route definition.                     |
| `router ospf …` / `router eigrp …` | Enables dynamic routing protocols.           |
| `show ip route / show ip cef`      | View hardware forwarding tables.             |
| `standby` / `vrrp` / `glbp`        | First-hop redundancy configuration.          |
| `show platform hardware capacity`  | Inspect ASIC resource usage (TCAM slots).    |
## 🧾 6. Grand Summary – Routers vs L2 Switch vs L3 Switch

| Feature           | Router                      | L3 Switch                            | L2 Switch        |
| ----------------- | --------------------------- | ------------------------------------ | ---------------- |
| Primary Role      | WAN edge routing & services | High-speed LAN routing & aggregation | LAN connectivity |
| OSI Layers        | 3 (4–7 with firewall)       | 2 + 3                                | 2                |
| Forwarding Engine | CPU (software)              | ASIC (hardware)                      | ASIC (hardware)  |
| Routing Protocols | All (OSPF, BGP, EIGRP)      | Limited subset (enterprise)          | None             |
| NAT/VPN           | Full support                | Limited/None                         | None             |
| Ports             | Low density, varied media   | High density Gig/10G                 | High density Gig |
| Performance       | Mbps–Gbps range             | Multi-Tbps                           | Multi-Tbps       |
| Cost              | High                        | Medium–High                          | Low              |
| Best Placement    | WAN gateway                 | Distribution/Core                    | Access           |

### ⚡ Final Takeaway

> 🧩 A **Layer 3 Switch** is a switch that grew a router’s brain but kept a switch’s body.  
> It routes at hardware speed, enforces ACL/QoS in ASICs, and bridges LAN segments faster than any router could.  
> Use it for **campus cores and distribution**, not for WAN edge duties.

---
---
---

# Topology (1× L3 switch core, 1× L2 access, 1× WAN router, 3 PCs)

```bash
             (WAN/Upstream)
                 R1
            Gi0/0 10.0.0.2/30
                  |
             Gi0/1 10.0.0.1/30
               L3-SW1  (multilayer switch)
     SVI VLAN10 192.168.10.1/24   SVI VLAN20 192.168.20.1/24
     SVI VLAN99 192.168.99.1/24   Trunk to L2-SW1 (VLANs 10,20,99)
                  |
               L2-SW1 (access)
        Fa0/1→PC-A (VLAN10)   Fa0/2→PC-B (VLAN20)   Fa0/3→PC-MGMT (VLAN99)

```

## Addressing plan

| VLAN | Purpose | Subnet          | Gateway (SVI) |
| ---- | ------- | --------------- | ------------- |
| 10   | Sales   | 192.168.10.0/24 | 192.168.10.1  |
| 20   | Eng     | 192.168.20.0/24 | 192.168.20.1  |
| 99   | Mgmt    | 192.168.99.0/24 | 192.168.99.1  |

# Configs (copy/paste)

> Interfaces might be `g0/1`, `g1/0/1`, etc. in your PT image; adjust names if needed.  
> The “trunk” below assumes `Gi1/0/48` between L3-SW1 and L2-SW1.

### 1) L3-SW1 (multilayer switch)

```bash
enable
conf t
hostname L3-SW1
no ip domain-lookup
ip routing                                ! <-- turn on L3 forwarding

! VLAN DB
vlan 10
 name SALES
vlan 20
 name ENG
vlan 99
 name MGMT
exit

! SVIs (Inter-VLAN routing gateways)
interface vlan 10
 ip address 192.168.10.1 255.255.255.0
 no shutdown
interface vlan 20
 ip address 192.168.20.1 255.255.255.0
 no shutdown
interface vlan 99
 ip address 192.168.99.1 255.255.255.0
 no shutdown

! Uplink to WAN router
interface GigabitEthernet0/1
 description TO-R1
 no switchport                           ! <-- routed port
 ip address 10.0.0.1 255.255.255.252
 no shutdown

! Trunk down to access switch
interface GigabitEthernet1/0/48
 description TO-L2-SW1
 switchport
 switchport mode trunk
 switchport trunk allowed vlan 10,20,99
 spanning-tree portfast trunk
 no shutdown

! (Optional) Put local mgmt interface into VLAN99 if using an in-band mgmt port
! interface vlan 99 already created above

! OSPF: advertise all local subnets and the WAN link
router ospf 1
 router-id 1.1.1.1
 passive-interface default
 no passive-interface GigabitEthernet0/1     ! OSPF hello only on WAN link
 network 10.0.0.0 0.0.0.3 area 0
 network 192.168.10.0 0.0.0.255 area 0
 network 192.168.20.0 0.0.0.255 area 0
 network 192.168.99.0 0.0.0.255 area 0
exit

! Basic hygiene
line con 0
 logging synchronous
exec-timeout 0 0
end
wr

```

#### 2) L2-SW1 (access switch)

```bash
enable
conf t
hostname L2-SW1
no ip domain-lookup

! VLANs must exist here too
vlan 10
 name SALES
vlan 20
 name ENG
vlan 99
 name MGMT
exit

! Access ports
interface range FastEthernet0/1
 switchport mode access
 switchport access vlan 10
 spanning-tree portfast
interface range FastEthernet0/2
 switchport mode access
 switchport access vlan 20
 spanning-tree portfast
interface range FastEthernet0/3
 switchport mode access
 switchport access vlan 99
 spanning-tree portfast

! Trunk up to L3-SW1
interface GigabitEthernet0/1
 description TO-L3-SW1
 switchport mode trunk
 switchport trunk allowed vlan 10,20,99
 spanning-tree portfast trunk
 no shutdown

end
wr

```

#### 3) R1 (WAN/edge placeholder router)

```bash
enable
conf t
hostname R1
no ip domain-lookup

interface GigabitEthernet0/0
 description TO-L3-SW1
 ip address 10.0.0.2 255.255.255.252
 no shutdown

! OSPF - inject a default into the campus (for Internet simulation)
router ospf 1
 router-id 2.2.2.2
 network 10.0.0.0 0.0.0.3 area 0
 default-information originate always
exit

! (Optional) a loopback as a test prefix
interface Loopback0
 ip address 203.0.113.1 255.255.255.255
end
wr

```

### 4) PCs (in PT GUI)

- **PC-A**: IP `192.168.10.10` / `255.255.255.0`, GW `192.168.10.1`
    
- **PC-B**: IP `192.168.20.10` / `255.255.255.0`, GW `192.168.20.1`
    
- **PC-MGMT**: IP `192.168.99.10` / `255.255.255.0`, GW `192.168.99.1`

# Quick verification

**On L3-SW1**

```bash
show ip interface brief
show vlan brief
show interfaces trunk
show ip route
show ip ospf neighbor
show ip cef
```

You should see:

- SVIs **up/up** for VLANs 10/20/99
    
- Trunk to L2-SW1 **on** with VLANs allowed
    
- OSPF neighbor with **R1** (FULL)
    
- Default route (`0.0.0.0/0`) learned from R1 via OSPF
    

**Pings**

- From **PC-A → PC-B** (inter-VLAN): should succeed (L3-SW1 routes via SVIs)
    
- From **PC-A → 203.0.113.1** (R1 loopback): should succeed (OSPF)
    
- From **L3-SW1 → 10.0.0.2**: `ping 10.0.0.2`
    

**Troubleshooting cheats**

```bash
show cdp neighbors detail          ! if using CDP in PT images
show spanning-tree summary
show platform hardware capacity    ! check TCAM/CAM (if supported)
debug ip ospf adj                  ! OSPF adjacency issues
```

## Why this lab is “L3 switch specific”

- `ip routing` + **SVIs** doing inter-VLAN routing at wire-speed (ASIC/CEF)
    
- Routed port to the WAN (`no switchport`)
    
- OSPF on the L3 switch (not possible on pure L2)
    
- Trunk to carry multiple VLANs between L3 core and L2 access












---





## 🧭 L3 SWITCH – ROLE AND FUNCTION

### 1. General Concept

A **Layer 3 switch** (also called a _multilayer switch_, MLS) combines the **high-speed frame switching** of a Layer 2 switch with the **packet-routing intelligence** of a router.  
It performs **routing inside hardware (ASICs)** instead of in software like traditional routers, giving wire-speed inter-VLAN routing within a LAN or campus core.

|OSI Layer|Function|Implementation|
|---|---|---|
|Layer 2|MAC learning, forwarding, VLAN segmentation|CAM table + ASIC|
|Layer 3|IP routing between VLANs, routing protocols, ACLs|Hardware-based forwarding (CEF, TCAM)|

### 2. Classification of L3 Switches

|Type|Description|Typical Use|
|---|---|---|
|**Access-layer L3 switch**|Provides inter-VLAN routing and gateway services for smaller networks. Usually fixed-configuration.|Small enterprise floors, branch offices|
|**Distribution-layer (core) L3 switch**|Aggregates multiple access switches, does policy-based routing, ACLs, QoS, load balancing.|Campus backbone, data center aggregation|
|**Data-center fabric switch (NX-OS)**|High-port-density, multi-chassis EtherChannel, virtualization features (VDC, VXLAN).|Spine-leaf architectures|
|**Industrial / ruggedized L3 switch**|Hardened chassis, limited routing features.|OT networks, IoT, industrial environments|

### 3. Role in the Network

- **Inter-VLAN Routing:** replaces router-on-a-stick by routing between VLANs internally via SVIs (Switch Virtual Interfaces).
    
- **Default Gateway:** acts as the first-hop gateway for clients in each VLAN.
    
- **Dynamic Routing:** supports OSPF, EIGRP, RIP, static routes, sometimes BGP.
    
- **Policy Enforcement:** uses ACLs and QoS to manage traffic.
    
- **Aggregation:** collects uplinks from multiple access switches (2–4 × 10G/40G ports).
    
- **High Availability:** supports HSRP/VRRP/GLBP, EtherChannel, and redundant power.
    

---

## ⚙️ 4. Internal Architecture (The “Dirty Secrets”)

### a. **Switch Fabric / Backplane**

- The **backplane** is the internal bus that interconnects all ports and the CPU.
    
- Modern L3 switches use **non-blocking crossbar fabrics** or **dual-plane architectures**, capable of multiple Tbps throughput.
    
- Example: Catalyst 9300 ≈ 480 Gbps switching fabric; Nexus 9000 ≈ > 6 Tbps.
    

### b. **ASICs (Application-Specific Integrated Circuits)**

- Core of performance.
    
- Performs **L2 switching**, **L3 routing lookup**, **ACL**, **QoS**, **NetFlow**, and **SPAN** all in hardware.
    
- Two major lookup engines:
    
    - **CAM (Content Addressable Memory):** for MAC addresses and ARP caching.
        
    - **TCAM (Ternary CAM):** for ACLs, QoS, and route lookups (CEF entries).
        
- **Cisco Express Forwarding (CEF)** stores Layer 3 forwarding tables directly in TCAM, avoiding CPU involvement.
    
- Each port group usually tied to a specific ASIC; internal bandwidth between ASICs handled by fabric modules or crossbars.
    

### c. **CPU and Control Plane**

- General-purpose RISC CPU (ARM, PowerPC, or x86 on higher models).
    
- Handles management traffic, routing protocol processes, SNMP, SSH, spanning-tree negotiation, etc.
    
- Does _not_ forward normal traffic — forwarding occurs in ASIC hardware.
    
- Runs Cisco IOS / IOS XE / NX-OS (depending on platform).

### d. **Memory System**

|Memory Type|Function|
|---|---|
|**RAM / DRAM**|Active IOS image, routing tables, running configuration|
|**Flash / eMMC**|IOS image storage, startup-config|
|**NVRAM**|Stores startup-config (on some models merged into flash)|
|**TCAM / CAM**|Hardware lookup tables for L2/L3 forwarding, ACL, QoS|
|**EEPROM**|Boot loader and device ID (System ID, MAC base address)|

### e. **Ports and Interfaces**

|Port Type|Description|
|---|---|
|**Access Ports**|L2 ports assigned to VLANs|
|**Trunk Ports**|Carry multiple VLANs via 802.1Q tags|
|**Uplink Ports**|10G/25G/40G SFP+, SFP28, QSFP|
|**Management Port**|Out-of-band Ethernet (mgmt0)|
|**Console/USB**|Serial access for local management|

### 5. Routing Process (Simplified)

1. Frame arrives on an access port → ASIC checks destination MAC → recognizes it as destined for an SVI.
    
2. ASIC strips the frame, performs **L3 lookup** in TCAM (CEF).
    
3. The switch modifies L2 headers, decrements TTL, and forwards through the egress ASIC.  
    All this happens in hardware within microseconds — no CPU involvement unless exception traffic (TTL 0, routing updates, etc.).


### 6. Software & OS Characteristics

- **IOS XE / NX-OS / IOS XR** – modular operating systems supporting:
    
    - VRF, Policy-Based Routing, NetFlow, DHCP Snooping, DAI, IP Source Guard.
        
    - STP variants (RSTP, MSTP) and Layer 3 protocols simultaneously.
        
- Support for:
    
    - **CEF (Cisco Express Forwarding)** for hardware routing
        
    - **MPLS/VXLAN** on advanced data-center models
        
    - **SNMPv3, Syslog, NetFlow, RESTCONF** for monitoring
        

---

### 7. Performance Attributes

|Feature|Description|
|---|---|
|**Latency**|Sub-microsecond hardware forwarding|
|**Throughput**|Up to multiple Tbps aggregate|
|**Redundancy**|Dual power, fan trays, supervisor modules|
|**QoS**|Per-port policing, shaping, marking|
|**Security**|DHCP snooping, DAI, IP Source Guard, port security, 802.1X|
|**Virtualization**|VRF-Lite, VLAN-based routing, VDCs on NX-OS|
|**Scalability**|Hundreds of VLANs, thousands of SVIs, millions of routes (on high-end)|

### 8. Comparison with L2 Switch and Router

|Feature|L2 Switch|L3 Switch|Router|
|---|---|---|---|
|Forwarding decision|MAC (L2)|IP (L3) + MAC|IP (L3)|
|Performance|Wire-speed|Wire-speed routing|Slower (software-based)|
|Routing Protocols|None|OSPF, EIGRP, RIP, BGP|All|
|Ports|Many, high-density|Many, high-density|Few, modular WAN ports|
|ASIC Hardware|Yes|Yes (with TCAM for routing)|Often limited / CPU-based|
|Typical Use|Access layer|Distribution/Core|WAN edge, Internet gateway|

### 9. Common Vendors & Examples

|Vendor|Series|OS|Notes|
|---|---|---|---|
|**Cisco**|Catalyst 3560/3850/9300, Nexus 9K|IOS XE / NX-OS|Campus and DC cores|
|**HPE Aruba**|2930F, 5400Rzl2|ArubaOS-CX|Similar feature set|
|**Juniper**|EX-4300, QFX-5100|Junos OS|DC/aggregation|
|**Arista**|7050, 7280|EOS|Spine-leaf high-speed|

### 10. Maintenance & Management

- **CLI:** standard IOS syntax
    
- **show mac address-table**, **show ip route**, **show interfaces status**, **show platform hardware capacity**
    
- **Configuration backup:** `copy running-config startup-config`
    
- **Upgrade:** via `copy tftp flash:` + `boot system flash:<image>`
    
- **Monitor:** SNMPv3, Syslog, NetFlow exporters, NTP synchronization.

### 11. Dirty Little Secrets (Real-World Notes)

- **SVI limit:** Each SVI consumes TCAM entries — high VLAN counts can overflow TCAM.
    
- **ACL scale:** Too many complex ACLs can exhaust TCAM banks → fallback to CPU (slow!).
    
- **QoS trade-offs:** Hardware queues are finite; per-port shaping affects ASIC scheduling.
    
- **ARP storms / CAM overflows** can disable forwarding if table saturation occurs.
    
- **Stacking vs. Chassis:** StackWise uses ring fabric (~480 Gbps), while modular chassis (Sup720, Sup2T) use crossbar or fabric modules with supervisor redundancy.
    
- **Routing in hardware** ≠ free — each prefix consumes TCAM slots; prefix-aggregation and summarization save memory.
    
- **Firmware bugs in ASIC microcode** can cause silent forwarding anomalies (why Cisco releases frequent FPGA updates).

### 12. Summary

**A Layer 3 switch = switch + router in one chassis.**  
It forwards frames at Layer 2, routes packets at Layer 3 — both in hardware.  
It’s designed for **intra-campus routing, speed, and policy control**, not for WAN edge.  
Routers are still used at borders; L3 switches dominate LAN cores.

### 13. CLI Essentials (CCNA-Level)

```bash
# Enable IP routing on a Layer3 switch
Switch(config)# ip routing

# Create VLANs and assign ports
Switch(config)# vlan 10
Switch(config-vlan)# name Sales
Switch(config)# int range fa0/1 - 12
Switch(config-if-range)# switchport access vlan 10

# Create SVIs for routing
Switch(config)# int vlan 10
Switch(config-if)# ip address 192.168.10.1 255.255.255.0
Switch(config-if)# no shutdown

# Configure a static route
Switch(config)# ip route 0.0.0.0 0.0.0.0 192.168.100.1

# Enable a routing protocol
Switch(config)# router ospf 1
Switch(config-router)# network 192.168.0.0 0.0.255.255 area 0

# Verify
Switch# show ip route
Switch# show ip interface brief
Switch# show vlan brief
Switch# show interfaces status

```


---
## 🧩 OSI & Data Flow Diagram (L3 Switch Overview)

Below is a conceptual diagram showing the **layer functions, hardware separation, and data flow** inside a modern Layer 3 switch:

```bash
                          ┌─────────────────────────────┐
                          │       Management Plane      │
                          │ (CPU + IOS / NX-OS Control) │
                          │─────────────────────────────│
                          │ Handles:                    │
                          │ • CLI, SNMP, SSH, Syslog    │
                          │ • Routing Protocols (OSPF)  │
                          │ • STP, VLAN DB, AAA, NTP    │
                          └─────────────┬───────────────┘
                                        │
                                        │   Control Messages (slow path)
                                        ▼
 ┌──────────────────────────────────────────────────────────────────────────┐
 │                HARDWARE FORWARDING PLANE (ASICs + Fabric)               │
 │──────────────────────────────────────────────────────────────────────────│
 │ CAM (MAC Table) → For L2 switching decisions                            │
 │ TCAM (CEF / ACL / QoS tables) → For L3 routing and policies             │
 │ Crossbar / Fabric ASIC → Moves frames between ingress/egress ports      │
 │ QoS / ACL / Policing modules per port                                   │
 └──────────────────────────────────────────────────────────────────────────┘
                                        │
                                        │   Wire-speed forwarding
                                        ▼
                          ┌─────────────────────────────┐
                          │        Physical Ports       │
                          │  (Copper, SFP+, QSFP)       │
                          └─────────────────────────────┘

```

**Packet life inside an L3 switch:**

1. Frame enters ingress port → ASIC checks **VLAN + MAC** (L2).
    
2. If destination in another VLAN → SVI identified → **TCAM performs L3 lookup**.
    
3. Header rewritten (TTL–1, new MAC) → QoS/ACL applied → **egress ASIC** transmits.
    
4. CPU only used for exceptions (routing updates, ARP requests, management).
    

This architecture allows routing **at wire speed**, unlike classic routers.

## ⚡ TL;DR — L3 Switch in One Page

|Category|Description|
|---|---|
|**Primary Role**|Combines **L2 switching** and **L3 routing** in one chassis. Routes between VLANs internally (SVIs).|
|**OSI Layers**|Operates mainly on **Layer 2 & 3**, some Layer 4 for ACL/QoS classification.|
|**Hardware Engines**|ASICs with **CAM** (MAC tables) and **TCAM** (route, ACL, QoS lookups).|
|**Forwarding Plane**|Hardware-based, non-blocking backplane (multi-Tbps throughput).|
|**CPU/Control Plane**|Handles protocols (STP, OSPF, ARP, SNMP, SSH) and management tasks.|
|**Memory Types**|DRAM (runtime), Flash/NVRAM (IOS & config), CAM/TCAM (hardware tables).|
|**Port Types**|Access, Trunk, Uplink (SFP/QSFP), Management (OOB), Console.|
|**Routing Support**|Static, RIP, OSPF, EIGRP, VRF-Lite, sometimes BGP.|
|**Performance**|Hardware line-rate routing; low latency (< 1 µs typical).|
|**High-Availability**|HSRP/VRRP/GLBP, EtherChannel, StackWise, redundant PSUs.|
|**Security Features**|ACLs in TCAM, DHCP Snooping, DAI, Port Security, 802.1X, VRF segmentation.|
|**Management**|CLI, SNMPv3, NetFlow, Syslog, NTP, RESTCONF.|
|**Use Case**|Enterprise **distribution/core layer**, **inter-VLAN routing**, data-center leaf/spine.|
|**Not Ideal For**|WAN edge, NAT-heavy, complex service chaining (router territory).|

### 🧠 Real-World “Engineer Notes”

- TCAM space = gold — every ACL or route consumes entries; optimize with summarization.
    
- Some models require `ip routing` command to enable L3 features.
    
- Avoid VLANs spanning too many switches → STP load + TCAM exhaustion.
    
- Always upgrade **ASIC firmware** (FPGA microcode) along with IOS XE/NX-OS images.
    
- Monitor via `show platform hardware capacity` to track TCAM utilization.

