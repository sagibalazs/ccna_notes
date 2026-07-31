
# IPv6 Addressing v2. 

## IPv6 ONE-PAGE DIAGRAM SHEET (Pure Visual Cheatsheet)

```less
──────────────────────────────────────────────────────────────────────────────
IPv6 ONE-PAGE DIAGRAM SHEET — PURE VISUAL REFERENCE
──────────────────────────────────────────────────────────────────────────────

1) IPv6 ADDRESS STRUCTURE (128 BITS)
+---------------------------+--------------+------------------------------+
|   Global Routing Prefix   |  Subnet ID   |        Interface ID          |
|         (n bits)          |   (16 bits)  |          (64 bits)           |
+---------------------------+--------------+------------------------------+

Example (/48):
2001:0db8:abcd : 0012 : 3456:789a:bcde:f012
|------ /48 ----|--16--|----------- 64 bits -------------------------------|

Compressed Notation:
Full: 2001:0db8:0000:0000:abcd:0000:0000:0001
Short: 2001:db8::abcd:0:0:1
Final: 2001:db8::abcd:0:0:1

──────────────────────────────────────────────────────────────────────────────
2) IPV6 SPECIAL PREFIXES
GUA (Global):       2000::/3
ULA (Private):      fc00::/7
Link-Local:         fe80::/10
Multicast:          ff00::/8
Documentation:      2001:db8::/32
NAT64 Prefix:       64:ff9b::/96
IPv4-Mapped:        ::ffff:0:0/96
Loopback:           ::1/128
Unspecified:        ::/128

──────────────────────────────────────────────────────────────────────────────
3) SOLICITED-NODE MULTICAST (NDP)
ff02::1:ffXX:XXXX
Take last 24 bits of interface's unicast address.

Example:
Unicast: xxxx:xxxx:xxxx:xxxx:aaaa:bbbb:cccc:DDDD
→ ff02::1:ffcc:DDDD

──────────────────────────────────────────────────────────────────────────────
4) NDP MESSAGE FLOW (L2 RESOLUTION + ROUTER DISCOVERY)

              HOST                                        ROUTER
                │                                            │
                │--- RS → ff02::2 -------------------------->│
                │<-- RA (prefix, flags, MTU, gateway) -------│
                │
Resolve MAC:
                │--- NS → ff02::1:ffXX:XXXX ---------------->│
                │<-- NA (MAC of target) ---------------------│

DAD:
Host sends NS for its *own* address → if no NA returns → OK.

──────────────────────────────────────────────────────────────────────────────
5) SLAAC AUTOCONFIGURATION FLOW
[Interface Up]
      ↓
Generate Link-Local (fe80::/10)
      ↓
DAD for link-local
      ↓
Send RS → ff02::2
      ↓
Receive RA → (prefix /64, flags A/M/O)
      ↓
If A=1 → create GUA:
   prefix::(EUI-64 or RFC7217 Stable-ID)
      ↓
DAD for GUA
      ↓
Join Solicited-Node Multicast
      ↓
IPv6 Operational

Flags:
A = SLAAC enabled
M = DHCPv6 stateful
O = DHCPv6 stateless (DNS/options)

──────────────────────────────────────────────────────────────────────────────
6) IPV6 STATIC ROUTE LOGIC
Default Route:
::/0 → via fe80::1 (interface Gig0/0)

Static Route:
ipv6 route <prefix> <interface> <link-local-next-hop>

Lookup Flow:
1) Longest Prefix Match
2) Next-hop must be link-local
3) NDP resolve → NA returns MAC
4) Frame forwarded to gateway MAC

──────────────────────────────────────────────────────────────────────────────
7) IPV6 HEADER (40 BYTES FIXED)
+--------+-------------------------------+
| Bits   | Field                         |
+--------+-------------------------------+
| 0–3    | Version (6)                   |
| 4–11   | Traffic Class                 |
|12–31   | Flow Label                    |
|32–47   | Payload Length                |
|48–55   | Next Header                   |
|56–63   | Hop Limit                     |
|64–191  | Source Address                |
|192–319 | Destination Address           |
+--------+-------------------------------+

──────────────────────────────────────────────────────────────────────────────
8) IPV6 EXTENSION HEADER CHAIN ORDER
IPv6 Header
   ↓
Hop-by-Hop Options
   ↓
Destination Options (before Routing)
   ↓
Routing Header
   ↓
Fragment Header
   ↓
Authentication Header (AH)
   ↓
ESP (IPsec Encryption)
   ↓
Destination Options (final)
   ↓
Upper Layer Protocol (TCP/UDP/ICMPv6)

Example Entire Chain:
IPv6 → Hop-by-Hop → Routing → Fragment → Dest → TCP → Payload

──────────────────────────────────────────────────────────────────────────────
9) COMMON MULTICAST GROUPS
ff02::1      = All Nodes
ff02::2      = All Routers
ff02::5      = OSPFv3 All Routers
ff02::6      = OSPFv3 DR/BDR
ff02::9      = RIPng
ff02::A      = EIGRPv6

──────────────────────────────────────────────────────────────────────────────
10) IPV6 ROUTING PROTOCOLS (CCNA SCOPE)
OSPFv3 = uses ff02::5 / ff02::6
EIGRPv6 = uses ff02::A
RIPng  = uses ff02::9
Static routing = requires link-local next-hop

──────────────────────────────────────────────────────────────────────────────
11) ADDRESS GENERATION METHODS
EUI-64:
MAC → insert FF:FE → flip 7th bit.

Stable Privacy (RFC7217):
Hash-based ID, avoids MAC leaking.

Temporary Address:
Random & rotating → outbound privacy.

──────────────────────────────────────────────────────────────────────────────
END OF IPv6 ONE-PAGE DIAGRAM SHEET
──────────────────────────────────────────────────────────────────────────────
```


## **IPv6 Diagram Collection (Engineering Format)**


```less
──────────────────────────────────────────────────────────────
1. IPv6 Address & Prefix Layout
──────────────────────────────────────────────────────────────
128-bit IPv6 Unicast Address:

+---------------------------+-------------+---------------------+
|  Global Routing Prefix    |  Subnet ID  |    Interface ID     |
|        (n bits)           |   (16 bits) |      (64 bits)      |
+---------------------------+-------------+---------------------+

Example (/48):
2001:0db8:abcd : 0012 : 3456:789a:bcde:f012
|------ /48 ----|--16--|----------- 64 bits --------------|

Solicited-Node Multicast Address (NDP):
ff02::1:ffXX:XXXX
(Last 24 bits of unicast address appended)

──────────────────────────────────────────────────────────────
2. NDP Flow (RS/RA + NS/NA)
──────────────────────────────────────────────────────────────
            Host                                  Router
             │                                       │
             │--- RS → ff02::2 -------------------->│
             │  (Router Solicitation)                │
             │                                       │
             │<-- RA --- ff02::1 -------------------│
             │  (Prefix, flags, MTU, DNS, gateway)   │
             │                                       │
Addr resolution:
             │--- NS → ff02::1:ffXX:XXXX ---------->│
             │  (Who has this IPv6? Send MAC.)       │
             │                                       │
             │<-- NA  ------------------------------│
             │  (Here is MAC address)                │
             │                                       │

DAD:
Host sends NS for its own address.
If no NA returns → address is unique.

──────────────────────────────────────────────────────────────
3. SLAAC Workflow
──────────────────────────────────────────────────────────────
[Interface Up]
      ↓
Generate Link-Local (fe80::/10)
      ↓
DAD for link-local
      ↓
Send RS → ff02::2
      ↓
Receive RA (prefix /64 + flags)
      ↓
If A-flag=1:
   Construct GUA:
   prefix:: + Interface-ID (EUI-64 or RFC7217)
      ↓
DAD for global address
      ↓
Join solicited-node multicast
      ↓
IPv6 Ready

Flags Meaning:
- A = Autonomous (SLAAC)
- M = Managed (DHCPv6 stateful)
- O = Other config (DHCPv6 stateless)

──────────────────────────────────────────────────────────────
4. IPv6 Static Route Logic
──────────────────────────────────────────────────────────────
Default Route:
::/0  →  via fe80::1  dev Gig0/0

Static Route:
ipv6 route <prefix> <interface> <link-local-next-hop>

Resolution Logic:
1. Router uses outgoing interface.
2. Next-hop must be link-local.
3. NDP resolves next-hop MAC.
4. Longest prefix match determines final route.

Example:
2001:db8:10::/64 via fe80::1 Gig0/0
        ↓
Check NDP table for fe80::1
        ↓
Send to router’s MAC on Gig0/0
        ↓
Forward to destination subnet

──────────────────────────────────────────────────────────────
5. IPv6 Packet Header (40-byte fixed)
──────────────────────────────────────────────────────────────
+--------+---------------+-------------------------------------+
| Bits   | Field         | Description                         |
+--------+---------------+-------------------------------------+
| 0–3    | Version       | Always 6                             |
| 4–11   | TrafficClass  | QoS / ECN                            |
|12–31   | FlowLabel     | Flow identification                  |
|32–47   | PayloadLength | Size of payload                      |
|48–55   | NextHeader    | Next header or L4 protocol           |
|56–63   | HopLimit      | TTL equivalent                       |
|64–191  | SourceAddress | 128-bit source                       |
|192–319 | DestAddress   | 128-bit destination                  |
+--------+---------------+-------------------------------------+

──────────────────────────────────────────────────────────────
6. IPv6 Extension Header Chain Order
──────────────────────────────────────────────────────────────
IPv6 Header
   ↓
Hop-by-Hop Options Header
   ↓
Destination Options (before Routing header)
   ↓
Routing Header
   ↓
Fragment Header
   ↓
Authentication Header (AH)
   ↓
ESP (Encapsulating Security Payload)
   ↓
Destination Options (final)
   ↓
Upper Layer (TCP/UDP/ICMPv6/etc.)

Example Chain:
IPv6 → Hop-by-Hop → Routing → Fragment → Dest → TCP → Payload

──────────────────────────────────────────────────────────────
END OF DIAGRAM COLLECTION
──────────────────────────────────────────────────────────────

```


# **2. CCNA IPv6 Summary (Cheat Sheet, Based ONLY on Exam Blueprint)**

This summary extracts ONLY what CCNA v1.1 actually requires about IPv6, in the shortest possible engineer-accurate form.  
Every line is aligned with the CCNA exam outline:

- **1.8 Configure/verify IPv6 addressing & prefix**
    
- **1.9 Describe IPv6 address types**
    
- **3.3 Configure/verify IPv6 static routing**
    
- **Modified EUI-64**
    
- **Client OS verification**
    

All phrasing is neutral and exam-focused.  
Source: **Cisco Official CCNA v1.1 Exam Blueprint**

200-301-CCNA-v1.1

Here is the cheat sheet (copy-ready):

```less
# CCNA IPv6 Cheat Sheet

## IPv6 Address Basics
- IPv6 is a 128-bit address written in hexadecimal.
- Consists of 8 groups of 16 bits (hextets).
- Leading zeros in each hextet may be omitted.
- One sequence of consecutive zeros may be replaced with "::".
- Prefix notation uses CIDR (e.g., /64, /48).

## Mandatory IPv6 Address Types (CCNA Scope)
1. **Global Unicast (GUA) – 2000::/3**
   - Public, Internet-routable.
2. **Unique Local (ULA) – fc00::/7**
   - Private, non-Internet routed.
3. **Link-Local (LLA) – fe80::/10**
   - Mandatory on every interface.
   - Used for NDP, RS/RA, local communication, static route next-hop.
4. **Multicast – ff00::/8**
   - Replaces broadcast.
   - Examples:
     - ff02::1 (all nodes)
     - ff02::2 (all routers)
     - ff02::1:ffXX:XXXX (solicited-node multicast)
5. **Anycast**
   - Same address on multiple devices; routing chooses nearest one.

## Modified EUI-64 (Exam Topic 1.9.d)
- Used for SLAAC interface ID generation.
- Takes 48-bit MAC → inserts "ff:fe" → flips the 7th bit.
- Example:
  MAC: 00:1A:2B:3C:4D:5E  
  EUI-64 ID: 021A:2BFF:FE3C:4D5E

## SLAAC (Stateless Autoconfiguration)
- Router Advertisements (RA) provide:
  - Prefix (/64)
  - A-flag: SLAAC
  - M/O flags: DHCPv6 usage
- Host generates GUA from prefix + EUI-64/stable-ID.
- Host always performs DAD before using address.

## DHCPv6 (Exam-Level Understanding)
- **Stateless**: DNS/settings only, address from SLAAC.
- **Stateful**: DHCPv6 assigns full address.
- Default gateway is **never learned from DHCPv6**, only from RA.

## IPv6 Address Verification (Client OS)
- Windows: ipconfig /all
- Linux: ip -6 addr show, ip -6 neigh, ip -6 route
- MacOS: ifconfig, networksetup -getinfo

## IPv6 Static Routing (Exam Objective 3.3)
- IPv6 static route format:
  ipv6 route <prefix> <interface> <link-local-next-hop>
- Default route:
  ::/0 via fe80::1  (example)
- Host route:
  /128
- Network route:
  e.g., 2001:db8:1:1::/64
- Floating static route: higher administrative distance.

## IPv6 Prefix Lengths (Exam-Level)
- /128 = single host
- /64 = required LAN prefix (SLAAC requirement)
- /48, /56 = typical organization allocations

## NDP (Neighbor Discovery) Functional Summary
- Replaces ARP.
- Uses ICMPv6:
  - RS (Router Solicitation)
  - RA (Router Advertisement)
  - NS (Neighbor Solicitation)
  - NA (Neighbor Advertisement)
- Used for:
  - Address resolution
  - Prefix discovery
  - Default gateway discovery
  - Duplicate Address Detection (DAD)

## Common IPv6 Commands (Cisco IOS)
- show ipv6 interface brief
- show ipv6 interface <int>
- show ipv6 route
- show ipv6 neighbors
- ipv6 address <address>/<prefix>
- ipv6 address autoconfig
- ipv6 route ::/0 <interface> <fe80:: next-hop>

## IPv6 Multicast (CCNA Scope)
- Used for:
  - NDP
  - Routing protocols
  - Neighbor resolution
- No broadcast in IPv6.

## Key Differences Between IPv4 & IPv6 (Exam-Level)
- IPv6 uses hexadecimal, IPv4 decimal.
- IPv6 has no broadcast.
- IPv6 uses NDP instead of ARP.
- IPv6 uses ICMPv6 extensively.
- IPv6 supports SLAAC.
- IPv6 header simplified (fixed 40 bytes).
- NAT generally not required for addressing.

## Minimum You Must Memorize for CCNA IPv6
- Address types (GUA, ULA, Link-local, Multicast, Anycast).
- Modified EUI-64 process.
- SLAAC vs DHCPv6.
- Static routing, including link-local next-hop.
- NDP message roles (RS/RA/NS/NA).
- Prefix notation and compression rules.
- Required commands to configure/verify IPv6.
- No broadcast; multicast replaces it.

```


## Contents

```less
# IPv6 – Final Checklist (Engineer-Level Document Outline)

## 0. Introduction
- Short history, motivations, exhaustion of IPv4, design goals  
- Core RFCs (IPv6 base spec, addressing architecture, NDP, SLAAC, DHCPv6, extension headers)  
- Differences in philosophy vs. IPv4 (simplicity, extensibility, multicast-centric operations)

## 1. IPv6 Addressing Fundamentals
- 128-bit structure, hexadecimal notation  
- Full notation, compressed notation, zero-compression rules  
- Prefix lengths and subnetting rules  
- Diagram of address structure (Global Routing Prefix / Subnet ID / Interface ID)  
- Calculating and validating prefix boundaries  

## 2. IPv6 Address Types
- **Unicast**
  - GUA (Global Unicast)
  - ULA (Unique Local)
  - Link-local
  - Loopback (::1)
  - Unspecified (::)
- **Multicast**
  - ff00::/8 structure and flags  
  - Well-known groups (ff02::1, ff02::2, …)  
  - Solicited-node multicast (ff02::1:ffXX:XXXX)
- **Anycast**
  - Concept, routing behavior, use cases  

## 3. IPv6 Address Spaces
- Global Unicast 2000::/3  
- Unique Local fc00::/7  
- Link-local fe80::/10  
- Multicast ff00::/8  
- Documentation prefix 2001:db8::/32  
- NAT64 prefix 64:ff9b::/96  
- Summary tables for all categories  

## 4. Address Assignment Mechanisms
- Manual/static addressing  
- **SLAAC**
  - RA flags (A, M, O flags)
  - Prefix advertisement  
  - Default gateway discovery  
- **DHCPv6**
  - Stateless (info-only)  
  - Stateful (full address assignment)  
  - Why DHCPv6 does not provide gateway info  
- **Privacy Extensions**
  - Temporary addresses  
  - Stable privacy addresses  
  - Operational implications  
- **SLAAC vs DHCPv6 comparison matrix**

## 5. Core Functions & Protocols
- **ICMPv6**
  - Error messages, informational messages  
  - Required for PMTU, ND, SLAAC  
- **Neighbor Discovery Protocol (NDP)**
  - NS/NA, RS/RA  
  - On-link determination  
  - Duplicate Address Detection  
  - Neighbor cache states  
- **Multicast Listener Discovery (MLD)**  
- **Extension Headers**
  - Hop-by-Hop  
  - Routing header (RH0 deprecation)  
  - Fragment header  
  - Destination options  
  - Security issues and filtering  
- **Fragmentation & PMTU**
  - No router fragmentation  
  - ICMPv6 Packet-Too-Big dependency  

## 6. IPv6 Routing
- Static routes and next-hop link-local behavior  
- OSPFv3, EIGRP for IPv6, RIPng, MP-BGP  
- Anycast routing principles  
- Route advertisement differences from IPv4  

## 7. IPv6 Packet Structure
- Minimal IPv6 header and all fields  
- Flow label, Traffic Class, Hop Limit  
- Header chain concept  
- Extension headers ordering rules  
- **IPv4 vs IPv6 header comparison table**  
- Impact on processing and forwarding performance  

## 8. Operational Workflow
- Device boot → link-local creation → DAD → RS/RA → GUA assignment  
- SLAAC process detailed  
- Router discovery  
- Neighbor discovery lifecycle  
- Multicast replacing broadcast  
- Testing reachability and prefix validation  

## 9. Migration Techniques
- Dual stack  
- Tunneling (6in4, 6to4, ISATAP, GRE with IPv6)  
- NAT64/DNS64  
- SIIT / 464XLAT  
- Enterprise deployment patterns  
- Migration decision matrix  

## 10. Security
- NDP vulnerabilities  
- Rogue RA attacks  
- SeND (Cryptographically Secured NDP)  
- RA Guard  
- DHCPv6 Guard  
- First-hop security mechanisms  
- ICMPv6 filtering guidelines (what MUST NOT be blocked)  
- Extension header exploitation & filtering policies  
- Firewall IPv6 hardening recommendations  

## 11. Best Practices & No-Goes
- Always use /64 per Layer-2 segment  
- Avoid NAT66 unless absolutely necessary  
- Never block ICMPv6 essential types  
- Prefer stable privacy addresses for clients  
- Allocate hierarchical prefixes (/48, /56) for organizations  
- Document all prefixes and lifetimes  
- Avoid embedding MAC (EUI-64) where privacy is required  

## 12. Troubleshooting
- show ipv6 interface  
- show ipv6 neighbors  
- show ipv6 route  
- debug ipv6 nd  
- ping6, traceroute6  
- Checking RA, ND, MLD  
- Common error patterns:  
  - Missing RA  
  - Duplicate address detection failures  
  - PMTU black holes  
  - Wrong multicast membership  
  - Incorrect link-local next hop  

---

# Mini-Wiki (Abbreviations & Terms)

| Abbreviation / Term | Short Description |
|---------------------|------------------|
| IPv6 | Internet Protocol version 6; 128-bit successor to IPv4 |
| GUA | Global Unicast Address; publicly routable IPv6 address |
| ULA | Unique Local Address; private, non-routable prefix fc00::/7 |
| SLAAC | Stateless Address Autoconfiguration; automatic IPv6 addressing via RA |
| RA | Router Advertisement; IPv6 router broadcasts prefix and flags |
| RS | Router Solicitation; host requests RA from routers |
| NDP | Neighbor Discovery Protocol; replaces ARP in IPv6 |
| NS | Neighbor Solicitation; request for MAC resolution or DAD |
| NA | Neighbor Advertisement; response to NS |
| DAD | Duplicate Address Detection; ensures address uniqueness |
| ICMPv6 | Control protocol required for IPv6 error, ND, PMTU, SLAAC |
| MLD | Multicast Listener Discovery; IPv6 equivalent of IGMP |
| PMTU | Path MTU Discovery; determines usable MTU without fragmentation |
| DNS64 | Mechanism to synthesize AAAA responses from A records |
| NAT64 | Translator allowing IPv6-only hosts to reach IPv4 networks |
| SIIT | Stateless IP/ICMP Translation; IPv4/IPv6 header translation |
| EUI-64 | Method to derive Interface ID from MAC; deprecated for privacy |
| Anycast | One-to-nearest routing to multiple hosts sharing same address |
| Extension Header | Additional IPv6 headers for routing, fragmentation, options |
| RH0 | Deprecated routing header due to security vulnerabilities |
| DHCPv6 | Dynamic Host Configuration Protocol for IPv6 |
| Stateless DHCPv6 | Provides DNS/info only; no address assignment |
| Stateful DHCPv6 | Full address assignment and configuration |
| Link-local | fe80::/10; mandatory local-network address for all IPv6 interfaces |
| Solicited-node multicast | ff02::1:ffXX:XXXX; used for NDP address resolution |
| ff02::1 | All-nodes multicast group |
| ff02::2 | All-routers multicast group |
| Dual Stack | Running IPv4 and IPv6 simultaneously |
| 6in4 | IPv6-in-IPv4 tunneling mechanism |
| ISATAP | Intra-site automatic tunnel addressing protocol |
| NAT66 | IPv6-to-IPv6 NAT; rarely recommended |
| SeND | Secure NDP using cryptographic validation |
| RA Guard | Switch-level protection against rogue RA messages |
| DHCPv6 Guard | Prevents unauthorized DHCPv6 servers |
| Flow Label | IPv6 field used for flow identification and QoS |
| Hop Limit | IPv6 equivalent of TTL |
| 2001:db8::/32 | Reserved documentation prefix |
| 64:ff9b::/96 | NAT64 well-known prefix |
| /64 | Standard IPv6 prefix per Layer-2 segment |
```

# Pv6 – Engineer Level Theory

## 0. IPv6 Short Story (History, Why, When, RFCs)

### **Background & Motivation**

- IPv4 was designed in the late 1970s for academic/military research, not global-scale public internet.    
- The rapid growth of the Internet created an **address exhaustion problem**.    
- NAT extended IPv4’s lifetime but introduced architectural limitations.    
- The Internet Engineering Task Force (IETF) began planning a successor protocol in the early 1990s.    
- Primary design goals:    
    - Provide **practically unlimited address space**.        
    - Reduce complexity introduced by IPv4 extensions (NAT, fragmentation behavior).        
    - Enable cleaner packet forwarding and simpler header processing.        
    - Support for new functionality via **extension headers**.        
    - Improve multicasting and remove broadcast.        
    - Provide autoconfiguration mechanisms (SLAAC).        
    - Make the protocol scalable for decades.        

### **Timeline Overview**

- **1992–1994:** IETF begins work on "IPng" — IP Next Generation.    
- **1995:** IPv6 is officially standardized.    
- **1998–2006:** First implementations appear in BSD, Linux, Cisco IOS.    
- **2012:** “World IPv6 Launch” — major ISPs and content providers enable IPv6 permanently.    
- **2020s → ongoing:** IPv6 adoption becomes operational standard for ISPs, cloud, and mobile networks.    

### **Why IPv6 Was Needed**

- IPv4 provides ~4.3 billion addresses; not enough for:    
    - Globalized internet usage        
    - Mobile devices        
    - IoT growth        
    - Cloud-scale deployments        
- IPv6 provides **2^128 (~3.4×10³⁸)** addresses — enough to give each device on Earth billions of addresses.    
- Removes NAT dependency (although NAT66 exists, it's discouraged).    
- Supports modern requirements:
    
    - Autoconfiguration        
    - True end-to-end connectivity        
    - Efficient routing aggregation        
    - Multicast-based services        
    - Cleaner packet processing        
    - Built-in extensibility (extension headers)        

### **Design Philosophy Changes vs IPv4**

- IPv4 was extended repeatedly, resulting in fragmentation of standards.    
- IPv6 integrates many lessons learned from IPv4:    
    - **No broadcast** → replaced by multicast & anycast.        
    - **Simplified header** for faster routing.        
    - **Flexible extension header chain** instead of IPv4 options.        
    - **Larger address space** reducing complex NAT mechanisms.        
    - **Stateless autoconfiguration** as a native capability.        

### **Core RFCs You Must Know**

These RFCs define the _foundation_ of IPv6:

```less
RFC 2460 → Original IPv6 Specification (replaced by RFC 8200)
RFC 8200 → Current IPv6 Specification (Standard)
RFC 4291 → IPv6 Addressing Architecture
RFC 5952 → IPv6 Address Text Representation Rules
RFC 4861 → Neighbor Discovery Protocol (NDP)
RFC 4862 → Stateless Address Autoconfiguration (SLAAC)
RFC 4443 → ICMPv6
RFC 6105 → RA Guard
RFC 7217 → Stable Privacy Address Generation
RFC 4941 → IPv6 Privacy Extensions (Temporary Addresses)
RFC 4193 → Unique Local Addresses (ULA)
RFC 3587 → Global Unicast Address Format
RFC 4380 → Teredo
RFC 4213 → Basic Transition Mechanisms (6in4, etc.)
RFC 6146/6147 → NAT64 & DNS64
```

### **Key Technical Goals (Summary)**

- Support for **massive global address space**    
- Easier subnetting and hierarchical allocation    
- Stateless and stateful autoconfiguration (SLAAC, DHCPv6)    
- Built-in mandatory ICMPv6 for operational reliability    
- Strong dependency on **multicast** instead of broadcast    
- Packet header designed for hardware-optimized forwarding    
- Clean extensibility for future networking needs

## 1. IPv6 Address Fundamentals

### **1.1 Structure of an IPv6 Address**

- IPv6 addresses are **128-bit** identifiers for interfaces and routing.    
- Written in **hexadecimal**, separated by **eight 16-bit blocks** (hextets).    
- Each block represents **16 bits / 4 hex characters**.    
- Example full form:    
    - `2001:0db8:0000:0000:0000:0000:0000:0001`
        
- Typical structure for unicast:    
    - **Global Routing Prefix** (e.g., first /48)        
    - **Subnet ID** (typically next 16 bits)        
    - **Interface ID** (last 64 bits)        

#### **Diagram – Basic IPv6 Address Structure**

```less
+-------------------------+--------------+-------------------------------+
|   Global Routing Prefix |  Subnet ID   |         Interface ID          |
|        (n bits)         |  (16 bits)   |          (64 bits)            |
+-------------------------+--------------+-------------------------------+
Example (GUA /64):
2001:0db8:abcd:0012 : 02ff:fe34:5678:9abc
^------48 bits------^  ^--16 bits--^   ^-------------64 bits-----------^
```

### **1.2 IPv6 Address Notation**

IPv6 defines **multiple representation rules** for reducing visual complexity.

#### **1.2.1 Full (Uncompressed) Representation**

- Always 8 blocks, each 4 hex digits.    
- Leading zeros **must** be shown.    
- Example:    
    - `fe80:0000:0000:0000:abcd:12ff:fe34:5678`
        

#### **1.2.2 Zero Compression Rules**

1. **Leading zeros in each block may be removed.**    
    - `00ab` → `ab`, `0001` → `1`
        
2. **One sequence of consecutive all-zero blocks may be replaced by `::`.**    
    - `0000:0000:0000` → `::`
        
3. **`::` may appear only once** in an address.    

Examples:

- Full: `2001:0db8:0000:0000:abcd:0000:0000:0001`    
- Remove leading zeros: `2001:db8:0:0:abcd:0:0:1`    
- Compress zeros: `2001:db8::abcd:0:0:1`    
- Final: `2001:db8::abcd:0:0:1`    

#### **1.2.3 Zero Expansion (Reverse Rule)**

When expanding a compressed IPv6 address:
- Restore the missing blocks to reach **exactly 8** hextets.    
- Replace any omitted blocks with `0000`.    
- Add leading zeros as needed.    

Example:
- `fe80::1` → `fe80:0000:0000:0000:0000:0000:0000:0001`
    

---

### **1.3 IPv6 Prefix Length Representation**

- IPv6 uses **CIDR-style notation** identical to IPv4 but on 128 bits.    
- Example:    
    - `2001:db8:abcd::/48`        
    - Prefix length indicates how many bits represent the network part.        

#### **Typical Prefix Assignments**

|Prefix Size|Usage|
|---|---|
|/32|ISP allocation (regional)|
|/48|Enterprise or site allocation|
|/56|Small business site, often per-customer for ISP|
|/64|Required for most LAN segments (SLAAC)|
|/128|Single host address|

#### **Important Operational Rule**

- **/64 is mandatory for SLAAC**  
    The Interface ID must be 64 bits for the autoconfiguration mechanism.
    

---

### **1.4 Counting, Subnetting & Calculations**

- A /64 contains:    
    - `2^(128-64) = 2^64 ≈ 1.8×10^19` addresses.        
- A /48 provides:    
    - `2^(64-48) = 65,536` subnets of size /64.        
- A /56 provides:    
    - `2^(64-56) = 256` subnets of size /64.        

#### **Diagram – Prefix Boundaries for GUA**

```less
2001:0DB8:AAAA:BBBB:CCCC:DDDD:EEEE:FFFF
|---- Global Prefix ----| Subnet ID |--------- Interface ID ---------|
|-------- /48 ----------|---16 bits-|-------------64 bits------------|
```

### **1.5 Validation Checklist for IPv6 Address Formatting**

- Is the address 128 bits total?    
- Does the representation contain max **one** `::`?    
- Does expansion yield **8 hextets**?    
- Are prefix lengths valid (0–128)?    
- If used on a LAN:    
    - Is the subnet **/64**?        
    - Does SLAAC require RA flags?        

---

### **1.6 Key Differences from IPv4 Address Representation**

- IPv6 uses hex, IPv4 uses decimal.    
- IPv6 allows compression; IPv4 does not.    
- IPv4 uses subnet masks; IPv6 uses pure prefix notation.    
- IPv6 supports vastly larger hierarchical structures.    
- IPv6 addresses can automatically self-generate interface IDs.

## 2. IPv6 Address Types

IPv6 defines **three primary address types**: **Unicast**, **Multicast**, and **Anycast**.  
Each has subcategories with specific operational behaviors.  
Broadcast **does not exist** in IPv6.

---

### **2.1 Unicast Addresses**

A **unicast** address identifies a **single interface**. Packets are delivered one-to-one.

#### **2.1.1 Global Unicast Address (GUA) – Public IPv6**

- Prefix: **2000::/3**
    
- Equivalent of IPv4 public addresses.
    
- Globally unique and Internet-routable.
    
- Hierarchical structure for efficient aggregation:
    
    - **Global Routing Prefix**
        
    - **Subnet ID**
        
    - **Interface ID**
        

Example:

```less
2001:db8:abcd:12::1
```

#### **2.1.2 Unique Local Address (ULA) – Private IPv6**

- Prefix: **fc00::/7**
    
    - Typically implemented as **fd00::/8**
        
- Equivalent of IPv4 private ranges (10.0.0.0/8, 192.168.0.0/16).
    
- Intended for:
    
    - Internal networks
        
    - VPN overlays
        
    - Lab environments
        
- Not routable on the public Internet.
    
- Collision avoidance through pseudo-random 40-bit Global ID.
    

Example:

```less
fd12:3456:789a::/48
```

#### **2.1.3 Link-Local Address**

- Prefix: **fe80::/10**
    
- Must exist on **every IPv6-enabled interface**.
    
- Automatically generated at interface-up.
    
- Used for:
    
    - Neighbor Discovery (NDP)
        
    - Router Advertisements (RA)
        
    - Local communication without router
        
    - Next-hop routing decisions (link-local is required for static routes)
        

Characteristics:

- Never routed beyond the local L2 link.
    
- Must have a /64 prefix.
    

Example:

```less
fe80::1a2b:3c4d:5e6f
```

#### **2.1.4 Loopback Address**

- `::1/128`
    
- Equivalent of IPv4 `127.0.0.1`.
    
- Used for local stack testing.
    

#### **2.1.5 Unspecified Address**

- `::/128`
    
- Represents “no address”.
    
- Used only as a **source** before an address is assigned.
    
- Never used as a destination.
    

---
### **2.2 Multicast Addresses**

IPv6 replaces IPv4 broadcast with **multicast-only** mechanisms.

- Prefix: **ff00::/8**
    
- Multicast operates one-to-many.
    
- Used by:
    
    - NDP
        
    - MLD
        
    - Routing protocols (OSPFv3, EIGRPv6, RIPng)
        
    - Service discovery, group services
        

#### **2.2.1 Multicast Address Format**

```less
| 8 bits | 4 bits | 4 bits |        112 bits        |
|  FF    | Flags  | Scope  |     Group Identifier    |
```

**2.2.2 Important Scope Values**

| Scope           | Value | Meaning                    |
| --------------- | ----- | -------------------------- |
| Interface-local | 1     | Local interface only       |
| Link-local      | 2     | Most common; not routed    |
| Admin-local     | 4     | Local admin boundary       |
| Site-local      | 5     | Single site                |
| Organization    | 8     | Global inside organization |
| Global          | E     | Internet-wide multicast    |

**2.2.3 Essential Multicast Groups**

| Address     | Group              |
| ----------- | ------------------ |
| **ff02::1** | All-nodes          |
| **ff02::2** | All-routers        |
| **ff02::5** | All OSPFv3 routers |
| **ff02::6** | OSPFv3 DR/BDR      |
| **ff02::9** | RIPng              |
| **ff02::a** | EIGRP IPv6         |
| **ff02::d** | All PIM routers    |

#### **2.2.4 Solicited-Node Multicast**

Used for NDP (ARP replacement):

- Prefix: **ff02::1:ff00:0000/104**
    
- Host joins automatically for each unicast address.
    

Example derivation:

```less
Unicast:    2001:db8:1:2:abcd:1234:5678:9abc
Last 24 bits:              78:9abc
Solicited-node: ff02::1:ff78:9abc
```

### **2.3 Anycast Addresses**

One-to-nearest delivery (based on routing distance).

Characteristics:

- Same address configured on **multiple interfaces**.
    
- Routers forward packets to the **closest** node.
    
- Used for:
    
    - DNS resolvers
        
    - Distributed services
        
    - Default gateway redundancy
        
- Cannot be distinguished from unicast by syntax; behavior defined by routing.
    

Example:

```less
2001:db8:aaaa::1 configured on multiple routers
```

### **2.4 Special Purpose Addresses**

Summarizing critical special addresses:

|Address / Prefix|Purpose|
|---|---|
|::|Unspecified|
|::1|Loopback|
|fe80::/10|Link-local|
|fc00::/7|Unique Local (private)|
|2000::/3|Global unicast|
|ff00::/8|Multicast|
|2001:db8::/32|Documentation-only|
|64:ff9b::/96|NAT64 well-known prefix|
|::ffff:0:0/96|IPv4-mapped IPv6|

### **2.5 Key Operational Rules**

- **Every IPv6 interface must have a link-local address.**
    
- **A single interface can have multiple unicast addresses** (privacy, stable, temporary).
    
- Routers must forward multicast but never broadcast.
    
- Multicast replaces all broadcast-based mechanisms.
    
- Anycast is implemented by routing, not protocol headers.


## 3. IPv6 Address Spaces

IPv6 organizes its **128-bit global address space** into clearly defined categories.  
Each category follows a prefix-based allocation model and has strict operational behavior.  
This section provides a structured overview of **all major IPv6 address spaces**, their prefixes, usage, and engineering implications.

---

### **3.1 High-Level IPv6 Address Space Overview**

The entire IPv6 address spectrum can be summarized into these functional groups:

| Space                | Prefix        | Purpose                                    |
| -------------------- | ------------- | ------------------------------------------ |
| Global Unicast       | **2000::/3**  | Publicly routable Internet addresses       |
| Unique Local (ULA)   | **fc00::/7**  | Private, organizational addressing         |
| Link-Local           | **fe80::/10** | Mandatory local-link communication         |
| Multicast            | **ff00::/8**  | One-to-many delivery, replaces broadcast   |
| Loopback             | **::1/128**   | Local host testing                         |
| Unspecified          | **::/128**    | No address assigned                        |
| Special & Transition | Various       | NAT64, IPv4-mapped, tunnels, documentation |

### **3.2 Global Unicast Address Space (GUA) — 2000::/3**

- Equivalent of IPv4 **public address space**.
    
- Prefix range:

```less
2000:0000:0000:0000::  →  3fff:ffff:ffff:ffff:ffff:ffff:ffff:ffff
```

- Fully Internet-routable.
    
- Assigned by:
    
    - IANA → RIR (RIPE, ARIN, APNIC, AFRINIC, LACNIC) → ISP → Organization → Subnets
        
- Highly hierarchical for efficient global routing aggregation.
    
- Default subnet size for LAN: **/64** (mandatory for SLAAC).
    

#### Recommended Allocation Model

```less
ISP allocation typically /32 or /29
Enterprise allocation /48
Remote small sites /56
LAN segment /64
```

### **3.3 Unique Local Address (ULA) — fc00::/7**

Private IPv6 addressing for internal-only communication.

- Prefix breakdown:

```less
fc00::/8  → reserved (not widely used)
fd00::/8  → recommended local-use space
```

- Functions like IPv4 private ranges (10.0.0.0/8).
    
- Not routable on the public Internet.
    
- Designed to be **globally unique** (pseudo-random 40-bit Global ID).
    

#### ULA Structure Diagram

```less
| fc or fd | Global ID (40 bits) | Subnet ID (16 bits) | Interface ID (64 bits) |
|   8 bit  |      Random         |      /64 block      |         Host ID        |
```

Example:

```less
fd1a:2b3c:4d5e::/48
```

**Use Cases**

- Internal enterprise networks
    
- VPN-only infrastructures
    
- Lab/reserved scenarios
    
- Environments requiring stable but private addressing

### **3.4 Link-Local Address Space — fe80::/10**

Link-local addressing is **mandatory** for all IPv6-enabled interfaces.

- Prefix:

```less
fe80::/10  →  fe80:0000:0000:0000::/64 usually applied
```

- Exists automatically when an interface comes online.
    
- Never forwarded by routers.
    
- Used for:
    
    - Neighbor Discovery (NDP)
        
    - Router Advertisements (RA)
        
    - Next-hop routing operations
        
    - Autoconfiguration
        
    - Local diagnostics
        

**Engineering Requirement:**  
IPv6 static routes _must_ specify next-hop using **link-local addresses**, not global.

---

### **3.5 Multicast Address Space — ff00::/8**

Replaces broadcast entirely.

- Prefix structure:

```less
FF | Flags | Scope | Group ID (112 bits)
```

- Used for:
    
    - NDP: Solicited-node multicast
        
    - Routing protocols (OSPFv3, RIPng, EIGRPv6, PIM)
        
    - Service discovery
        
    - MLD
        

Multicast is the backbone for IPv6 operational mechanisms.

---

### **3.6 Loopback and Unspecified Address Spaces**

#### Loopback

- `::1/128`
    
- Equivalent to IPv4 127.0.0.1
    
- Stack testing only; never forwarded.
    

#### Unspecified

- `::/128`
    
- Used during initialization:
    
    - Source of DHCPv6 requests
        
    - Source of Duplicate Address Detection (DAD)
        

---

### **3.7 Special & Transition Address Spaces**

#### **3.7.1 NAT64 Prefix — 64:ff9b::/96**

Used for mapping IPv6 to IPv4 hosts.

Example mapping:

```less
IPv4: 192.0.2.33
Mapped IPv6: 64:ff9b::192.0.2.33
```

#### **3.7.2 IPv4-Mapped IPv6 — ::ffff:0:0/96**

Used internally by dual-stack systems.

Format:

```less
::ffff:a.b.c.d
```

#### **3.7.3 Documentation Range — 2001:db8::/32**

Reserved for tutorials, documentation, books.  
Must never be used in real networks.

#### **3.7.4 6to4 Tunneling Range — 2002::/16**

Legacy automatic tunneling mechanism (deprecated in modern networks).

---

### **3.8 Comparison Table of IPv6 Address Spaces**

```less
+----------------------+--------------+-------------------------------+
| Address Space        | Prefix       | Purpose                       |
+----------------------+--------------+-------------------------------+
| Global Unicast (GUA) | 2000::/3     | Public Internet addressing     |
| Unique Local (ULA)   | fc00::/7     | Private internal networks      |
| Link-Local           | fe80::/10    | Local-link only, mandatory     |
| Multicast            | ff00::/8     | One-to-many delivery           |
| Loopback             | ::1/128      | Local host communication       |
| Unspecified          | ::/128       | “No address”, initialization   |
| Documentation        | 2001:db8::/32| Education/testing only         |
| NAT64                | 64:ff9b::/96 | IPv6→IPv4 translation          |
| IPv4-mapped          | ::ffff:0:0/96| Dual-stack compatibility       |
+----------------------+--------------+-------------------------------+
```

### **3.9 Key Engineering Takeaways**

- Most operational IPv6 work revolves around **three spaces**: GUA, ULA, Link-local.
    
- Multicast is deeply integrated—**broadcast does not exist**.
    
- Link-local is required for internal IPv6 functions.
    
- Enterprises should use **/48** allocations for long-term scalability.
    
- Subnet size on LAN segments must remain **/64** for SLAAC compatibility.
    
- NAT is largely unnecessary and discouraged in IPv6 deployments.


## 4. IPv6 Address Assignment Mechanisms

IPv6 provides multiple address assignment methods, ranging from **fully automated** to **manually controlled**.  
This section covers all operational mechanisms for generating, distributing, and managing IPv6 addresses in enterprise and ISP environments.

---

## 4.1 Overview of Address Assignment Methods

IPv6 supports **four primary assignment mechanisms**:

1. **Manual / Static Assignment** – administrator defines full address + prefix + gateway.
    
2. **SLAAC (Stateless Address Autoconfiguration)** – host self-configures its addresses.
    
3. **DHCPv6**
    
    - Stateless (information only)
        
    - Stateful (full address assignment)
        
4. **NAT66 / Translation-based Addressing**
    
    - Generally discouraged
        
    - Used only in special cases (multi-homing, provider independence, legacy constraints)
        

IPv6 allows **multiple addresses per interface** simultaneously:

- Link-local (mandatory)
    
- Global unicast (GUA)
    
- Temporary / privacy addresses
    
- Stable addresses
    

---

## 4.2 Manual / Static Addressing

Manual configuration is similar to IPv4 static addressing.

### Characteristics

- Full control over addressing schema.
    
- Deterministic and predictable host addressing.
    
- Useful for:
    
    - Servers
        
    - Network infrastructure
        
    - Firewalls
        
    - Routers
        
    - Appliances requiring fixed identity
        

### Required Elements

- IPv6 Address + Prefix
    
- Default Gateway (typically a link-local address)
    
- DNS server(s)
    

### Advantages

- Highest level of control
    
- Ideal for critical systems
    

### Disadvantages

- Administrative overhead
    
- High error risk in large networks
    
- Not suitable for dynamic endpoints
    

---

## 4.3 SLAAC (Stateless Address Autoconfiguration)

SLAAC is one of IPv6’s most powerful features.  
It allows hosts to **self-generate** their IPv6 addresses without a server.

### 4.3.1 How SLAAC Works – High-Level Flow

1. **Host creates a link-local address**
    
    - fe80::/10 + Interface ID
        
    - Runs **DAD (Duplicate Address Detection)**
        
2. **Host sends RS (Router Solicitation)**  
    → Requests configuration information from routers.
    
3. **Router responds with RA (Router Advertisement)**  
    RA contains:
    
    - Prefix information
        
    - Flags for SLAAC and DHCPv6
        
    - Default gateway info
        
    - DNS info (optional, RFC 8106)
        
4. **Host generates a Global Unicast Address (GUA)**
    
    - Prefix from RA
        
    - Interface ID from:
        
        - Modified EUI-64
            
        - Stable privacy mechanism
            
        - Temporary privacy address
            
    - Runs DAD
        
5. Host becomes fully operational.
    

---

### 4.3.2 RA Flags Controlling Behavior

Router Advertisements contain two important flags:

|RA Flag|Meaning|Purpose|
|---|---|---|
|**A flag (Autonomous)**|“Use SLAAC to create address”|Controls SLAAC|
|**M flag (Managed)**|“Use DHCPv6 for addressing”|Enables Stateful DHCPv6|
|**O flag (Other configuration)**|“Use DHCPv6 for DNS/Options”|Stateless DHCPv6|

**Common combinations:**

| A   | M   | O   | Host Behavior                      |
| --- | --- | --- | ---------------------------------- |
| 1   | 0   | 0   | SLAAC only                         |
| 1   | 0   | 1   | SLAAC + Stateless DHCPv6           |
| 0   | 1   | 0/1 | Stateful DHCPv6 only               |
| 0   | 0   | 1   | Rare (options only; no addressing) |

### 4.3.3 SLAAC Interface ID Generation Methods

#### 1. **Modified EUI-64**

- Derived from the MAC address.
    
- Insert `FF:FE` in the middle.
    
- Flip the U/L bit.
    
- Example from your uploaded notes:  
    `00:1A:2B:3C:4D:5E` → `021A:2BFF:FE3C:4D5E`
    
- Pros: stable, deterministic
    
- Cons: exposes MAC address → **privacy risk**
    

#### 2. **Stable Privacy Addresses (RFC 7217)**

- Deterministic but does not reveal MAC.
    
- Stable as long as:
    
    - Prefix stays the same
        
    - Secret key unchanged
        
- Recommended for enterprise endpoints.
    

#### 3. **Temporary Addresses (RFC 4941)**

- Frequently rotating, short-lived identities.
    
- Used for outbound traffic to hide host identity.
    
- Not suitable for servers or firewalls.
    

---

## 4.4 DHCPv6

Different from IPv4 DHCP in several ways:

### 4.4.1 Stateless DHCPv6

- Provides **DNS**, **domain**, and other configuration.
    
- Does NOT assign IPv6 addresses.
    
- Works together with SLAAC.
    
- Used when:
    
    - Admin wants SLAAC for addressing
        
    - And DHCPv6 for DNS/options
        

### 4.4.2 Stateful DHCPv6

- Equivalent to IPv4 DHCP (assigns full IPv6 address + prefix).
    
- Requires:
    
    - RA: M flag = 1
        
- Router does NOT provide prefix in RA.
    
- DHCPv6 server assigns:
    
    - IPv6 address
        
    - DNS server
        
    - Lease time
        
    - Other options
        

### 4.4.3 Operational Differences from IPv4 DHCP

- Does **not provide default gateway**  
    (IPv6 requires RA for gateway)
    
- No broadcast – uses multicast.
    
- Designed to coexist with SLAAC.
    

---

## 4.5 NAT for IPv6 (NAT66, NAT64, etc.)

IPv6 aims to **eliminate NAT**, but some forms exist:

### 4.5.1 NAT66 (IPv6-to-IPv6 NAT)

- Translates one IPv6 prefix to another.
    
- Rare, not recommended.
    
- Used only for:
    
    - Provider-independent addressing without BGP
        
    - Mergers / overlapping ULAs
        
    - Some security policies
        

### 4.5.2 NAT64/DNS64

- Maps IPv6-only clients to IPv4 servers.
    
- Used in:
    
    - Transition environments
        
    - IPv6-only mobile networks
        
    - Datacenters moving away from IPv4
        

---

## 4.6 Comparison Table of Address Assignment Methods

```less
+------------------+------------------+---------------------------------------------+
| Method           | Control Level    | Description                                 |
+------------------+------------------+---------------------------------------------+
| Manual           | Very High        | Static config; predictable; admin heavy     |
| SLAAC            | Low              | Host self-configures using RA               |
| Stateless DHCPv6 | Medium           | SLAAC + DHCPv6 options only                 |
| Stateful DHCPv6  | High             | Full address assignment                     |
| NAT66            | Medium           | Prefix translation; avoid when possible     |
| NAT64/DNS64      | Special          | IPv6→IPv4 translation for transition         |
+------------------+------------------+---------------------------------------------+
```

## 4.7 Key Engineering Takeaways

- IPv6 does not rely on DHCP for gateways—**RA defines gateways**.
    
- SLAAC is mandatory for many modern OS (Windows, Linux, iOS, Android).
    
- Interface IDs should avoid exposing MAC addresses → use stable privacy.
    
- Enterprises often deploy:
    
    - SLAAC + Stateless DHCPv6 for clients
        
    - Static or DHCPv6 stateful for servers
        
- NAT66 has almost no benefit and adds complexity; avoid unless necessary.
    
- Multiple IPv6 addresses per interface are normal and expected.


## 5. Core IPv6 Functions & Protocols

IPv6 functionality heavily depends on core control-plane protocols that **replace**, **extend**, or **refactor** mechanisms used in IPv4.  
Unlike IPv4, where many mechanisms were optional or add-ons, IPv6 integrates these functions as **mandatory components** of the protocol suite.

This section describes all major IPv6 protocol components that govern addressing, discovery, communication, forwarding, fragmentation, and multicast.

---

## 5.1 ICMPv6 (Internet Control Message Protocol for IPv6)

ICMPv6 is **essential** for IPv6 operation.  
Blocking ICMPv6 → breaks IPv6.

### **5.1.1 Functions of ICMPv6**

- Error reporting
    
- Informational messaging
    
- Required for:
    
    - SLAAC
        
    - NDP
        
    - Router discovery
        
    - Duplicate Address Detection
        
    - Path MTU Discovery (PMTU)
        
    - Redirect messages
        

### **5.1.2 ICMPv6 Message Types**

|Category|Function|Examples|
|---|---|---|
|Error Messages|Network/host unreachable, Packet Too Big, Time Exceeded|Type 1–4|
|Informational|Echo Request/Reply|Type 128/129|
|Neighbor Discovery|NS, NA, RS, RA, Redirect|Type 133–137|
|MLD|Multicast membership messages|Type 130–132|

### **5.1.3 Important ICMPv6 Types**

- **128** – Echo Request
    
- **129** – Echo Reply
    
- **133** – Router Solicitation (RS)
    
- **134** – Router Advertisement (RA)
    
- **135** – Neighbor Solicitation (NS)
    
- **136** – Neighbor Advertisement (NA)
    
- **137** – Redirect
    
- **2** – Packet Too Big (critical for PMTU)
    

---

## 5.2 Neighbor Discovery Protocol (NDP)

NDP replaces IPv4 ARP, ICMP Router Discovery, ICMP Redirect, IGMP (partially), and more.  
Defined by **RFC 4861**.

### **5.2.1 NDP Functions**

- Discover routers
    
- Autoconfigure addresses (with RA)
    
- Resolve Layer 2 addresses (ARP replacement)
    
- Neighbor reachability detection
    
- Duplicate Address Detection (DAD)
    
- Redirect mechanism
    
- Prefix and MTU advertisement
    

### **5.2.2 NDP Message Types (ICMPv6-Based)**

|Message|Use|
|---|---|
|**RS (133)**|Ask routers for RA|
|**RA (134)**|Provide prefix, flags, gateway, MTU|
|**NS (135)**|Address resolution, DAD|
|**NA (136)**|Response with L2 address|
|**Redirect (137)**|Router informs better next-hop|

---

## 5.3 Duplicate Address Detection (DAD)

Before using any IPv6 address (link-local or global), a node must verify the address is unique.

### **Process**

1. Host sends **NS** to the solicited-node multicast of the address being tested.
    
2. If **no response** → address is unique.
    
3. If another node replies with **NA** → address is duplicate → configuration fails.
    

### **Engineering Note**

- Disable DAD only for edge cases (VRRP conflict recovery, HA systems).
    
- DAD is mandatory for SLAAC and DHCPv6 clients.

## 5.4 Neighbor Cache & Reachability Detection

IPv6 maintains a **Neighbor Cache**, similar to ARP table but more detailed.

### **Neighbor States**

|State|Meaning|
|---|---|
|**INCOMPLETE**|Resolving L2 address via NS|
|**REACHABLE**|Neighbor confirmed reachable|
|**STALE**|No recent traffic; needs validation|
|**DELAY**|Waiting before re-probing|
|**PROBE**|Actively checking reachability|

These states support efficient routing and detection of failing devices.

---

## 5.5 Router Discovery

Hosts learn about routers and prefixes using RS/RA messages.

### **Router Advertisement Contains**

- Prefixes (for SLAAC)
    
- RA flags (A/M/O)
    
- On-link determination
    
- Default gateway (router’s link-local address)
    
- Hop-limit value
    
- MTU
    
- DNS information (RFC 8106)
    

### **Routers send RA:**

- Periodically
    
- When triggered by RS
    

---

## 5.6 Multicast Listener Discovery (MLD)

MLD is the IPv6 equivalent of IGMP in IPv4.  
Defined in **RFC 3810** (MLDv2).

### **Purpose**

- Allows routers to discover multicast listeners.
    
- Ensures multicast traffic is delivered only where needed.
    

### **Key Points**

- Operates between hosts and local router.
    
- Critical for solicited-node multicast groups (used in NDP).
    
- Required for IPv6 multicast routing protocols.
    

---

## 5.7 Solicited-Node Multicast (Core NDP Component)

Every unicast address automatically joins a **solicited-node multicast group**.

### **Purpose**

- Efficient address resolution (ARP replacement)
    
- Duplicate Address Detection
    

### **Structure**

```less
ff02::1:ffXX:XXXX  (last 24 bits reflect the unicast address)
```

This dramatically reduces traffic vs IPv4 broadcast.

---

## 5.8 Extension Headers

Extension headers allow IPv6 to extend functionality without redesigning the core protocol.

### **Ordering Rule (simplified)**

```less
IPv6 Header → Hop-by-Hop → Destination → Routing → Fragment → Authentication → ESP → Upper-layer protocol
```

### **Types of Extension Headers**

|Header|Purpose|
|---|---|
|Hop-by-Hop Options|Router-based processing along entire path|
|Routing Header (RH0 deprecated)|Source routing, limited modern use|
|Fragment Header|Fragmentation by hosts only|
|Destination Options|Info for destination or intermediate nodes|
|AH (Authentication Header)|IPsec authentication|
|ESP (Encapsulating Security Payload)|IPsec encryption|

### **Security Notes**

- RH0 deprecated due to amplification and routing-loop attacks.
    
- Filtering extension headers must be done carefully—not all firewalls handle them properly.
    

---

## 5.9 Fragmentation and Path MTU Discovery (PMTU)

In IPv6, **routers never fragment packets**.  
Fragmentation is performed **only by the sending host**.

### **5.9.1 PMTU Process**

1. Host sends packet assuming MTU (typically 1500 bytes).
    
2. If router cannot forward due to MTU mismatch:
    
    - Router sends **ICMPv6 Packet Too Big (Type 2)** message.
        
3. Host adjusts segment size accordingly.
    
4. Process repeats until optimal MTU is established.
    

### **5.9.2 Engineering Implications**

- Blocking ICMPv6 breaks PMTU → causes black holes.
    
- Tunnel interfaces often impose lower MTUs → must be considered in design.
    
- Fragmented packets are more expensive to process and increase attack surface.
    

---

## 5.10 Redirect Function

Routers use **Redirect** messages to optimize local routing:

- Example: two hosts behind same router → router teaches host the better next-hop.
    
- Equivalent to IPv4 ICMP Redirect.
    
- Security risk if not filtered → First-Hop Security required.
    

---

## 5.11 IPv6 First-Hop Security Mechanisms

Common attack vectors:

- Rogue RA
    
- Fake DHCPv6 servers
    
- NDP spoofing
    
- Redirect poisoning
    

### **Protection Tools**

|Mechanism|Protects Against|
|---|---|
|RA Guard|Rogue routes/prefixes|
|DHCPv6 Guard|Unauthorized DHCPv6|
|ND Inspection|Spoofed NS/NA|
|Source Guard|Address spoofing|
|SeND|Cryptographic NDP (rarely used)|

---

## 5.12 Key Engineering Takeaways

- ICMPv6 is essential; must not be filtered like IPv4 ICMP.
    
- NDP is more sophisticated and more vulnerable than IPv4 ARP → needs protection.
    
- Solicited-node multicast makes address resolution extremely efficient.
    
- Extension headers require careful firewall handling.
    
- PMTU is mandatory for IPv6 end-to-end reliability.
    
- First-Hop Security is a **must** in enterprise networks.



## 6. IPv6 Routing

IPv6 routing follows the same high-level principles as IPv4—packets are forwarded based on the **longest-prefix match** in the routing table—but the **protocol behavior, next-hop logic, and protocol support** differ significantly.  
This section provides a complete engineer-level overview of IPv6 routing fundamentals, static routing, dynamic routing protocol support, operational behavior, and architectural considerations.

---

## 6.1 General Principles of IPv6 Routing

### **6.1.1 Longest Prefix Match (LPM)**

- Same logic as IPv4: router selects the **most specific** prefix.
    
- IPv6 routing tables often contain very large prefix sets (global BGP tables ~200k+).
    

### **6.1.2 Interface Requirements**

- Every IPv6 interface **must have a link-local address**.  
    Routers use **link-local** next-hop addressing even when forwarding **global** traffic.
    

### **6.1.3 Routing Table Structure**

Typical IPv6 routing table entries include:

- **Destination prefix**
    
- **Admin distance**
    
- **Metric**
    
- **Next-hop (usually link-local)**
    
- **Outgoing interface**
    

Example (conceptual):

```less
2001:db8:abcd::/64 via fe80::1 dev Gig0/0 metric 10
```

### **6.1.4 No Concept of IPv4 Broadcast**

Routing protocols rely on:

- **Multicast**
    
- **Link-local addresses**
    
- **Solicited-node multicast**
    
- **All-routers multicast (ff02::2)**
    

---

## 6.2 Static Routing in IPv6

### **6.2.1 IPv6 Static Route Requirements**

- Must specify:
    
    - Destination prefix
        
    - Outgoing interface **AND** link-local next-hop
        
- Link-local is mandatory because global addresses may not be reachable on-link.
    

### **6.2.2 Example Structure**

```less
ipv6 route <prefix> <interface> <link-local-next-hop>
```

### 6.2.3 Default Route

```less
::/0  → IPv6 default route
```

### **6.2.4 Administrative Distance**

Same concept as IPv4:
- Static: AD 1
    
- Connected: AD 0
    
- OSPFv3, EIGRPv6, RIPng have their own values
    

### **6.2.5 Pros and Cons**

**Pros:**

- Predictable, stable, secure, low overhead.
    

**Cons:**

- High administrative overhead
    
- No automatic failover
    
- Must manually define all paths
    

---

## 6.3 IPv6 Dynamic Routing Protocols Overview

IPv6 requires routing protocols to support:

- 128-bit addresses
    
- Multicast-only neighbor discovery
    
- Link-local next-hop resolution
    
- Prefix-based routing without classful boundaries
    

### **Routing Protocol Support Summary**

|Protocol|IPv6 Version|Notes|
|---|---|---|
|OSPFv3|Yes|New protocol for IPv6; supports address families|
|EIGRP for IPv6|Yes|Almost identical to IPv4 EIGRP|
|RIPng|Yes|Simple distance-vector protocol|
|MP-BGP (Multiprotocol BGP)|Yes|Global Internet routing standard|
|IS-IS|Yes|Native multi-protocol design; widely used in carriers|

---

## 6.4 OSPFv3 (Open Shortest Path First for IPv6)

### **6.4.1 Key Characteristics**

- Defined in RFC 5340.
    
- Separate protocol from OSPFv2.
    
- Runs on **link-local addresses**.
    
- Uses multicast:
    
    - **ff02::5** → All OSPFv3 routers
        
    - **ff02::6** → DR/BDR
        
- Supports multiple address families (IPv4, IPv6).
    

### **6.4.2 Changes from OSPFv2**

- Router ID still 32-bit (not tied to IPv6 address).
    
- LSAs restructured to remove embedded IPv4 addresses.
    
- Authentication removed from protocol → uses IPsec.
    

### **6.4.3 Area structure remains identical**

- Backbone area 0
    
- Stub, totally stubby, NSSA still exist
    

---

## 6.5 EIGRP for IPv6

### **6.5.1 Characteristics**

- Same DUAL algorithm as IPv4 EIGRP.
    
- Configured under IPv6 directly (no network statements).
    
- Uses multicast **ff02::a**.
    
- Router-id must still be configured (32-bit).
    
- Operates with link-local next-hop.
    

### **6.5.2 Differences from IPv4 EIGRP**

- No automatic route redistribution from IPv4.
    
- Neighbors form using link-local addresses only.
    
- Runs only on enabled interfaces (no global network command).
    

---

## 6.6 RIPng (RIP Next Generation)

### **6.6.1 Characteristics**

- Simple, distance-vector protocol.
    
- Metric: hop count (max 15).
    
- Uses multicast **ff02::9**.
    
- Rare in modern networks except labs or very small deployments.
    

### **6.6.2 Differences from IPv4 RIPv2**

- No authentication (must use IPsec for security).
    
- Routes are IPv6-only.
    
- Uses link-local addresses for next-hop.
    

---

## 6.7 MP-BGP (Multiprotocol BGP)

### **6.7.1 Characteristics**

- Primary protocol for global Internet IPv6 routing.
    
- Supports address families:
    
    - IPv4 unicast
        
    - IPv6 unicast
        
    - VPNv6
        
    - Flowspec
        
- Uses global IPv6 addresses as next-hop in many cases.
    
- Very scalable: supports millions of prefixes.
    

### **6.7.2 Differences from IPv4 BGP**

- Next-hop rules differ for IPv6.
    
- Policy control may require explicit address family definitions.
    
- Peering is usually done using **global unicast** IPv6, not link-local.
    

---

## 6.8 IS-IS for IPv6

### **6.8.1 Characteristics**

- Not tied to IP → originally designed for CLNP.
    
- Supports multiple protocols simultaneously.
    
- Used heavily by carriers and large ISPs.
    
- Extremely scalable network core routing.
    

### **6.8.2 Advantages**

- Link-state protocol without relying on IPv6 multicast.
    
- More flexible TLV structure for large-scale deployments.
    

---

## 6.9 Anycast Routing Behavior

### **6.9.1 What is Anycast?**

- Same IPv6 address assigned to multiple nodes.
    
- Routing automatically selects nearest node based on metric.
    

### **6.9.2 Use Cases**

- DNS root servers
    
- CDN load distribution
    
- Default gateway redundancy
    
- Fast failover for services
    

### **6.9.3 Operational Rules**

- Anycast addresses must be assigned as **/128**.
    
- Must not be used as source addresses.
    
- Routing handles selection → no special host logic.
    

---

## 6.10 Routing Workflow Differences: IPv4 vs IPv6

### **6.10.1 Address Resolution**

- IPv4 → ARP (broadcast)
    
- IPv6 → NDP (multicast)
    

### **6.10.2 Next-hop Resolution**

- IPv6 next-hop is **always** a link-local address.
    

### **6.10.3 No Broadcast → All Routing Protocols Use Multicast**

### **6.10.4 Routing Table Growth**

- IPv6 uses hierarchical allocations → more aggregation potential.
    
- Internet routing tables are still large, but more structured.
    

---

## 6.11 Route Summarization in IPv6

### **6.11.1 Easier Summarization**

- Due to hierarchical delegations (ISP → Org → Sites → Subnets).
    

### **6.11.2 Enterprise Summarization Model**

```less
/48 → Enterprise prefix
/56 → Departments, buildings, or remote sites
/64 → Per VLAN or subnet
```

### **6.11.3 Benefits**

- Cleaner routing tables
    
- Improved convergence
    
- Fewer route advertisements
    

---

## 6.12 Multihoming in IPv6

### **6.12.1 Approaches**

1. **BGP multihoming with provider-independent (PI) space**
    
2. **NAT66 multihoming** (avoid if possible)
    
3. **Multiple prefixes advertised internally** (source-dependent routing)
    

### **6.12.2 Challenges**

- Source address selection becomes complex.
    
- Requires Policy-Based Routing (PBR) or RFC 8028 (solution for correct source/gateway selection).
    

---

## 6.13 Key Engineering Takeaways

- IPv6 routing next-hop is almost always a **link-local address**.
    
- OSPFv3 and EIGRP for IPv6 heavily depend on multicast.
    
- MP-BGP is the foundation of global IPv6 routing.
    
- Summarization is easier and more structured than IPv4.
    
- Routing protocols require IPv6-specific security (RA Guard, DHCP Guard).
    
- IPv6 multihoming is more complex than IPv4 due to source address selection rules.
    

---


## 7. IPv6 Packet Structure

IPv6 introduces a **clean and streamlined packet header**, designed for high-performance forwarding, extensibility, and simplified processing in hardware.  
This section describes the full IPv6 header, all relevant fields, extension headers, and a detailed comparison with the IPv4 header.

---

## 7.1 Design Goals of the IPv6 Header

- Simplify router processing by removing or relocating rarely-used fields.
    
- Support protocol extensibility via **extension header chain**.
    
- Reduce fragmentation overhead.
    
- Optimize for hardware forwarding (consistent fixed-size main header).
    
- Improve QoS and flow identification capabilities.
    

---

## 7.2 IPv6 Base Header Structure

The IPv6 **main header** is always **40 bytes**, unlike IPv4’s variable header (20–60 bytes).

### 7.2.1 IPv6 Header Diagram

```less
+------------------------+------------------------+
| Version (4)            | Traffic Class (8)      |
+------------------------+------------------------+
| Flow Label (20)                                |
+------------------------------------------------+
| Payload Length (16)    | Next Header (8)       |
+------------------------+------------------------+
| Hop Limit (8)                                  |
+------------------------------------------------+
|               Source Address (128)             |
+------------------------------------------------+
|             Destination Address (128)          |
+------------------------------------------------+
```

(Values in parentheses indicate bit-length.)

---

## 7.3 Field-by-Field Description

### **Version (4 bits)**

- Always set to **6**.
    
- Ensures protocol identification.
    

### **Traffic Class (8 bits)**

- IPv6 equivalent of IPv4 TOS.
    
- Supports Differentiated Services (DiffServ) and ECN.
    
- Used for QoS marking.
    

### **Flow Label (20 bits)**

- Allows routers to identify and process packets belonging to the same flow consistently.
    
- Supports QoS and real-time applications.
    
- Intended for hardware-accelerated forwarding.
    

### **Payload Length (16 bits)**

- Size of payload (excluding the 40-byte IPv6 header).
    
- Max value: 65,535 bytes.
    
- For larger payloads → **Jumbo Payload Option** (in Hop-by-Hop header).
    

### **Next Header (8 bits)**

- Indicates:
    
    - Upper-layer protocol (TCP = 6, UDP = 17, ICMPv6 = 58)
        
    - **or** next extension header.
        
- Equivalent to IPv4’s Protocol field.
    

### **Hop Limit (8 bits)**

- Equivalent to IPv4 TTL.
    
- Prevents routing loops.
    

### **Source & Destination Addresses (128 bits each)**

- Unicast, multicast, or anycast.
    
- No broadcast exists.
    

---

## 7.4 Extension Header Concepts

IPv6 supports a flexible header architecture:

### **7.4.1 Why Extension Headers?**

- IPv4 “Options” were rarely implemented and slow.
    
- IPv6 separates optional functions into dedicated headers.
    
- Allows fast forwarding through predictable main header size.
    

### **7.4.2 Extension Header Chain**

Headers appear in a specific order.  
Routers only process specific types; most headers are for endpoints.

### **Ordered List of Extension Headers**


```less
IPv6 Header
  ↓
Hop-by-Hop Options
  ↓
Destination Options (before Routing)
  ↓
Routing Header
  ↓
Fragment Header
  ↓
Authentication Header (AH)
  ↓
Encapsulating Security Payload (ESP)
  ↓
Destination Options (before Upper Layer)
  ↓
Upper Layer Protocol (TCP/UDP/ICMPv6/etc.)
```

## 7.5 Extension Header Types (Detailed)

### **7.5.1 Hop-by-Hop Options Header**

- Must be processed by every router along the path.
    
- Used rarely due to performance impact.
    
- Contains special options such as:
    
    - Jumbo Payload option
        
    - Router alert option
        

### **7.5.2 Routing Header**

- Contains routing instructions for the destination.
    
- **Routing Header Type 0 (RH0)** is deprecated due to severe security vulnerabilities (reflection & amplification).
    

### **7.5.3 Fragment Header**

- Fragmentation occurs **only at the source**.
    
- Routers never fragment IPv6 packets.
    
- PMTU discovery required for correct operation.
    
- Fragment header includes:
    
    - Fragment offset
        
    - Identification
        
    - M (more fragments) flag
        

### **7.5.4 Destination Options Header**

- Contains information processed only by:
    
    - Final destination, or
        
    - Next routing header address in segment routing.
        

### **7.5.5 Authentication Header (AH)**

- Provides connectionless integrity and authentication.
    
- Part of IPsec suite.
    

### **7.5.6 Encapsulating Security Payload (ESP)**

- Provides encryption + integrity.
    
- Also part of IPsec.
    

---

## 7.6 Upper-Layer Protocol Identification

### Common Next Header values

|Protocol|Value|
|---|---|
|TCP|6|
|UDP|17|
|Routing Header|43|
|Fragment Header|44|
|ICMPv6|58|
|No Next Header|59|
|Destination Options|60|
|Authentication Header|51|
|ESP|50|

---

## 7.7 Comparison of IPv4 vs IPv6 Headers

```less
+--------------------------+--------------------------+
| IPv4                     | IPv6                     |
+--------------------------+--------------------------+
| Variable header (20–60B) | Fixed header (40B)       |
| Header checksum          | No checksum              |
| Fragmentation anywhere   | Fragmentation only at src|
| Options in base header   | Extension headers        |
| Broadcast supported      | No broadcast (multicast) |
| TTL                      | Hop Limit                |
| Source routing (rare)    | Routing header           |
| NAT heavily used         | End-to-end addressing    |
| ARP for L2 resolution    | NDP (ICMPv6-based)       |
+--------------------------+--------------------------+
```

## 7.8 Performance & Security Implications

### **Performance**

- Fixed header enables faster hardware parsing.
    
- Router processing simplified—only Hop-by-Hop headers may slow forwarding.
    
- Extension headers allow load-balancing and QoS enhancements.
    

### **Security**

- Extension headers add complexity to firewall and ACL processing.
    
- Fragment header is commonly abused for evasion attacks.
    
- RH0 required global deprecation due to serious risks.
    

### **Operational Considerations**

- Firewalls must parse long extension header chains.
    
- Some networks drop packets with multiple extension headers.
    
- Network devices should follow RFC 7112 (complete header chain in first fragment).
    

---

## 7.9 Header Parsing Example (With Extension Headers)

```less
IPv6 Header
Next Header → Routing Header
  ↓
Routing Header
Next Header → Fragment Header
  ↓
Fragment Header
Next Header → Destination Options
  ↓
Destination Options
Next Header → TCP
  ↓
TCP Header → Payload
```

Routers may skip processing of most extension headers except Hop-by-Hop.

---

## 7.10 Key Engineering Takeaways

- IPv6 header is fixed at 40B → predictable and fast.
    
- IPv4 fields removed: checksum, options, ID, flags, fragmentation offset.
    
- Fragmentation logic fully redesigned: responsibility shifted to source host.
    
- Extension headers provide structured flexibility—must be handled by security devices.
    
- IPv6 forwarding performance is superior due to simplified core header.
    
- Understanding extension headers is mandatory for advanced operations and security hardening.


## 8. Operational Workflow of IPv6

This section describes **how IPv6 behaves in real networks**—from the moment a device boots, to obtaining addresses, joining multicast groups, discovering routers, learning neighbors, selecting source addresses, and ultimately routing traffic.  
Understanding this workflow is critical for troubleshooting, design, and building mental models of IPv6 communication.

---

## 8.1 Boot Process & Initial Conditions

When an IPv6-enabled interface comes online:

### **8.1.1 The Interface Automatically Generates a Link-Local Address**

- Prefix: **fe80::/10**
    
- Interface ID generated using:
    
    - EUI-64
        
    - Stable privacy (RFC 7217)
        
    - Static config (rare)
        
- Before using the address:
    
    - **Duplicate Address Detection (DAD)** is performed.
        

### **8.1.2 The Node Joins Important Multicast Groups**

Upon initialization, the host automatically subscribes to:

|Multicast Group|Purpose|
|---|---|
|**ff02::1**|All-nodes|
|**ff02::1:ffXX:XXXX**|Solicited-node group (per address)|
|**ff02::2**|(Routers only) all-routers|
|MLD relevant groups|Multicast membership|

This replaces IPv4 broadcast behavior.

---

## 8.2 Duplicate Address Detection (DAD)

After forming its link-local address:

1. Host sends **NS** to its own solicited-node multicast.
    
2. If **no NA** received → address is usable.
    
3. If NA received → DAD fails → interface disables that address.
    

DAD applies to:

- Link-local addresses
    
- SLAAC addresses
    
- DHCPv6-assigned addresses
    
- Temporary privacy addresses
    

---

## 8.3 Router Discovery

Once link-local is ready, the host needs:

- Global Unicast Address (GUA)
    
- Default gateway
    
- Prefix information
    
- Optional DNS information
    

### **8.3.1 Host Sends Router Solicitation (RS)**

- Sent to: **ff02::2** (all routers)
    
- Purpose: Request immediate Router Advertisement (RA)
    

### **8.3.2 Router Sends Router Advertisement (RA)**

Contains:

- Prefix(es) for SLAAC
    
- Flags for DHCPv6 (M-flag, O-flag)
    
- Default gateway (router’s link-local)
    
- On-link prefix info
    
- MTU
    
- DNS info (RFC 8106)
    

---

## 8.4 Global Address Formation

After receiving RA:

### **8.4.1 Host Determines Address Assignment Method**

Based on RA flags:

|A|M|O|Host Action|
|---|---|---|---|
|1|0|0|SLAAC|
|1|0|1|SLAAC + DHCPv6 (stateless)|
|0|1|0/1|Stateful DHCPv6|
|0|0|1|Options only (rare)|

### **8.4.2 SLAAC Flow**


```less
Prefix from RA (/64)
     ↓
Interface ID generated (EUI-64 or stable privacy)
     ↓
Combine → 2001:db8:abcd:1::<interface-id>
     ↓
Perform DAD
     ↓
Address is ready
```

### **8.4.3 DHCPv6 Stateful Flow**

- Host multicasts DHCPv6 SOLICIT.
    
- DHCPv6 server replies with ADVERTISE.
    
- Client REQUEST → server REPLY.
    
- Host receives:
    
    - IPv6 address
        
    - Lease time
        
    - DNS server
        
    - Other configs
        
- DAD still required.
    

### **8.4.4 Host May Generate Temporary Privacy Addresses**

- Used for outbound flows.
    
- Not suitable for incoming connections.
    

---

## 8.5 Default Gateway Selection

### **Key Rule**

The default gateway in IPv6 **must always be a link-local address**.

Example:

```less
default route via fe80::1 on interface eth0
```

Decision criteria:

- Router with highest preference in RA.
    
- Router with reachable next-hop (NDP state REACHABLE).
    
- If multiple equal routers → round robin or host-specific logic.
    

---

## 8.6 Neighbor Discovery Lifecycle (Replacing ARP)

IPv6 uses NDP for L2 resolution.

### **8.6.1 Address Resolution (Replacing ARP)**

1. Host needs MAC of next-hop.
    
2. Sends **NS** to solicited-node multicast of target address.
    
3. Target replies with **NA** containing Layer-2 address.
    
4. Host updates Neighbor Cache.
    

### **8.6.2 Neighbor Reachability Tracking**

Routers/hosts determine if a neighbor is still reachable:

- REACHABLE → recently confirmed
    
- STALE → may probe soon
    
- DELAY → waiting before probe
    
- PROBE → send NS again
    
- INCOMPLETE → unresolved
    

This improves resilience vs IPv4 ARP.

---

## 8.7 On-Link Determination

IPv6 nodes use:

- Prefix information in RA
    
- NDP Neighbor Solicitation responses
    
- Router behavior
    

to determine if a destination is on the same link.

Criteria:

- Same prefix ⇒ likely on-link
    
- Router telling "not on-link" via RA flag indicating on-link behavior
    
- If unsure ⇒ send NS probe
    

---

## 8.8 Source Address Selection

Multiple addresses per interface require selection rules (RFC 6724).

### Order of Preference

1. **Use the most specific matching prefix**.
    
2. Prefer:
    
    - GUA over ULA (if destination is GUA)
        
    - ULA if destination is ULA
        
3. Avoid deprecated addresses.
    
4. Temporary addresses used for outbound flows (privacy-enabled hosts).
    

Misconfiguration of this logic causes routing failures, especially in multihoming scenarios.

---

## 8.9 Routing Workflow (Host Perspective)

1. Determine destination address type:
    
    - Link-local
        
    - On-link GUA
        
    - Off-link GUA
        
    - ULA
        
    - Multicast
        
2. If **on-link** → resolve via NDP
    
3. If **off-link** → forward to default gateway
    
4. Source address selected according to rules
    
5. Host verifies path via PMTU
    
6. Packet sent with:
    
    - Hop Limit decremented at each router
        
    - Correct next-hop from routing table
        

---

## 8.10 Multicast Behavior in Normal Operations

IPv6 uses multicast for virtually all discovery operations:

|Function|Multicast Group|
|---|---|
|All hosts|ff02::1|
|All routers|ff02::2|
|Solicited-node multicast|ff02::1:ffXX:XXXX|
|OSPFv3|ff02::5, ff02::6|
|EIGRPv6|ff02::A|
|RIPng|ff02::9|

### Benefits vs IPv4 Broadcast

- Traffic is filtered to only interested nodes.
    
- Reduces broadcast domain noise.
    
- More scalable for large L2 segments.
    

---

## 8.11 PMTU Discovery in Operational Workflow

For every new destination:

- Host sends packets at assumed MTU (e.g., 1500).
    
- If too large:
    
    - Router sends **ICMPv6 Packet Too Big**.
        
- Host adjusts MTU accordingly.
    
- Prevents fragmentation in the network.
    

Blocking ICMPv6 leads to:

- PMTU black holes
    
- Slow or failing connections
    

---

## 8.12 Getting Ready for Communication (Final Lifecycle Summary)

```less
Interface up
   ↓
Link-local generated
   ↓
Join multicast groups
   ↓
DAD for link-local
   ↓
Send RS
   ↓
Receive RA
   ↓
Generate GUA (SLAAC or DHCPv6)
   ↓
DAD for GUA
   ↓
Install default gateway (link-local)
   ↓
Resolve neighbors (NDP)
   ↓
Perform PMTU discovery
   ↓
Start normal IPv6 communication
```

This lifecycle happens automatically and continuously adapts to topology changes.

---

## 8.13 Key Engineering Takeaways

- Link-local addresses are foundational—everything depends on them.
    
- RA & RS regulate almost all network parameters (gateway, prefix, MTU).
    
- NDP is more advanced but also more vulnerable than ARP—requires First-Hop Security.
    
- PMTU is critical for IPv6 reliability; ICMPv6 must not be blocked.
    
- Temporary and stable privacy addresses improve security but add operational complexity.
    
- Multicast replaces broadcast in every discovery mechanism.
    
- Understanding this workflow is mandatory for IPv6 troubleshooting.


## 9. IPv6 Migration Techniques

IPv6 migration is **not** a single strategy but a toolbox of mechanisms enabling coexistence, transition, and long-term IPv6 dominance.  
Since IPv6 is not backward-compatible with IPv4, networks must use structured migration models combining **dual-stack**, **tunneling**, and **translation** technologies.

This section provides a complete engineer-level overview of all standardized migration techniques, their behavior, advantages, drawbacks, and deployment roles.

---

## 9.1 Migration Strategy Overview

There are three main categories:

1. **Dual-Stack** – Devices run IPv4 + IPv6 simultaneously (preferred where possible).
    
2. **Tunneling** – Encapsulate IPv6 inside IPv4 (or vice versa) to traverse incompatible infrastructure.
    
3. **Translation** – Allow IPv6-only systems to communicate with IPv4-only systems (NAT64/DNS64).
    

### Key Goals of Migration

- Maintain service continuity.
    
- Gradually reduce IPv4 dependency.
    
- Allow IPv6-only networks where beneficial (mobile, datacenter, cloud).
    
- Ensure interoperability during long transition phase.
    

---

## 9.2 Dual-Stack (Preferred Long-Term Model)

### **9.2.1 Concept**

Each interface runs both:

- An IPv4 address
    
- An IPv6 address
    

The network stack selects the appropriate protocol automatically.

### **9.2.2 Advantages**

- Maximum compatibility
    
- Simplest troubleshooting (native IPv4 + native IPv6)
    
- No encapsulation overhead
    
- No translation issues or protocol limitations
    

### **9.2.3 Disadvantages**

- Requires dual routing tables
    
- Doubles network complexity
    
- IPv4 address exhaustion still exists
    
- Requires firewall rules for both stacks
    

### **9.2.4 Suitable For**

- Enterprise networks
    
- WAN environments
    
- ISP backbones
    
- Cloud and datacenter networks
    

---

## 9.3 Tunneling Mechanisms

Tunnels transport one protocol through another.  
IPv6 tunnels help traverse infrastructure not yet natively IPv6-enabled.

### Two Categories:

- **Manual / Configured tunnels**
    
- **Automatic tunnels** (mostly deprecated)
    

---

## 9.3.1 Manually Configured Tunnels

### **a) IPv6-in-IPv4 Tunnel (Static 6in4)**

Encapsulates IPv6 in IPv4 using protocol 41.

**Use Cases**

- Connecting IPv6 islands over IPv4 infrastructure
    
- Labs and point-to-point setups
    

**Pros**

- Simple
    
- Stable
    
- Fast
    

**Cons**

- Requires static endpoints
    
- Not scalable for dynamic environments
    

---

### **b) GRE Tunnel (Generic Routing Encapsulation)**

Encapsulates IPv6 inside GRE inside IPv4 or vice versa.

**Pros**

- Supports multiprotocol
    
- Supports running routing protocols inside tunnel
    
- Flexible
    

**Cons**

- Adds overhead
    
- Requires MTU planning (fragmentation risk)
    

---

### **c) IPsec Tunnel Mode**

Encrypted transport of IPv6 via IPv4.

**Pros**

- Secure
    
- Suitable for VPNs
    

**Cons**

- Higher overhead
    
- Complex to configure

## 9.3.2 Automatic Tunnels (Mostly Deprecated)

### **a) 6to4 — 2002::/16 (Deprecated)**

- Automatically derives IPv6 prefix from public IPv4 address.
    
- No longer recommended due to:
    
    - Reliability issues
        
    - Dependence on relay routers
        
    - Poor performance
        

### **b) Teredo — 2001::/32 (Deprecated)**

- Tunnels IPv6 over UDP/IPv4 to traverse NAT.
    
- Used by older Windows systems.
    
- Now disabled in almost all modern deployments.
    

### **c) ISATAP (Intra-Site Automatic Tunnel Addressing Protocol)**

- Provides IPv6 connectivity over IPv4 inside enterprise networks.
    

**Status:**  
Still used in specialized legacy environments but discouraged.

---

## 9.4 Translation Mechanisms

Translation is required when:

- Network is IPv6-only
    
- Destination is IPv4-only
    

The primary mechanism is **NAT64/DNS64**, defined in modern transition architectures.

---

## 9.4.1 NAT64 (RFC 6146)

### **Concept**

- IPv6-only clients communicate with IPv4-only servers.
    
- NAT64 translates:
    
    - IPv6 → IPv4
        
    - IPv4 → IPv6
        

### **Requirements**

- NAT64 gateway
    
- DNS64 synthesis (for converting A → AAAA records)
    

### **Prefix**

```less
64:ff9b::/96
```

### **Advantages**

- Enables IPv6-only devices to operate without IPv4 stack.
    
- Essential in:
    
    - Mobile networks (e.g., T-Mobile USA)
        
    - Large-scale ISPs
        
    - IPv6-only datacenters
        

### **Disadvantages**

- IPv4 literals in applications do not work.
    
- Some protocols break (embedded IPv4 addresses).
    
- Traceability complex.
    

---

## 9.4.2 DNS64 (RFC 6147)

### Mechanism

- When IPv6-only client queries IPv4-only domain:
    
    - DNS64 receives A record
        
    - Synthesizes a AAAA record by embedding IPv4 into NAT64 prefix
        

### Example

```less
IPv4 Server: 192.0.2.55
Synthesized IPv6: 64:ff9b::192.0.2.55
```

### Purpose

- Makes IPv4-only servers transparent to IPv6-only clients.
    

---

## 9.4.3 SIIT (Stateless IP/ICMP Translation) — RFC 6145

### Features

- Stateless; no per-session tracking.
    
- Often used as foundation for other mechanisms (464XLAT).
    

### Pros

- Fast, lightweight
    
- No NAT table
    

### Cons

- Limited flexibility
    
- No port translation
    

---

## 9.4.4 464XLAT (Used in Mobile Networks)

Modern Android-only mobile networks use:

- **CLAT** on device → stateless translation
    
- **PLAT** (provider NAT64)
    

### Purpose

- Allows IPv4-only apps on IPv6-only networks (carrier-grade environment).
    

---

## 9.5 Enterprise-Grade Migration Models

### **9.5.1 Model 1 — Dual-Stack with Gradual IPv4 Reduction**

- Most widely used in corporate networks.
    
- Deploy IPv6 on all LAN interfaces.
    
- Gradually reduce IPv4 prefixes and NAT pools.
    

### **9.5.2 Model 2 — IPv6-Only Core, Dual-Stack Access**

- Datacenter or campus core runs pure IPv6.
    
- Edge networks run dual-stack for compatibility.
    
- Reduces IPv4 operational overhead.
    

### **9.5.3 Model 3 — IPv6-Only with Translation at Edge**

- Internal servers/clients run IPv6 only.
    
- NAT64/DNS64 at edge enables IPv4 access.
    
- Suitable for:
    
    - Cloud
        
    - Telco
        
    - High-scale deployments
        

### **9.5.4 Model 4 — Isolated IPv6 Islands via Tunnels**

- Transitional for networks lacking IPv6 ISP support.
    
- Connect sites via GRE/6in4 until native IPv6 becomes available.
    

---

## 9.6 Comparison Table of Migration Techniques

```less
+------------------+-----------------------------+---------------------------+
| Method           | Pros                        | Cons                      |
+------------------+-----------------------------+---------------------------+
| Dual-Stack       | Best compatibility          | Doubles complexity       |
|                  | No translation needed       | Still requires IPv4      |
+------------------+-----------------------------+---------------------------+
| Manual Tunnels   | Simple, flexible            | Overhead, MTU issues     |
+------------------+-----------------------------+---------------------------+
| Automatic Tunnels| Easy deployment             | Deprecated, unreliable   |
+------------------+-----------------------------+---------------------------+
| NAT64/DNS64      | Enables IPv6-only networks  | Protocol limitations     |
|                  | Saves IPv4 space            | No IPv4 literal support  |
+------------------+-----------------------------+---------------------------+
| SIIT/464XLAT     | Very scalable               | More complex architectures|
+------------------+-----------------------------+---------------------------+
```

## 9.7 Key Engineering Takeaways

- **Dual-stack is the preferred and most stable migration method**, but costly long-term.
    
- Tunneling is useful short-term but should be removed once native IPv6 is available.
    
- NAT64/DNS64 enable large-scale IPv6-only deployments (mobile carriers, cloud).
    
- Automatic tunnels like 6to4 and Teredo should be avoided.
    
- IPv6-only networks with translation at the edge are increasingly common.
    
- Source address selection becomes critical in multihoming scenarios.
    
- Monitoring and troubleshooting become more complex with mixed environments.


## 10. IPv6 Security

IPv6 introduces **strong architectural improvements**, but it also brings **new threat vectors**, especially in the link-layer and first-hop area.  
Security must be designed into IPv6 deployments from the beginning—retroactive fixes are significantly more difficult compared to IPv4.

This section provides a complete engineer-level overview of IPv6 security: threat modeling, protocol weaknesses, first-hop security mechanisms, ICMPv6 filtering rules, extension header issues, and best practices for enterprise deployments.

---

## 10.1 IPv6 Security Philosophy

### **10.1.1 Core Principles**

- IPv6 was designed with **end-to-end reachability**, not NAT as a security layer.
    
- **ICMPv6 and NDP are mandatory**, making filtering more delicate.
    
- Multicast replaces broadcast → introduces new attack surfaces.
    
- Security must shift toward:
    
    - **First-Hop Security**
        
    - **Source Address Validation**
        
    - **Control-plane protection**
        
    - **Proper firewall design**
        

### **10.1.2 Misconceptions**

- “IPv6 is more secure because it's newer” → **false**
    
- “IPv6 doesn’t need NAT for security” → **correct**, NAT was never a security feature.
    

---

## 10.2 IPv6 Threat Landscape Overview

### **10.2.1 L2/L3 Security Threats**

- Rogue Router Advertisements (RA)
    
- Fake DHCPv6 servers
    
- NDP spoofing (replacement for ARP spoofing)
    
- Redirect attacks
    
- Neighbor cache exhaustion
    
- SLAAC manipulation
    
- Multicast flooding
    
- Fake MLD messages
    

### **10.2.2 IPv6-Specific Threats**

- Interface ID privacy leakage (EUI-64)
    
- Extension header evasion attacks
    
- Fragmentation-based evasion
    
- Dual-stack pivot attacks (IPv6 bypassing IPv4 ACLs)
    
- Tunneling abuse (Teredo, ISATAP, 6to4)
    

### **10.2.3 Host-Level Threats**

- Unprotected services exposed globally
    
- Incorrect source address selection → traffic leaks
    
- Privacy address misuse
    

---

## 10.3 ICMPv6 Security Considerations

Unlike IPv4, ICMPv6 **must remain mostly unfiltered**.

### **10.3.1 ICMPv6 Must-Not-Block List**

The following are **mandatory** for normal IPv6 operation:

- Packet Too Big (Type 2) → PMTU
    
- Time Exceeded (Type 3)
    
- Parameter Problem (Type 4)
    
- Echo Request/Reply (Types 128/129)
    
- RS (133), RA (134), NS (135), NA (136)
    
- MLD (130–132)
    

Blocking these results in:

- PMTU black holes
    
- Broken SLAAC
    
- Failure in NDP → neighbor unreachable
    
- Lost router discovery
    

### **10.3.2 ICMPv6 That _Can_ Be Filtered**

- Redirects (Type 137)
    
- Router Advertisements on untrusted ports
    
- All incoming unsolicited error messages at edges
    

---

## 10.4 NDP Security Issues

NDP replaces ARP and inherits + expands its vulnerabilities.

### **10.4.1 NDP Attack Vectors**

- Neighbor spoofing (fake NS/NA)
    
- Router spoofing (fake RA)
    
- Neighbor cache poisoning
    
- DAD DoS → prevent interface from getting address
    
- Redirect spoofing
    
- NDP table exhaustion (like ARP cache DoS)
    

### **10.4.2 Why NDP Is More Vulnerable Than ARP**

- Larger protocol surface
    
- More message types
    
- Uses multicast (more attack scope)
    
- More critical roles (gateway, prefix distribution)
    

---

## 10.5 First-Hop Security (FHS)

First-Hop Security (Cisco term; generic concept across vendors) provides protection for IPv6's vulnerable mechanisms.

### **10.5.1 RA Guard**

Prevents rogue router advertisements.

**Protects against:**

- Fake gateway announcements
    
- Wrong prefixes
    
- Malicious on-link information
    

### **10.5.2 DHCPv6 Guard**

Prevents unauthorized DHCPv6 servers from handing out addresses.

### **10.5.3 ND Inspection / NDP Snooping**

Validates:

- NS and NA messages
    
- Mapping between IPv6 and MAC addresses
    
- Prevents neighbor cache poisoning
    

### **10.5.4 Source Guard (IPv6 Source Address Validation)**

Ensures that:

- IPv6 source address matches the address learned on the port
    
- Prevents spoofing attacks
    

### **10.5.5 Binding Table (SAVI – Source Address Validation Improvement)**

Tracks:

- IPv6 address
    
- MAC address
    
- Port
    
- Prefix
    
- Binding lifetime
    

Used by ND Inspection, DHCPv6 Guard, and Source Guard.

---

## 10.6 RA & DHCPv6 Attacks (Examples)

### **10.6.1 Rogue Router Advertisement**

Attacker:

- Sends RA with high preference
    
- Clients switch default gateway → man-in-the-middle
    
- Or attacker sets lifetime to 0 → DoS
    

### **10.6.2 Fake DHCPv6 Server**

Attacker offers:

- Wrong DNS server → traffic hijack
    
- Wrong prefix → blackhole
    
- Wrong domain → MITM or exfiltration
    

### **10.6.3 DAD DoS**

Attacker responds to all DAD NS with fake NA:

- Hosts think address is duplicate
    
- Host unable to configure any IPv6 address
    

---

## 10.7 Extension Header Security Issues

### **10.7.1 Firewall Evasion via Extension Headers**

Attackers manipulate:

- Header chain reordering
    
- Fragmentation
    
- Oversized or nested headers
    

Poorly implemented firewalls fail to parse chain correctly.

### **10.7.2 RH0 (Routing Header Type 0)**

- Allowed source-routing attacks
    
- Amplification and traffic reflection
    
- **Fully deprecated (RFC 5095)**
    

### **10.7.3 Fragmentation Header Abuse**

Attackers:

- Embed malicious payload in second fragment
    
- Confuse IDS/IPS due to reassembly logic flaws
    
- Avoid L4 header inspection
    

Recommendation:

- Drop first fragments without L4 headers
    
- Follow RFC 7112 (complete header chain in first fragment)
    

---

## 10.8 Firewalling IPv6 Traffic

IPv6 firewalls must:

### **10.8.1 Allow Mandatory ICMPv6**

Otherwise:

- SLAAC breaks
    
- NDP breaks
    
- PMTU breaks
    
- End-to-end connectivity becomes unpredictable
    

### **10.8.2 Implement Stateful Filtering**

Stateless ACLs often fail due to:

- Multiple addresses per host
    
- Temporary/privacy addresses
    
- Extension headers
    

### **10.8.3 Implement IPv6-Specific Rules**

Examples:

- Deny inbound all-routers multicast from end hosts
    
- Allow RA only on trusted router-facing interfaces
    
- Filter extension headers carefully
    
- Enforce link-local protections
    

---

## 10.9 Dual-Stack Security Considerations

### **10.9.1 Dual-Stack = Double Attack Surface**

Every service must be firewalled in:

- IPv4
    
- IPv6
    

### **10.9.2 Common Misconfigurations**

- IPv4 ACL applied but IPv6 ACL missing
    
- IPv6 enabled by default → exposed services
    
- NAT in IPv4 unintentionally hides IPv4 problems but not IPv6
    

### **10.9.3 Tunneling Abuse**

Attackers use:

- Teredo
    
- 6to4
    
- ISATAP
    

to bypass IPv4-only firewalls.

Recommendation:

- Block unauthorized tunneling protocols at perimeter.
    

---

## 10.10 Hardening Hosts & Infrastructure

### **10.10.1 Host Hardening**

- Disable unused IPv6 interfaces.
    
- Disable EUI-64 if privacy needed.
    
- Enable stable or temporary addresses based on role.
    
- Disable automatic tunnel interfaces (Teredo, ISATAP, 6to4).
    

### **10.10.2 Router Hardening**

- RA throttling
    
- ICMPv6 rate limiting
    
- Disable RH0
    
- Drop suspicious extension header patterns
    
- Enable First-Hop Security features
    

### **10.10.3 Switch Hardening**

- RA Guard
    
- DHCPv6 Guard
    
- ND Inspection
    
- MLD Snooping
    
- SAVI
    

---

## 10.11 Summary of Security Best Practices

### **Allow**

- ICMPv6 essential types
    
- Link-local functions
    
- RA from trusted router ports
    
- DHCPv6 from trusted server ports
    

### **Block**

- RH0 routing headers
    
- Unsolicited inbound multicast (except operational groups)
    
- Untrusted RA/DHCPv6 sources
    
- IPv6-in-IPv4 tunneling unless required
    

### **Monitor**

- NDP tables for anomalies
    
- Duplicate MAC-address mappings
    
- Wrong-prefix announcements
    
- Fragmentation anomalies
    

### **Design**

- Use /64 subnets
    
- Avoid EUI-64 for privacy
    
- Segment networks logically
    
- Apply FHS consistently across switches
    

---

## 10.12 Key Engineering Takeaways

- IPv6 is not inherently more secure—just different.
    
- ICMPv6 and NDP form a fragile trust model → must be protected.
    
- First-Hop Security is mandatory in any enterprise IPv6 deployment.
    
- Extension headers introduce complexity for firewalls and IDS/IPS.
    
- Misconfigured dual-stack exposes services unintentionally.
    
- NAT is not a security mechanism; rely on proper firewalling instead.
    
- Disabling IPv6 in environments lacking IPv6 firewalls is recommended.

## 11. IPv6 Best Practices & No-Goes

This section consolidates all architectural, operational, addressing, and security recommendations that network engineers must follow when designing or operating IPv6 networks.  
These practices apply to enterprise, service provider, datacenter, and campus networks.

---

## 11.1 IPv6 Addressing Best Practices

### **11.1.1 Always Use /64 for LAN Segments**

- Required for SLAAC.
    
- Required for most OS implementations.
    
- Industry-wide standard (RFC 4291, RFC 7421).
    
- Avoid subnets like /80, /96 → break SLAAC, cause inconsistent behavior.
    

### **11.1.2 Use Hierarchical Addressing**

Structure your addressing in layers:

```less
/32 or /29  → ISP allocation
/48         → your organization
/56         → remote sites, departments
/64         → VLANs
```

Benefits:

- Summarization
    
- Clean routing table
    
- Easy automation
    

### **11.1.3 Use Meaningful Subnet ID Structure**

Example:

```less
2001:db8:0001:XXYY::
XX = Site ID
YY = VLAN ID
```

### **11.1.4 Avoid EUI-64 for Interface ID**

Because:

- Reveals MAC → privacy risk
    
- Enables device tracking
    
- Adds no real operational benefit today
    

Preferred:

- **Stable privacy IDs (RFC 7217)**
    
- **Manually assigned interface IDs** (for servers/firewalls)
    

---

## 11.2 Host Addressing Recommendations

### **11.2.1 Allow Multiple Addresses per Interface**

Expected and normal:

- Permanent stable GUA
    
- Temporary privacy address
    
- Link-local address
    

### **11.2.2 Servers Should Use Static or DHCPv6-Stateful**

Avoid temporary addresses for servers:

- Breaks DNS
    
- Breaks firewalling
    
- Breaks logging and auditing
    

### **11.2.3 Clients Should Use SLAAC + Stateless DHCPv6**

- Minimal operational overhead
    
- Automatic prefix propagation
    
- Automatic DNS distribution via DHCPv6/RA
    

---

## 11.3 Router & Gateway Best Practices

### **11.3.1 Default Gateway Should Use Link-Local Address**

Why:

- Stable even when global prefixes change
    
- Allows independent renumbering
    

### **11.3.2 Advertise Only Valid Prefixes**

Avoid:

- Advertised /128, /127 on LAN
    
- Incorrect on-link flags
    
- RA conflicts between routers
    

### **11.3.3 Use HSRP/VRRPv3 or Anycast for Redundancy**

IPv6 supports:

- VRRPv3 for IPv6
    
- HSRPv2/v3
    
- Or anycast default gateway design
    

---

## 11.4 Routing Best Practices

### **11.4.1 Implement Summarization at Every Layer**

IPv6 is designed for hierarchical aggregation.

### **11.4.2 Use Link-Local Addresses for Routing Protocol Neighbors**

Recommended for:

- OSPFv3
    
- EIGRP for IPv6
    
- RIPng
    
- IS-IS
    

### **11.4.3 Filter Routing Protocol Multicast Traffic**

Examples:

- ff02::5 / ff02::6 for OSPFv3
    
- ff02::a for EIGRPv6
    
- ff02::9 for RIPng
    

---

## 11.5 DNS & Naming Best Practices

### **11.5.1 Ensure Dual-Stack DNS Records**

For public-facing services:

- Provide both **A** and **AAAA** records.
    
- Allow IPv6-only clients to reach services.
    

### **11.5.2 Internal DNS**

- Should support dynamic updates.
    
- Should store stable, not temporary addresses.
    

### **11.5.3 Avoid IPv6 Literals**

Applications should use hostnames, not IPv6 numeric addresses.

---

## 11.6 PMTU & Fragmentation Best Practices

### **11.6.1 Never Block ICMPv6 “Packet Too Big”**

Otherwise:

- PMTU black holes
    
- Broken TCP connections
    
- Unstable throughput
    

### **11.6.2 Avoid Fragmentation**

IPv6 fragmentation:

- Is done only by source
    
- Is expensive
    
- Is a common evasion technique (security risk)
    

Use:

- Correct MTU planning
    
- Jumbo frames only when end-to-end supported
    

---

## 11.7 Multicast & NDP Best Practices

### **11.7.1 Ensure MLD Snooping is Enabled on Switches**

Reduces unnecessary multicast flooding.

### **11.7.2 Protect NDP Using First-Hop Security**

Implement on access switches:

- RA Guard
    
- DHCPv6 Guard
    
- ND Inspection
    
- Source Guard
    
- Binding table enforcement (SAVI)
    

### **11.7.3 Monitor Neighbor Tables**

Rapid growth indicates:

- Scanning
    
- Attacks
    
- Misbehaving hosts
    

---

## 11.8 Security Best Practices

### **11.8.1 Allow Essential ICMPv6**

- RA
    
- RS
    
- NS
    
- NA
    
- PTB
    

### **11.8.2 Block High-Risk Traffic**

- Routing Header Type 0
    
- Unknown extension headers
    
- Fragmented first fragments without L4 header (RFC 7112)
    
- Bogon (invalid) address ranges
    

### **11.8.3 Disable Unused Automatic Tunneling**

Disable:

- Teredo
    
- ISATAP
    
- 6to4  
    Unless explicitly needed.
    

### **11.8.4 Firewalls Should Be IPv6-Aware**

Ensure:

- Dual-stack ACLs
    
- Stateful IPv6 inspection enabled
    
- Proper inspection of extension headers
    

### **11.8.5 Hardening Hosts**

- Disable IPv6 if not needed
    
- Disable EUI-64 address generation
    
- Enable stable or temporary privacy addresses
    

---

## 11.9 Operational Best Practices

### **11.9.1 Logging & Monitoring**

Track:

- IPv6 source/destination
    
- RA/DHCP events
    
- NDP table entries
    
- PMTU changes
    

### **11.9.2 Documentation**

Document:

- Prefix hierarchy
    
- Interface IDs for static hosts
    
- Delegation structure (/48, /56, /64)
    

### **11.9.3 Testing**

Test:

- SLAAC & DHCPv6
    
- PMTU
    
- Multicast group behavior
    
- First-hop protections
    

### **11.9.4 Consider IPv6-Only for Certain Environments**

Ideal for:

- Mobile networks
    
- IoT networks
    
- Datacenters
    
- University labs
    

Use NAT64/DNS64 for IPv4 reachability.

---

## 11.10 IPv6 No-Goes (What You Must Avoid)

### **11.10.1 No Subnets Smaller than /64 for Hosts**

These break:

- SLAAC
    
- Many OS stacks
    
- Several routing features
    

### **11.10.2 No Dropping of Core ICMPv6 Messages**

Dropping:

- Packet Too Big
    
- NDP
    
- RA/RS
    

Breaks the network.

### **11.10.3 No Relying on NAT66 for Security**

NAT is:

- Not a security feature
    
- Not needed in IPv6
    
- Introduces complexity
    

### **11.10.4 No Mixing EUI-64 with Static IDs Without Plan**

Creates unpredictable address overlap and traceability issues.

### **11.10.5 No Leaving Automatic Tunnels Enabled**

Used in attacks (Teredo/ISATAP/6to4).

### **11.10.6 No Deploying IPv6 Without Security Controls**

An IPv6-capable but unprotected network is **more vulnerable** than IPv4-only.

---

## 11.11 Key Engineering Takeaways

- IPv6 must be intentionally designed, not “enabled and forgotten.”
    
- **/64 is mandatory** for host networks.
    
- Link-local addresses form the backbone of IPv6 routing and discovery.
    
- Use hierarchical prefixes (prefer /48 for organizational deployment).
    
- Never block essential ICMPv6—it's critical for IPv6 to function.
    
- Protect NDP with First-Hop Security.
    
- Disable unnecessary IPv6 tunnels.
    
- Implement dual-stack firewalls and monitoring.
    
- Use explicit, documented address planning.


## 12. Troubleshooting IPv6

IPv6 troubleshooting requires understanding of **neighbor discovery**, **address lifecycles**, **multicast operations**, and the tight dependency on **ICMPv6**.  
This section provides a complete engineer-level troubleshooting framework with methods, logic flows, commands, and common failure scenarios.

---

## 12.1 Troubleshooting Philosophy

### **12.1.1 Key Principles**

- Troubleshoot **link-local first**, not GUA.
    
- Validate **NDP** before routing.
    
- Ensure **RA/RS exchange** happens as expected.
    
- Confirm **PMTU is not broken** (Packet Too Big must be allowed).
    
- Verify **multicast groups** (solicited-node) for address resolution.
    
- IPv6 failures often appear as:
    
    - Slow connections
        
    - PMTU black holes
        
    - “Works for some hosts but not others”
        

IPv6 is deterministic—follow the workflow and issues become clear.

---

## 12.2 Baseline Troubleshooting Checklist

### **12.2.1 Step 1 — Check Link State**

- Interface UP/DOWN
    
- IPv6 enabled?
    
- Link-local assigned?
    

### **12.2.2 Step 2 — Verify IPv6 Addressing**

Check:

- Link-local (fe80::/10)
    
- GUA/ULA address
    
- Subnet (/64 mandatory for hosts)
    
- Temporary vs stable addresses
    

### **12.2.3 Step 3 — Validate Router Advertisements**

- RA received?
    
- Correct prefix?
    
- Flags correct? (A/M/O)
    

### **12.2.4 Step 4 — Verify NDP & Neighbor Cache**

- Can the device resolve MAC of gateway?
    
- Neighbor state must be REACHABLE or STALE.
    

### **12.2.5 Step 5 — Check Routing**

- Default route via link-local?
    
- Longest-prefix match correct?
    
- Route filtering issues?
    

### **12.2.6 Step 6 — PMTU Verification**

- ICMPv6 Packet Too Big messages allowed?
    

### **12.2.7 Step 7 — DNS Resolution**

- AAAA records available?
    
- DNS64 issues in IPv6-only networks?
    

---

## 12.3 Useful Troubleshooting Commands

Below is a unified reference table for Cisco IOS, Linux, and Windows.


```less
+----------------------------+----------------------------+------------------------------+
| Cisco IOS                  | Linux                      | Windows (PowerShell)         |
+----------------------------+----------------------------+------------------------------+
| show ipv6 interface brief  | ip -6 addr show            | ipconfig /all                |
| show ipv6 interface <int>  | ip -6 neigh                | netsh int ipv6 show neigh    |
| show ipv6 neighbors        | ip -6 route                | netsh int ipv6 show route    |
| show ipv6 route            | tracepath6 <dest>          | tracert -6 <dest>            |
| debug ipv6 nd              | ping6 <dest>               | ping -6 <dest>               |
| show ipv6 nd               | rdisc6 (RA test)           | netsh int ipv6 show int      |
| show ipv6 cef              | tcpdump -i eth0 ip6        | powershell Get-NetNeighbor   |
+----------------------------+----------------------------+------------------------------+
```

## 12.4 Troubleshooting Address Assignment (SLAAC / DHCPv6)

### **12.4.1 SLAAC Problems**

Symptoms:

- No global IPv6 address
    
- Only link-local present
    
- Wrong prefix assigned
    
- Duplicate address detected
    

Checklist:

- RA messages received?
    
- A-flag set?
    
- /64 prefix provided?
    
- DAD successful?
    
- Interface joined correct solicited-node multicast group?
    

### **12.4.2 DHCPv6 Problems**

Symptoms:

- Host has only link-local despite M-flag=1
    
- DNS not applied (stateless DHCPv6)
    
- Server not responding
    

Checklist:

- DHCPv6 server reachable?
    
- O/M flags in RA correct?
    
- Firewall blocks DHCPv6 multicast?
    
- Relay agent configured?
    

---

## 12.5 Troubleshooting NDP (Neighbor Discovery)

NDP failures resemble ARP failures in IPv4.

### **Common Symptoms**

- Host cannot reach gateway
    
- Gateway cannot reach host
    
- Neighbors in INCOMPLETE state
    
- Duplicate address detection loops
    
- Very slow connections (fallback to repeated NS/NA)
    

### **Checklist**

- Is solicited-node multicast group correct?
    
- Is multicast forwarding enabled (MLD snooping)?
    
- Is ND Inspection interfering?
    
- Are link-local addresses correct?
    
- Does neighbor table show REACHABLE state?
    
- Does attacker send fake NA/NS (security issue)?
    

### **Useful Commands**

```less
Linux: ip -6 neigh
Cisco: show ipv6 neighbors
Windows: netsh int ipv6 show neigh
```

## 12.6 Troubleshooting Default Gateway Issues

### **Symptoms**

- Host cannot reach any external network
    
- Host ARP/NDP resolves successfully but routing fails
    
- Asymmetric routing without connection success
    

### **Checklist**

- Default gateway uses link-local? (required)
    
- RA advertises correct default route?
    
- Router preference in RA?
    
- Multiple routers causing conflicts?
    
- Static default route configured improperly?
    

### **NDP Requirement**

Routers must advertise:

- Router lifetime > 0
    
- Valid prefix information
    

---

## 12.7 Troubleshooting Routing Problems

### **Symptoms**

- Off-link communication fails
    
- Only local subnet works
    
- Intermittent reachability
    

### **Checklist**

- Missing route on router(s)?
    
- Summarization conflict?
    
- Wrong longest-prefix match?
    
- Routing filters dropping prefixes?
    
- BGP/OSPFv3 adjacency issues?
    

### **Verify with Commands**

```less
show ipv6 route
show ipv6 ospf3 neighbor
show bgp ipv6 unicast summary
ip -6 route
```

## 12.8 Troubleshooting PMTU Black Holes

### **Symptoms**

- Ping works (small packet)
    
- Web browsing very slow
    
- TCP connections stall under load
    
- Large file transfers hang
    

### **Cause**

ICMPv6 **Packet Too Big** filtered.

### **Checklist**

- Firewall allows ICMPv6 Type 2?
    
- Tunnel MTU issues?
    
- Path MTU < interface MTU?
    

### **Diagnostic Tools**

```less
tracepath6 <destination>
ping6 -s <large-size> <destination>
```

## 12.9 Troubleshooting DNS Issues

### **Symptoms**

- IPv4 works, IPv6 fails
    
- IPv6 works for some domains, not others
    
- AAAA records missing
    
- DNS64 misconfigured in IPv6-only networks
    

### **Checklist**

- Does DNS server provide AAAA?
    
- RA or DHCPv6 provide correct DNS servers?
    
- Is DNS64 synthesizing correct records?
    
- Is IPv6 resolving internal hosts using stable addresses?
    

---

## 12.10 Troubleshooting Multicast Issues

### **Symptoms**

- SLAAC fails
    
- NDP fails
    
- OSPFv3 neighbors don’t form
    
- Router discovery issues
    

### **Checklist**

- Switch MLD Snooping enabled?
    
- Multicast flooding suppressed?
    
- ff02::1, ff02::2 reachable?
    
- Storm-control interfering?
    

---

## 12.11 Troubleshooting Security Controls

### **Potential Issues**

- RA Guard blocking legitimate routers
    
- DHCPv6 Guard blocking intended DHCPv6 server
    
- ND Inspection rejecting valid messages
    
- IPsec header parsing issues
    
- Extension headers blocked incorrectly
    

### **Checklist**

- Review binding table entries
    
- Validate RA/DHCPv6 trust boundaries
    
- Check ACLs for essential ICMPv6
    
- Examine logs for dropped extension headers
    

---

## 12.12 Troubleshooting Dual-Stack Environments

### **Symptoms**

- IPv4 works, IPv6 does not
    
- Prefer IPv4 even when IPv6 available
    
- Services reachable only over one stack
    

### **Checklist**

- Source address selection rules correct?
    
- Default route installed?
    
- IPv6 firewall rules too restrictive?
    
- DNS offers AAAA records?
    
- IPv6 disabled on host interface unintentionally?
    

---

## 12.13 Troubleshooting Tunnels & Transition Mechanisms

### **Symptoms**

- Traffic stalls over GRE or IPsec tunnels
    
- PMTU issues common due to overhead
    
- NAT64 working only for some applications
    

### **Checklist**

- Adjust tunnel MTU (often 1400–1470)
    
- Confirm inner IPv6 routes installed
    
- Verify NAT64 prefix and DNS64 synthesis
    
- Disable deprecated tunnels (6to4, Teredo, ISATAP)
    

---

## 12.14 Tools for IPv6 Troubleshooting (Summary)

### **CLI Tools**

- ping6
    
- traceroute6 / tracepath6
    
- tcpdump / Wireshark
    
- ip -6 commands (Linux)
    
- show ipv6 commands (Cisco IOS)
    

### **Diagnostic Indicators**

- NS/NA absence: NDP broken
    
- RA absence: SLAAC broken
    
- PTB absence: PMTU broken
    
- Solicited-node multicast absent: addressing broken
    
- Link-local incorrect: routing broken
    

---

## 12.15 Common IPv6 Failure Scenarios (Quick Reference)

```less
Scenario: Host only has link-local address
Cause: No RA received or A-flag off
Fix: Check router RA configuration

Scenario: Host cannot reach gateway
Cause: NDP resolution failure
Fix: Check neighbor table, multicast, ND Inspection

Scenario: IPv6 slow but not dead
Cause: PMTU black hole
Fix: Allow ICMPv6 Packet Too Big

Scenario: Clients receive wrong prefixes
Cause: Rogue RA
Fix: Enable RA Guard

Scenario: Routing fails across subnets
Cause: Wrong longest-prefix match or missing route
Fix: Check routing tables, summarization, filters

Scenario: DNS works for IPv4 but not IPv6
Cause: Missing AAAA records or DNS64 misconfiguration
Fix: Check DNS server

Scenario: IPv6 unreachable but IPv4 works
Cause: IPv6 firewall rules missing
Fix: Configure dual-stack firewall
```

## 12.16 Key Engineering Takeaways

- IPv6 troubleshooting starts with **link-local**, not GUA.
    
- NDP failures cause most local-link issues.
    
- RA issues cause most global addressing issues.
    
- PMTU issues cause most performance issues.
    
- ICMPv6 filtering is the most common cause of broken IPv6.
    
- Dual-stack adds complexity: both stacks must be secured & monitored.
    
- Tools are different, but the troubleshooting logic is systematic.




##  1. IPv6 Mini-Wiki (All Terms & Abbreviations)

```less
+----------------------------+-------------------------------------------------------------+
| Term / Abbreviation        | Short Definition                                            |
+----------------------------+-------------------------------------------------------------+
| IPv6                       | Internet Protocol version 6; 128-bit addressing standard    |
| GUA                        | Global Unicast Address; public IPv6, 2000::/3               |
| ULA                        | Unique Local Address; private IPv6, fc00::/7                |
| LLA                        | Link-Local Address; fe80::/10, mandatory per interface      |
| SLAAC                      | Stateless Address Autoconfiguration via RA                  |
| RA                         | Router Advertisement; prefix, flags, gateway info           |
| RS                         | Router Solicitation; host asks router for RA                |
| NDP                        | Neighbor Discovery Protocol; ARP replacement                |
| NS                         | Neighbor Solicitation; resolve L2 or DAD check              |
| NA                         | Neighbor Advertisement; response w/ MAC                     |
| DAD                        | Duplicate Address Detection; ensures uniqueness             |
| ICMPv6                     | Control messages essential for IPv6 operation               |
| MLD                        | Multicast Listener Discovery; IGMP equivalent               |
| PMTU                       | Path MTU discovery; relies on ICMPv6 PTB                    |
| PTB                        | ICMPv6 Packet Too Big; required for PMTU                    |
| M-Flag                     | RA flag: use DHCPv6 stateful                                |
| O-Flag                     | RA flag: DHCPv6 stateless for options                       |
| A-Flag                     | RA flag: SLAAC-enabled                                      |
| DHCPv6                    | IPv6 DHCP protocol; stateful/stateless modes                |
| EUI-64                     | Interface ID derived from MAC; privacy risks                |
| Stable Privacy ID         | RFC7217 deterministic privacy-friendly interface ID         |
| Temporary Address         | Outbound privacy address (RFC4941)                          |
| Anycast                    | One-to-nearest routing behavior                             |
| Multicast                  | One-to-many addressing model (ff00::/8)                     |
| Solicited-node multicast   | ff02::1:ffXX:XXXX; used for NDP                             |
| ff02::1                   | All-nodes multicast                                          |
| ff02::2                   | All-routers multicast                                        |
| 2001:db8::/32              | Documentation/test IPv6 space                                |
| 64:ff9b::/96               | NAT64 mapped address prefix                                  |
| NAT66                      | IPv6-to-IPv6 NAT (avoid)                                     |
| NAT64/DNS64               | IPv6-only → IPv4-only translation & DNS synthesis           |
| SIIT                       | Stateless IP/ICMP Translation                                |
| RH0                        | Routing Header 0; deprecated due to security risk           |
| Hop-by-Hop Header         | Extension header processed by every router                  |
| Fragment Header           | For source fragmentation; routers never fragment IPv6        |
| AH                         | Authentication Header (IPsec)                                |
| ESP                        | Encapsulating Security Payload (IPsec encryption)            |
| VRRPv3                    | First-hop redundancy for IPv6                                 |
| OSPFv3                    | OSPF for IPv6                                                |
| EIGRP for IPv6            | IPv6 variant of EIGRP                                         |
| RIPng                      | RIP for IPv6                                                 |
| MP-BGP                    | Multiprotocol BGP supporting IPv6 AFI/SAFI                   |
| SAVI                      | Source Address Validation Improvement                         |
| RA Guard                   | Blocks rogue router advertisements                           |
| DHCPv6 Guard              | Blocks unauthorized DHCPv6 servers                           |
| ND Inspection             | Validates NDP integrity                                      |
| Source Guard              | Validates host IPv6 source addresses                         |
| MLD Snooping              | Controls IPv6 multicast propagation on switches              |
| PTB Blocking              | Firewall issue causing PMTU black holes                      |
| PI Space                  | Provider-independent IPv6 allocation                          |
+----------------------------+-------------------------------------------------------------+
```




