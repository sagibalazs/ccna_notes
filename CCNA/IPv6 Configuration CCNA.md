

# IPv6 Configuring CCNA Level

# **SECTION 0 — Document Title**

# **NE – CCNA PREP – 1.8 CONFIGURE AND VERIFY IPv6 ADDRESSING AND PREFIX**

This document describes IPv6 addressing, prefix planning, configuration, and verification across Cisco IOS devices following the _engineer-format_ standard.


## Cheat Sheet - CLI

```less
# =====================================================================
# IPv6 ONE-PAGE COMMAND CHEAT SHEET (PER INTERFACE / DEVICE TYPE)
# =====================================================================

# ---------------------------------------------------------------------
# GLOBAL – ENABLE IPv6 ROUTING
# ---------------------------------------------------------------------
conf t
 ipv6 unicast-routing
exit


# =====================================================================
# ROUTER – L3 ETHERNET INTERFACE (LAN)
# =====================================================================
interface g0/0
 description LAN Interface
 no shutdown

 # Global Unicast
 ipv6 address 2001:db8:10:1::1/64

 # Optional deterministic Link-Local
 ipv6 address fe80::1 link-local

 # (Optional) EUI-64 autogen
 # ipv6 address 2001:db8:10:1::/64 eui-64

 # Advertise prefix for SLAAC
 ipv6 nd prefix 2001:db8:10:1::/64

 # Optional RA flags:
 # ipv6 nd other-config-flag       # O=1 (stateless DHCPv6)
 # ipv6 nd managed-config-flag     # M=1 (stateful DHCPv6)
exit


# =====================================================================
# LAYER 3 SWITCH – SVI (VLAN INTERFACE)
# =====================================================================
interface vlan 10
 description VLAN 10 Gateway
 no shutdown

 ipv6 address 2001:db8:10:10::1/64
 ipv6 address fe80::10 link-local

 ipv6 nd prefix 2001:db8:10:10::/64
exit


# =====================================================================
# MULTILAYER SWITCH – ROUTED PORT (NO SWITCHPORT)
# =====================================================================
interface g1/0/1
 description Routed Port to Router
 no switchport
 no shutdown

 ipv6 address 2001:db8:200:1::1/64
 ipv6 address fe80::1 link-local
exit


# =====================================================================
# LOOPBACK INTERFACE
# =====================================================================
interface loopback0
 description Router-ID Loopback
 ipv6 address 2001:db8:ffff::1/128
exit


# =====================================================================
# POINT-TO-POINT IPv6 (RECOMMENDED /127)
# =====================================================================
interface serial0/0/0
 description P2P Link
 no shutdown

 ipv6 address 2001:db8:50::1/127
 ipv6 address fe80::1 link-local
exit


# =====================================================================
# DHCPv6 – STATELESS (SLAAC + DNS)
# =====================================================================
ipv6 dhcp pool STATELESS-POOL
 dns-server 2001:db8::53
 domain-name example.local

interface g0/1
 ipv6 address 2001:db8:30:1::1/64
 ipv6 nd other-config-flag         # O=1
 ipv6 dhcp server STATELESS-POOL
exit


# =====================================================================
# DHCPv6 – STATEFUL (FULL ADDRESS FROM SERVER)
# =====================================================================
ipv6 dhcp pool STATEFUL-POOL
 address prefix 2001:db8:40:1::/64
 dns-server 2001:db8::53

interface g0/2
 ipv6 address 2001:db8:40:1::1/64
 ipv6 nd managed-config-flag       # M=1
 ipv6 dhcp server STATEFUL-POOL
exit


# =====================================================================
# DHCPv6 CLIENT (ROUTER RECEIVES ADDRESS VIA DHCPv6)
# =====================================================================
interface g0/3
 ipv6 address dhcp
 ipv6 enable
exit


# =====================================================================
# PREFIX DELEGATION (DHCPv6-PD)
# =====================================================================

# Upstream WAN interface
interface g0/4
 ipv6 address dhcp
 ipv6 dhcp client pd ISP-PD
exit

# LAN interface using delegated /64 prefix
interface g0/0
 ipv6 address ISP-PD ::1/64
 ipv6 nd prefix ISP-PD ::/64
exit


# =====================================================================
# VERIFICATION COMMANDS (ALL DEVICES)
# =====================================================================
show ipv6 interface brief
show ipv6 interface g0/0
show ipv6 neighbors
show ipv6 route
show ipv6 dhcp pool
show ipv6 dhcp binding
show ipv6 dhcp client pd
ping ipv6 <address>
ping fe80::<link-local>%<interface>
# =====================================================================
```





# **SECTION 1 – MINI-WIKI: IPv6 ADDRESSING & PREFIXING (ENGINEER DENSITY)**

## **IPv6 Address**

128-bit hierarchical address used for routing in IPv6 networks. Written in hexadecimal, separated by colons. Supports vastly more networks/hosts than IPv4 and contains integrated autoconfiguration, security, and multicast enhancements.

## **Prefix**

Defines the network portion of an IPv6 address. Written in CIDR notation (e.g., `/64`).  
IPv6 routing and addressing _always_ operate on prefixes, not masks.

## **Interface Identifier (IID)**

The host portion of an IPv6 address (typically 64 bits). Generated via:

- Manual assignment
    
- EUI-64 from MAC
    
- Randomized IID (privacy extensions)
    
- DHCPv6 (stateful)
    

## **Global Unicast Address (GUA)**

Routable on the public Internet. Prefix = `2000::/3`.  
Assigned manually, by SLAAC, DHCPv6, or via ISP Prefix Delegation.

## **Unique Local Address (ULA)**

Private IPv6 address space for internal networks. Prefix = `fd00::/8`.  
Not routed on the Internet but behaves like internal GUA.

## **Link-Local Address**

Automatically created on all IPv6 interfaces. Prefix = `fe80::/10`.  
Required for IPv6 neighbor discovery, routing protocols, and SLAAC.  
Exists even without a global address.

## **Multicast Address**

One-to-many delivery used for control-plane operations. Examples:

- **ff02::1** (all nodes)
    
- **ff02::2** (all routers)
    
- **ff02::1:ffXX:XXXX** (solicited-node multicast for ND)
    

## **Anycast Address**

A unicast-like address assigned to multiple interfaces.  
Packets go to the nearest instance based on routing metrics.

## **Stateless Address Autoconfiguration (SLAAC)**

Hosts self-configure using router advertisements (RA).  
Router provides prefix; host generates interface ID.  
Flags:

- **A flag:** autonomous configuration
    
- **O flag:** other configuration (stateless DHCPv6)
    
- **M flag:** managed (stateful DHCPv6)
    

## **Stateful DHCPv6**

DHCPv6 server provides full IPv6 address + options.  
Requires **M=1** flag in RA.

## **Stateless DHCPv6**

DHCPv6 provides _only_ additional info (DNS, domain name).  
Host forms IPv6 address using SLAAC.  
Requires **O=1**, **M=0**.

## **EUI-64**

Automatic generation of the interface identifier from MAC (inserting `ff:fe` and flipping the U/L bit).  
Creates predictable host part.

## **Privacy Extension (Temporary Addresses)**

Random IID for outgoing connections to improve privacy.  
Generated in parallel with stable addresses.

## **Prefix Delegation (PD)**

ISP assigns a larger block (e.g., /56 or /48) to the customer router.  
Customer router subdivides it into /64 networks for LANs.

## **Duplicate Address Detection (DAD)**

Every IPv6 node tests for address uniqueness by sending a Neighbor Solicitation for its own address.

## **Neighbor Discovery (ND)**

Replaces ARP. Manages:  
– Router discovery  
– Prefix discovery  
– Address resolution  
– Next-hop determination  
– DAD

## **Router Advertisement (RA)**

Sent by routers. Controls host addressing behavior.  
Contains prefix info and SLAAC/DHCPv6 flags (A/M/O).

## **IPv6 Prefix Length Standards**

- **/64:** Standard for LAN segments (required for SLAAC)
    
- **/127:** Recommended for point-to-point router links
    
- **/128:** Loopbacks or host-only assignments
    
- **/48, /56:** Common ISP delegated blocks


## SECTION 2 – CCNA SUMMARY (CHEAT SHEET)

```less
# ============================================================
# CCNA IPv6 CONFIGURE & VERIFY – COMPLETE SUMMARY (CHEAT SHEET)
# ============================================================

# -------------------------
# BASIC IPv6 CONCEPTS
# -------------------------
IPv6 Address Length.............. 128 bits
Notation......................... Hexadecimal, colon-separated
Prefix Notation.................. CIDR (e.g., /64)
Standard LAN Prefix.............. /64
PtP Router Links................. /127
Loopback/Host.................... /128

# -------------------------
# ADDRESS TYPES
# -------------------------
Global Unicast (GUA)............. 2000::/3 (Internet-routable)
Unique Local (ULA)............... fd00::/8 (private networks)
Link-Local....................... fe80::/10 (always present, mandatory)
Multicast........................ ff00::/8 (one-to-many)
Solicited-Node Multicast......... ff02::1:ffXX:XXXX (ND)
Anycast.......................... Same prefix as unicast, assigned to many

# -------------------------
# ADDRESS ASSIGNMENT METHODS
# -------------------------
Manual Static IPv6............... ipv6 address 2001:db8:1::1/64
EUI-64 Autogenerated............. ipv6 address 2001:db8:1::/64 eui-64
SLAAC............................ Host forms address using RA (A=1)
Stateless DHCPv6................. SLAAC + DHCPv6 options (O=1)
Stateful DHCPv6.................. DHCPv6 assigns full address (M=1)

# -------------------------
# ROUTER ADVERTISEMENTS FLAGS
# -------------------------
A flag (Autonomous).............. 1 = host may use SLAAC
O flag (Other config)............ 1 = host should use DHCPv6 for DNS etc.
M flag (Managed config).......... 1 = host must use Stateful DHCPv6

# -------------------------
# BASIC CISCO IPv6 CONFIGURATION
# -------------------------
Enable IPv6 Routing.............. ipv6 unicast-routing
Assign IPv6 Address.............. ipv6 address <address>/<prefix>
Assign EUI-64 Address............ ipv6 address <prefix> eui-64
Manual Link-Local................ ipv6 address fe80::1 link-local

# -------------------------
# SVI CONFIGURATION (L3 SWITCH)
# -------------------------
interface vlan 10
 ipv6 address 2001:db8:10::1/64
 ipv6 address fe80::10 link-local

# -------------------------
# DHCPv6 CONFIG (CCNA LEVEL)
# -------------------------

# Stateful DHCPv6 (server assigns full address)
ipv6 dhcp pool NET1
 address prefix 2001:db8:100::/64
 dns-server 2001:db8::53

interface g0/0
 ipv6 dhcp server NET1
 ipv6 nd managed-config-flag     # M=1

# Stateless DHCPv6 (server provides DNS only)
ipv6 dhcp pool SL1
 dns-server 2001:db8::53

interface g0/0
 ipv6 dhcp server SL1
 ipv6 nd other-config-flag       # O=1

# -------------------------
# COMMON HOST BEHAVIOR
# -------------------------
SLAAC only....................... A=1, M=0, O=0
SLAAC + Stateless DHCPv6......... A=1, M=0, O=1
Stateful DHCPv6.................. M=1  (O irrelevant)

# -------------------------
# NORMAL IPv6 INTERFACE OUTPUT ITEMS
# -------------------------
show ipv6 interface
  - Link-local address
  - Global unicast address(es)
  - ND reachable time, NS/NA counters
  - RA flags received
  - Joined multicast groups

# -------------------------
# ESSENTIAL VERIFICATION COMMANDS
# -------------------------
show ipv6 interface brief
show ipv6 interface
show ipv6 route
show ipv6 neighbors
ping ipv6 <address>
ping fe80::1%g0/0        # For link-local pings
traceroute ipv6 <address>

# -------------------------
# TROUBLESHOOTING QUICK RULES
# -------------------------
Link-local missing?............ Interface is down OR IPv6 disabled.
SLAAC not working?............. A flag missing in RA (/64 required).
DHCPv6 not working?............ Check M/O flags, server binding, RA.
ND resolution failing?......... Verify solicited-node multicast reachability.
Prefix wrong?.................. Check RA prefix-list and interface config.
# ============================================================
```


# **SECTION 3 – IPv6 ADDRESS DESIGN (ENGINEER LEVEL)**

## **3.1 Addressing Principles**

IPv6 addressing follows strict hierarchical design. Planning the prefix structure correctly is the foundation of scalable enterprise networks.

### **Hierarchical Division**

- **Provider Prefix**: ISP typically delegates **/48** or **/56** to an enterprise.
    
- **Site Prefix**: Enterprise subdivides into /64 LAN segments or /127 WAN PtP links.
    
- **Interface Identifier**: Hosts generate 64-bit IIDs (manual, EUI-64, or random).
    

### **Required Prefix Boundaries**

- **/64 is mandatory for all LAN segments**  
    SLAAC and ND require 64-bit Interface IDs. Non-/64 LANs break host autoconfiguration.
    
- **/127 for point-to-point links**  
    Prevents subnet scanning, eliminates ND exhaustion attacks, reduces address waste.
    
- **/128 for loopbacks**  
    Ensures a stable and unique router ID for routing protocols and management.
    

### **IPv6 Routing Dependency**

Routers advertise prefixes, not individual addresses.  
Planning must allow:

- hierarchical summarization
    
- multi-area OSPFv3 or multi-level IS-ISv6
    
- deterministic failover paths
    
- prefix aggregation where possible
    

---

## **3.2 IPv6 Address Assignment Methods**

### **Manual Static Assignment**

Used for:

- router interfaces
    
- servers
    
- critical infrastructure requiring predictable addresses
    

Provides deterministic topology and easy troubleshooting.

### **EUI-64 (Automatic IID Generation)**

Router or host forms IID automatically:

- splits MAC address
    
- inserts `ff:fe`
    
- flips the U/L bit
    

Advantage: automatic and stable address.  
Disadvantage: reveals MAC/OUI → privacy issue.

### **Randomized IID (Privacy Extensions)**

Introduces temporary outbound addresses for clients.  
Used mainly in:

- user devices
    
- end-hosts requiring privacy
    

Not used for servers or network infrastructure.

### **Stateless Address Autoconfiguration (SLAAC)**

Host uses RA prefix + local IID.  
No central server needed.  
Best for:

- client devices
    
- IoT
    
- lightweight deployments
    

### **Stateless DHCPv6**

Provides supplementary info:

- DNS servers
    
- domain search
    
- NTP
    

Does _not_ assign addresses.  
Requires RA flag O=1.

### **Stateful DHCPv6**

Provides:

- full address
    
- DNS servers
    
- lease management
    

Used where administrative control is required.  
Requires RA flag M=1.

### **Prefix Delegation (PD)**

ISP → CPE (customer router) receives a larger prefix (e.g., /56).  
CPE subdivides into /64 LAN prefixes.  
Used home and enterprise edge deployments.

---

## **3.3 Address Allocation Best Practices**

### **Use a Dedicated “Address Plan Document”**

Content should include:

- global prefix
    
- subnet numbering rules
    
- interface addressing rules
    
- loopback scheme
    
- router numbering scheme
    
- multi-site aggregation logic
    

### **Global Unicast Allocation Strategy**

Given a delegated **/48**, you can divide as follows:

- **/56 per building or per department**
    
- **/64 per VLAN or per subnet**
    
- **/128 for loopbacks**
    

### **ULA Allocation Strategy**

Use when:

- no Internet connectivity
    
- internal-only networks
    
- hybrid deployments (GUA + ULA)
    

Recommended format:  
`fdXX:XXXX:XXXX::/48` allocated from pseudo-random 40-bit global ID.

### **Server and Infrastructure Addressing**

- Avoid EUI-64
    
- Use deterministic low-numbered interface IDs (`::10`, `::100`, etc.)
    
- Use /128 for loopbacks
    
- Use /127 for router-to-router links
    

### **Client Network Addressing**

Clients typically use:

- SLAAC only
    
- SLAAC + stateless DHCPv6
    
- Stateful DHCPv6 (Windows enterprise)
    

Choose based on operational model.

---

## **3.4 Address Scoping**

### **Link-Local (fe80::/10)**

- Always configured
    
- Required for routing protocols (OSPFv3, EIGRPv6)
    
- Required for ND and SLAAC
    
- Never routed beyond the local link
    

### **Unique Local (ULA – fd00::/8)**

Use cases:

- internal-only systems
    
- IoT networks
    
- dark networks
    
- lab networks
    

### **Global Unicast (2000::/3)**

Internet-routable, globally unique.  
Used for enterprise LAN, WAN, servers, routers, and VPN endpoints.

### **Multicast (ff00::/8)**

IPv6 replaces broadcast with multicast.  
Critical addresses:

- ff02::1 All Nodes
    
- ff02::2 All Routers
    
- ff02::1:ffXX:XXXX Solicited-Node (ND)
    

### **Anycast**

Same prefix assigned to multiple routers.  
Used for:

- DNS
    
- load balancing
    
- closest-service routing



# **SECTION 4 – IPv6 CONFIGURATION ON CISCO IOS**

## **4.1 Common Tasks – IPv6 on All IOS Devices**

Below is **one continuous code block** containing all essential IPv6 configuration operations applicable to routers, L3 switches, firewalls (ASA similar), and any IOS device with L3 interfaces.

```less
# ============================================================
# GLOBAL IPv6 ENABLEMENT
# ============================================================

conf t
 ipv6 unicast-routing                  # Required for IPv6 forwarding on routers/L3 switches
 exit


# ============================================================
# INTERFACE CONFIG – MANUAL GLOBAL ADDRESS
# ============================================================

interface g0/0
 description LAN interface
 ipv6 address 2001:db8:10:1::1/64      # Manually assign IPv6 GUA
 ipv6 enable                           # Some IOS versions need this to force IPv6 on
 no shutdown
 exit


# ============================================================
# INTERFACE CONFIG – MANUAL LINK-LOCAL ADDRESS
# ============================================================

interface g0/0
 ipv6 address fe80::1 link-local        # Override auto-generated LL address
 exit


# ============================================================
# INTERFACE CONFIG – EUI-64 ADDRESS AUTOGENERATION
# ============================================================

interface g0/1
 description SLAAC-compatible interface
 ipv6 address 2001:db8:20:1::/64 eui-64 # Router generates IID from MAC
 exit


# ============================================================
# INTERFACE CONFIG – MULTIPLE ADDRESSES (DUAL-GUA)
# ============================================================

interface g0/0
 ipv6 address 2001:db8:10:1::1/64       # Primary address
 ipv6 address 2001:db8:10:1::100/64     # Secondary address
 exit


# ============================================================
# ENABLE IPv6 ROUTING PROTOCOLS (Optional)
# ============================================================

router ospfv3 1
 router-id 1.1.1.1
 exit

interface g0/0
 ospfv3 1 ipv6 area 0
 exit


# ============================================================
# BASIC VERIFICATION COMMANDS
# ============================================================

do show ipv6 interface brief             # Interface + addresses (LL + GUA)
do show ipv6 interface g0/0              # Detailed IPv6 state
do show ipv6 neighbors                   # ND neighbor cache
do show ipv6 route                       # Routing table
do ping ipv6 2001:db8:10:1::2            # IPv6 connectivity test

# ping link-local requires outgoing interface:
do ping fe80::2%g0/0                     # ND/L2 reachability test

# ============================================================
```


## **4.2 Per Interface Type – Complete Configuration Blocks**

Each interface type has _its own_ continuous block, ready for immediate paste into IOS.

---

### **4.2.1 Router Ethernet Interface (Layer 3)**


```less
# ============================================================
# ROUTER L3 ETHERNET INTERFACE IPv6 CONFIGURATION
# ============================================================

conf t
 ipv6 unicast-routing                     # Required for forwarding
 exit

interface g0/0
 description Router LAN Interface
 no shutdown

 # IPv6 addresses
 ipv6 address fe80::1 link-local          # Optional but recommended for deterministic LL
 ipv6 address 2001:db8:10:1::1/64         # Global unicast address
 # ipv6 address 2001:db8:10:1::/64 eui-64 # Optional alternative (automatic IID)

 # Enable SLAAC for hosts (advertise prefix)
 ipv6 nd prefix 2001:db8:10:1::/64        # Advertise prefix via RA
 ipv6 nd ra lifetime 1800                 # Router lifetime

 # DHCPv6 (stateless or stateful configured later)
 # ipv6 nd other-config-flag              # Enable stateless DHCPv6
 # ipv6 nd managed-config-flag            # Enable stateful DHCPv6

exit

# Verification
do show ipv6 interface g0/0
do show ipv6 route
do show ipv6 neighbors
# ============================================================
```


## 4.2.2 Switch SVI (Layer 3 Switch VLAN Interface)

```less
# ============================================================
# L3 SWITCH SVI – IPv6 CONFIGURATION
# ============================================================

conf t
 ipv6 unicast-routing
 exit

interface vlan 10
 description User VLAN 10
 no shutdown

 ipv6 address fe80::10 link-local           # Deterministic LL preferred
 ipv6 address 2001:db8:10:10::1/64          # Gateway for VLAN 10

 # Optional EUI-64-form address
 # ipv6 address 2001:db8:10:10::/64 eui-64

 # Support SLAAC (default: RA enabled on L3 switch)
 ipv6 nd prefix 2001:db8:10:10::/64

exit

# Verification
do show ipv6 interface vlan10
do show ipv6 route
# ============================================================
```

## 4.2.3 Routed Port on Multilayer Switch

```less
# ============================================================
# ROUTED PORT ON MLS – IPv6 CONFIGURATION
# ============================================================

conf t
 ipv6 unicast-routing
 exit

interface g1/0/1
 description Routed Port to Router B
 no switchport                           # Convert to L3 port
 no shutdown

 ipv6 address fe80::1 link-local
 ipv6 address 2001:db8:200:1::1/64

exit

# Verification
do show ipv6 interface g1/0/1
do show ipv6 neighbors
# ============================================================

```


## 4.2.4 Loopback Interface

```less
# ============================================================
# LOOPBACK INTERFACE IPv6 CONFIGURATION
# ============================================================

conf t
interface loopback0
 description Router-ID Loopback
 ipv6 address 2001:db8:ffff::1/128      # /128 stable identifier
 exit

# Verification
do show ipv6 interface loopback0
# ============================================================
```


## 4.2.5 Point-to-Point Serial or WAN Interface (/127)

```less
# ============================================================
# POINT-TO-POINT IPv6 (/127 RECOMMENDED)
# ============================================================

conf t
 ipv6 unicast-routing
 exit

interface serial0/0/0
 description PtP WAN Link
 no shutdown

 ipv6 address fe80::1 link-local
 ipv6 address 2001:db8:50::1/127        # Peer uses ::0 or ::1 depending on plan

exit

# Verification
do show ipv6 interface serial0/0/0
do ping ipv6 2001:db8:50::0
do show ipv6 neighbors
# ============================================================
```



# **SECTION 5 – ROUTER ADVERTISEMENTS (RA) & NEIGHBOR DISCOVERY (ND)**

## **5.1 Overview of IPv6 Neighbor Discovery (ND)**

IPv6 replaces ARP with **ND (Neighbor Discovery)**, implemented via ICMPv6.  
ND governs all control-plane functions for IPv6 hosts and routers:

### **ND Responsibilities**

- **Router Discovery** (finding IPv6 default gateways)
    
- **Prefix Discovery** (learning IPv6 network prefixes)
    
- **Address Autoconfiguration** (SLAAC)
    
- **Address Resolution** (MAC address lookup — replaces ARP)
    
- **Next-Hop Determination**
    
- **Duplicate Address Detection (DAD)** for both link-local and global addresses
    
- **Reachability Detection** using Neighbor Unreachability Detection (NUD)
    

### **ND Message Types (ICMPv6)**

- **Router Solicitation (RS)** – Host → Router (request RA)
    
- **Router Advertisement (RA)** – Router → Host (prefixes, flags)
    
- **Neighbor Solicitation (NS)** – Query MAC (or DAD)
    
- **Neighbor Advertisement (NA)** – Response with MAC
    
- **Redirect** – Router informs host of better next-hop
    

These replace ARP, ICMP router discovery, and IPv4 redirect mechanisms.

---

## **5.2 Router Advertisement (RA) Fundamentals**

Routers periodically send **RA messages** to multicast **ff02::1** (all nodes) or on request to hosts that send RS.

### **RA Components**

The RA contains:

- Prefix(es) (network part)
    
- Prefix lifetimes
    
- Flags (A, M, O)
    
- Default router lifetime
    
- MTU
    
- On-link flag (host may treat prefix as on-link)
    

### **Prefix Information Element**

Each advertised prefix includes:

- Network prefix (e.g., 2001:db8:10:1::/64)
    
- Valid lifetime
    
- Preferred lifetime
    
- Autonomous flag (A)
    
- On-link flag (L)
    

---

## **5.3 SLAAC and RA Flags (A / M / O)**

### **A flag — Autonomous Address Configuration**

- A = 1 → Host forms its own IPv6 address using SLAAC
    
- A = 0 → Prefix cannot be used for SLAAC
    

### **M flag — Managed Configuration**

- M = 1 → Host must obtain address **via DHCPv6 (stateful)**
    
- Router does _not_ assign global addresses in this mode
    
- DHCPv6 server assigns full GUI
    

### **O flag — Other Configuration**

- O = 1 → Host may use **stateless DHCPv6** for DNS/NTP/etc.
    
- Used with SLAAC (A=1)
    

### **Flag Combinations (Host Behavior)**

|A|M|O|Result|
|---|---|---|---|
|1|0|0|SLAAC only (address + gateway from RA)|
|1|0|1|SLAAC + stateless DHCPv6|
|0|1|0|Stateful DHCPv6 (DHCP server gives full address)|
|0|1|1|Stateful DHCPv6 (O irrelevant when M=1)|

---

## **5.4 Prefix Learning: How Hosts Build Their Address**

1. Host powers on → auto-assigns **link-local fe80::/10**
    
2. Host sends **RS** (optional, speeds up RA reception)
    
3. Router responds with **RA**
    
4. Host reads prefix + flags
    
5. Host forms IID using:
    
    - EUI-64
        
    - Randomized IID
        
    - Manual assignment (rare)
        
6. Host performs **DAD** for new address
    
7. Address becomes active
    

---

## **5.5 Duplicate Address Detection (DAD)**

DAD ensures address uniqueness on the local link.

### **Process**

- Host sends NS for its **own** IPv6 address
    
- Solicited-node multicast address is used
    
- If NA received → duplicate → host drops address
    

DAD applies to:

- link-local
    
- SLAAC-generated GUAs
    
- stateless DHCPv6 addresses
    
- stateful DHCPv6 addresses
    

Routers perform DAD as well.

---

## **5.6 Neighbor Resolution (ARP Replacement)**

IPv6 does not use broadcast.  
Instead it uses **solicited-node multicast groups**.

### **Solicited-node Group Calculation**

For address:  
`2001:db8:10:1::1234:abcd`  
Take last 24 bits (abcd), append to:  
`ff02::1:ff00:0/104`  
→ e.g., `ff02::1:ff34:abcd`

### **Neighbor Solicitation (NS)**

Host sends NS to the **solicited-node multicast address** asking for MAC.

### **Neighbor Advertisement (NA)**

Target replies with:

- Target MAC
    
- Flags (router, override)
    

If no reply → entry considered stale/unreachable.

---

## **5.7 Router Lifetime & Default Gateway Behavior**

The RA includes a **router lifetime** (default gateway validity timer).  
If timer expires:

- host removes router from default gateway list
    
- host can select another router (if present)
    
- host may send RS to solicit fresh RAs
    

---

## **5.8 ND Cache Behavior (NUD – Neighbor Unreachability Detection)**

NUD continuously tests neighbor reachability.

### **States**

- **INCOMPLETE** → No MAC learned
    
- **REACHABLE** → Valid, recently confirmed
    
- **STALE** → Needs confirmation
    
- **DELAY** → Grace period before probing
    
- **PROBE** → Resending NS
    
- **FAILED** → No response; remove neighbor
    

This mechanism permits faster convergence than ARP.

---

## **5.9 ND and Security Considerations (CCNA Basics)**

### **Key risks**

- Rogue RAs
    
- Neighbor spoofing
    
- Router impersonation
    

### **Basic countermeasures (IOS)**

- RA Guard (switch-level)
    
- IPv6 ACLs
    
- DHCPv6 Guard
    
- SeND (not widely deployed; exam-level only)


# **SECTION 6 – DHCPv6 CONFIGURATION (STATELESS, STATEFUL, PREFIX DELEGATION)**

## **6.1 DHCPv6 Overview**

### **Stateless DHCPv6**

- Host uses **SLAAC** to create its IPv6 address.
    
- DHCPv6 server provides **additional info only** (DNS, domain name, NTP).
    
- RA Flags: **A=1**, **M=0**, **O=1**.
    

### **Stateful DHCPv6**

- DHCPv6 server assigns **full IPv6 address** + DNS + other options.
    
- Works similarly to DHCPv4 but with no broadcast.
    
- RA Flags: **M=1** (O optional).
    

### **Prefix Delegation (PD)**

- Router (CPE) receives a large prefix from ISP (e.g., /56).
    
- Router delegates /64 sub-prefixes to downstream LAN interfaces.
    
- Used in real-world ISP deployments (home + business).
    

---

# **SECTION 6 – FULL DHCPv6 CONFIGURATION (ALL METHODS IN ONE CODE BLOCK)**

```less
# =====================================================================
# DHCPv6 – COMPLETE CCNA CONFIGURATION EXAMPLES IN ONE CODE BLOCK
# =====================================================================

# ---------------------------------------------------------------------
# GLOBAL PREPARATION
# ---------------------------------------------------------------------
conf t
 ipv6 unicast-routing                          # Required for IPv6 routing
exit


# =====================================================================
# PART 1 – STATELESS DHCPv6 SERVER (Hosts use SLAAC for addresses)
# =====================================================================
# Use Case:
# - Hosts generate their own IPv6 address (A=1)
# - DHCPv6 provides extra info like DNS, domain
# - RA Flags: O=1, M=0

conf t

# Create DHCPv6 pool
ipv6 dhcp pool STATELESS-POOL
 dns-server 2001:db8::53                       # DNS server
 domain-name example.local                     # Local domain

# Bind DHCPv6 to interface (router interface to clients)
interface g0/0
 description LAN for Stateless DHCPv6
 ipv6 address 2001:db8:10:1::1/64
 ipv6 address fe80::1 link-local

 ipv6 nd other-config-flag                     # O=1 → SLAAC + stateless DHCPv6
 # Do NOT enable M=1 here!

 ipv6 dhcp server STATELESS-POOL               # Provide optional info
 no shutdown
exit


# =====================================================================
# PART 2 – STATEFUL DHCPv6 SERVER (DHCPv6 assigns full address)
# =====================================================================
# Use Case:
# - Hosts do NOT autogenerate IPv6 address
# - DHCPv6 server assigns full IPv6 + DNS
# - RA Flags: M=1

conf t

# Create DHCPv6 stateful pool
ipv6 dhcp pool STATEFUL-POOL
 address prefix 2001:db8:20:1::/64             # DHCPv6 WILL assign these addresses
 dns-server 2001:db8::53

interface g0/1
 description LAN for Stateful DHCPv6
 ipv6 address 2001:db8:20:1::1/64
 ipv6 address fe80::1 link-local

 ipv6 nd managed-config-flag                   # M=1 → Use stateful DHCPv6
 # SLAAC disabled for address assignment when M=1

 ipv6 dhcp server STATEFUL-POOL
 no shutdown
exit


# =====================================================================
# PART 3 – DHCPv6 CLIENT (Router acting as client)
# =====================================================================
# Used if this device receives IPv6 address config from upstream.

conf t
interface g0/0
 ipv6 address dhcp                             # Request IPv6 address via DHCPv6
 ipv6 enable
 no shutdown
exit


# =====================================================================
# PART 4 – PREFIX DELEGATION (PD)
# =====================================================================
# Use Case:
# - ISP delegates a prefix (e.g., /56) to our router
# - Our router splits that prefix into /64 segments for LANs
# - Essential for IPv6 home/business ISP deployments

# ------------------------------
# UPSTREAM INTERFACE (DHCPv6-PD CLIENT)
# ------------------------------
conf t
interface g0/2
 description WAN toward ISP (PD client)
 ipv6 address dhcp                             # Request IPv6 address for WAN
 ipv6 dhcp client pd ISP-DELEGATION            # Ask ISP for delegated prefix
 no shutdown
exit


# ------------------------------
# LAN INTERFACES USING DELEGATED PREFIXES
# ------------------------------
# The router autogenerates LAN prefixes from the PD block.

conf t
interface g0/0
 description LAN 1 Using Delegated Prefix
 ipv6 address ISP-DELEGATION ::1/64            # Assign ::1 from PD-derived /64
 ipv6 nd prefix ISP-DELEGATION ::/64           # Advertise delegated prefix to clients
 no shutdown

interface g0/1
 description LAN 2 Using Delegated Prefix
 ipv6 address ISP-DELEGATION 0:0:1::1/64       # Use next /64 chunk from delegation
 ipv6 nd prefix ISP-DELEGATION 0:0:1::/64
 no shutdown
exit


# =====================================================================
# PART 5 – VERIFICATION COMMANDS
# =====================================================================

# Check pools
do show ipv6 dhcp pool

# Check bindings (stateful)
do show ipv6 dhcp binding

# Check interface DHCPv6 state
do show ipv6 interface g0/0
do show ipv6 interface g0/1

# Check PD received prefix
do show ipv6 dhcp client pd

# Connectivity
do ping ipv6 2001:db8:10:1::20                 # Ping host address
do ping fe80::aabb:ccff:fedd:1%g0/0            # Link-local ping with interface
# =====================================================================
```


# **SECTION 7 – VERIFICATION & TROUBLESHOOTING**

## **7.1 IPv6 Address Assignment Verification**

### **Step 1: Confirm Interface IPv6 Status**

Use:

- `show ipv6 interface brief`
    
- `show ipv6 interface <int>`
    

Check:

- Link-local address present
    
- Global unicast address correct
    
- Prefix matches design (/64, /127, /128)
    
- Interface is "up/up"
    
- Correct join of multicast groups (ff02::1, ff02::2, solicited-node)
    

### **Symptoms**

|Symptom|Likely Cause|
|---|---|
|No link-local address|IPv6 disabled, interface down, misconfigured|
|Wrong GUA prefix|RA misconfigured or wrong manual config|
|Duplicate address marked "tentative"|DAD failure|

### **Fix**

- Re-enable IPv6
    
- Correct RA prefix
    
- Change address / IID
    

---

## **7.2 SLAAC Verification**

### **Checklist**

- Interface must send RA with A=1
    
- LAN prefix must be **/64**
    
- Host must create IID
    
- Host receives default router from RA
    

### **Commands**

- `show ipv6 interface <int>`
    
- Look for: **"Autonomous address configuration"**
    

### **Common Failures**

|Issue|Explanation|Fix|
|---|---|---|
|Host has LL only|No RA received|Enable RA via IPv6 L3 config|
|No GUA, only LL|Prefix NOT /64|Correct prefix on router|
|SLAAC + DNS missing|O flag not set|`ipv6 nd other-config-flag`|
|SLAAC not used|A flag missing|`ipv6 nd prefix ...` correctly inserted|

---

## **7.3 Stateful DHCPv6 Verification**

### **Checklist**

- Router interface has M=1
    
- DHCPv6 pool exists
    
- DHCPv6 server bound to interface
    
- Host sends DHCPv6 Solicit → gets Advertise/Reply
    

### **Verification Commands**

- `show ipv6 dhcp pool`
    
- `show ipv6 dhcp binding`
    
- `show ipv6 interface` → look for "Stateful address configuration"
    

### **Common Failures**

|Issue|Explanation|Fix|
|---|---|---|
|Host not getting address|M flag missing|`ipv6 nd managed-config-flag`|
|DNS missing|O flag may be required|Add DNS to pool|
|No DHCPv6 binding|Wrong pool prefix|`address prefix …` must match LAN prefix|

---

## **7.4 Stateless DHCPv6 Verification**

### **Checklist**

- A=1, M=0, O=1
    
- SLAAC forms address
    
- DHCPv6 provides DNS
    
- Pool must **not** include `address prefix`
    

### **Commands**

- `show ipv6 dhcp pool`
    
- `show ipv6 interface` → look for "Other configuration flag"
    

### **Common Failures**

|Issue|Explanation|Fix|
|---|---|---|
|Host gets no DNS|O flag missing|`ipv6 nd other-config-flag`|
|Host gets full address unexpectedly|M=1 accidentally enabled|Remove M flag|

---

## **7.5 Prefix Delegation (PD) Verification**

### **Checklist**

- ISP DHCPv6 server must support PD
    
- WAN interface uses `ipv6 dhcp client pd NAME`
    
- LAN interfaces reference delegated prefix
    

### **Commands**

- `show ipv6 dhcp client pd`
    
- `show ipv6 interface`
    

### **Common Failures**

|Issue|Explanation|Fix|
|---|---|---|
|No delegated prefix|ISP-side PD not enabled|Correct ISP or lab config|
|LAN has no prefix|Router does not assign PD block|Assign with `NAME ::1/64`|

---

## **7.6 ND & Neighbor Table Verification**

### **Commands**

- `show ipv6 neighbors`
    
- `ping fe80::1%g0/0` (validate L2/ND reachability)
    

### **Observe States**

- REACHABLE
    
- STALE
    
- INCOMPLETE
    
- DELAY
    
- PROBE
    

### **Common Failures**

|Issue|Explanation|Fix|
|---|---|---|
|INCOMPLETE|No NS/NA exchange → L2 failure|Check cabling, VLANs, ND filters|
|Stuck in STALE|No traffic retesting neighbor|Ping to refresh NUD|
|ND drops|Security features filtering|Check RA-Guard, ACLs|

---

## **7.7 RA Troubleshooting**

### **Commands**

- `debug ipv6 nd`
    
- `show ipv6 interface`
    

### **Checklist**

- Router sends RA periodically
    
- RA includes correct prefix
    
- Correct flags (A/M/O)
    
- Proper router lifetime
    

### **Common Failures**

|Issue|Explanation|Fix|
|---|---|---|
|No RA on LAN|SVI down or L3 disabled|Activate VLAN, enable routing|
|Wrong prefix advertised|Misconfigured `ipv6 nd prefix`|Correct prefix advertisement|
|A flag off|Host cannot SLAAC|Re-enable A flag: `ipv6 nd prefix ...`|

---

## **7.8 Routing Verification**

### **Commands**

- `show ipv6 route`
    
- `show ipv6 protocols`
    
- `traceroute ipv6 …`
    

### **Interpretation**

|Issue|Explanation|
|---|---|
|No default route|Missing RA or static/default route|
|Missing LAN routes|Prefix not installed in routing table|
|No adjacency|ND failure, VLAN mismatch|

---

## **7.9 Connectivity Testing Deep-Dive**

### **ICMPv6 Ping Tests**

1. Ping link-local (with outgoing interface):  
    `ping fe80::1%g0/0`
    
2. Ping gateway:  
    `ping ipv6 2001:db8:10:1::1`
    
3. Ping between VLANs
    
4. Ping remote networks (tests routing)
    

### **Traceroute**

- Identifies incorrect routing paths
    
- Shows which router fails to forward IPv6
    

---

## **7.10 Quick Troubleshooting Matrix**

| Symptom                          | Likely Cause                            | Fix                                      |
| -------------------------------- | --------------------------------------- | ---------------------------------------- |
| Only fe80::                      | No RA, wrong prefix, VLAN down          | Fix RA, enable interface                 |
| Host gets address but no gateway | RA lifetime 0 or router not advertising | Fix RA config                            |
| DHCPv6 not responding            | M/O flags wrong, pool missing           | Correct RA flags and DHCP pool           |
| ND incomplete                    | Layer 2 issue                           | Fix VLAN, trunk, ACL blocking            |
| No route                         | Router not learning prefix              | Fix interface config or routing protocol |
| Duplicate address detected       | IID conflict                            | Change IPv6 address or enable random IID |


# **SECTION 8 – TL;DR TABLES (QUICK REFERENCE)**

## **8.1 SLAAC vs Stateless DHCPv6 vs Stateful DHCPv6**

|Mode|A Flag|M Flag|O Flag|Address Assigned By|DNS Provided By|Typical Use Case|
|---|---|---|---|---|---|---|
|**SLAAC Only**|1|0|0|Host (RA prefix)|None (unless manual)|Lightweight networks, IoT, home|
|**SLAAC + Stateless DHCPv6**|1|0|1|Host (SLAAC)|DHCPv6|Enterprise clients needing DNS|
|**Stateful DHCPv6**|0/1|1|0/1|DHCPv6 server|DHCPv6|Managed enterprise networks|
|**No SLAAC / No DHCPv6**|0|0|0|Manual|Manual|Infrastructure, servers, routers|

---

## **8.2 IPv6 Prefix Length Rules**

|Prefix|Meaning|Recommended Use|
|---|---|---|
|**/128**|Single address|Loopback, mgmt addresses|
|**/127**|2 usable addresses|Point-to-point router links|
|**/64**|Standard LAN subnet|REQUIRED for SLAAC and ND|
|**/56**|Delegation size from ISP|Home/business PD block|
|**/48**|Large enterprise allocation|Site-level addressing|
|**/32**|ISP allocation|Internet providers|

**Key Rule:**  
LANs **must** be /64 if SLAAC or standard ND behavior is expected.

---

## **8.3 IPv6 Address Types (Matrix View)**

|Type|Prefix|Scope|Auto-Created|Routed?|Usage|
|---|---|---|---|---|---|
|**Link-Local**|fe80::/10|Local link|Yes|No|ND, RA, routing protocols|
|**Unique Local (ULA)**|fd00::/8|Local site|No|Yes (internally)|Internal-only networks|
|**Global Unicast (GUA)**|2000::/3|Global|No|Yes|Internet + enterprise routing|
|**Multicast**|ff00::/8|Link or global|Yes (joins)|Yes|ND, routing protocols, services|
|**Anycast**|Same as GUA|Global|No|Yes|Load balancing, DNS, closest router|

---

## **8.4 ND / RA Behavior Quick Matrix**

|Function|Message|Trigger|Destination|Notes|
|---|---|---|---|---|
|Router Discovery|RS / RA|Host boot, periodic|ff02::1 / ff02::2|Host finds default gateway|
|Prefix Discovery|RA|Router broadcasts|ff02::1|Essential for SLAAC|
|Address Resolution|NS → NA|L2 resolution|Solicited-node MC|Replaces ARP|
|DAD|NS → silent|Host config|Solicited-node MC|Checks duplicate addresses|
|Redirect|Redirect msg|Router decision|Host|Points host to better next-hop|

---

## **8.5 Verification Commands Cheat Sheet**

|Task|Command|Expected Output|
|---|---|---|
|Interface summary|`show ipv6 interface brief`|LL + GUA, status up/up|
|Full interface info|`show ipv6 interface <int>`|RA flags, prefixes, ND info|
|Neighbor table|`show ipv6 neighbors`|REACHABLE, STALE, INCOMPLETE|
|Routing table|`show ipv6 route`|Connected, local, static, dynamic routes|
|DHCPv6 pools|`show ipv6 dhcp pool`|Prefix, DNS info|
|DHCPv6 bindings|`show ipv6 dhcp binding`|Client addresses|
|Prefix Delegation|`show ipv6 dhcp client pd`|Delegated ISP prefix|
|ND debug|`debug ipv6 nd`|Live ND/RA events|
|Connectivity test|`ping ipv6 <addr>`|ICMPv6 success|
|Link-local test|`ping fe80::X%interface`|ND + L2 verified|

---

## **8.6 Troubleshooting Cheatsheet (One-Glance Fix Table)**

|Symptom|Root Cause|Fix|
|---|---|---|
|Only LL address|No RA or misconfigured prefix|Enable RA, ensure /64|
|Host gets wrong prefix|Wrong RA or manual config|Fix `ipv6 nd prefix`|
|SLAAC not working|A flag disabled|Enable autonomous flag|
|Stateless DHCPv6 no DNS|O flag off|Set `ipv6 nd other-config-flag`|
|No DHCPv6 address|M flag not set or bad pool|Set `ipv6 nd managed-config-flag`|
|ND incomplete|L2/VLAN mismatch|Fix VLAN, trunk, ACL|
|Default gateway missing|RA router lifetime = 0|Fix RA parameters|
|Prefix Delegation missing|ISP not delegating|Fix upstream DHCPv6-PD|
|Duplicate address|IID collision|Regenerate IID or assign manual|


##  Mini Wiki

```less
# ============================================================
# MINI-WIKI — IPv6 ADDRESSING & PREFIXING (ENGINEER DENSITY)
# ============================================================

IPv6 Address:
  128-bit hierarchical address written in hexadecimal. Eliminates IPv4 exhaustion and supports
  autoconfiguration, mandatory link-local addressing, integrated multicast, and simplified routing.

Prefix:
  Defines the network portion of an IPv6 address, written in CIDR (e.g., /64). Routing and addressing
  operate strictly on prefixes, not masks.

Interface Identifier (IID):
  The host portion of an IPv6 address (typically 64 bits). Generated via manual assignment, EUI-64
  (MAC-based), randomized IIDs (privacy), or DHCPv6 (stateful).

Global Unicast Address (GUA):
  Routable, globally unique address space. Prefix: 2000::/3. Assigned via SLAAC, DHCPv6, or manual.

Unique Local Address (ULA):
  Private, non-routable-to-Internet IPv6 space. Prefix: fd00::/8. Used for internal networks only.

Link-Local Address (LLA):
  Automatically created, required for ND, RA, routing protocols. Prefix: fe80::/10. Never routed.

Multicast Address:
  One-to-many delivery. Prefix ff00::/8. Used for essential IPv6 control-plane functions.
    - ff02::1  All Nodes
    - ff02::2  All Routers
    - ff02::1:ffXX:XXXX  Solicited-node multicast (ND/ARP replacement)

Anycast Address:
  A unicast prefix assigned to multiple interfaces. Packets go to the nearest instance (routing metric).

SLAAC (Stateless Address Autoconfiguration):
  Host self-configures from RA prefix (A=1). No server needed. Forms address using IID (EUI-64 or
  random). Requires /64 networks.

Stateless DHCPv6:
  DHCPv6 supplies only additional info (DNS, domain). Address is still formed via SLAAC.
  Requires O=1 in RA.

Stateful DHCPv6:
  DHCPv6 server provides full IPv6 address + DNS/NTP/etc.
  Requires M=1 in RA.

Router Advertisement (RA):
  ICMPv6 message sent by routers. Communicates prefix, default gateway, flags (A/M/O), MTU, and
  lifetimes. Drives host addressing behavior.

RA Flags:
  A = Autonomous (SLAAC enabled)
  M = Managed (Stateful DHCPv6 required)
  O = Other (Stateless DHCPv6 available)

Neighbor Discovery (ND):
  Replaces ARP. Handles router discovery (RS/RA), address resolution (NS/NA), duplicate address
  detection, reachable-state maintenance (NUD), and prefix discovery.

Duplicate Address Detection (DAD):
  Host checks uniqueness by sending NS for its own address. If NA returns → conflict.

Solicited-Node Multicast:
  Automatically generated multicast used for address resolution instead of broadcast. Format:
  ff02::1:ffXX:XXXX where XX:XXXX = last 24 bits of the target IPv6 address.

Prefix Delegation (PD):
  ISP delegates a larger prefix (e.g., /56) to customer router via DHCPv6-PD. Router subdivides into
  /64 LAN prefixes. Foundation for IPv6 home/enterprise deployments.

Privacy Extensions:
  Host generates temporary randomized IIDs for outbound connections to improve privacy. Coexists
  with stable addresses.

IPv6 Prefix Length Standards:
  /64  — Required for all LANs using SLAAC and ND.
  /127 — Point-to-point router links.
  /128 — Loopbacks, mgmt, unique identifiers.
  /48–/56 — ISP delegated blocks for enterprises/home networks.

# ============================================================

```
