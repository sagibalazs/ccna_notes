

# Network Topologies and Architectures Part II.


# **TWO-TIER ARCHITECTURE (≈ Early 2000s → Today — Standard Enterprise LAN Design)**

**Article 7 / CCNA 1.2.a — According to your new schema**

---

# **1. Title + Date**

**Two-Tier Network Architecture — Access–Distribution Model (≈ 2000–Present)**

The first modern hierarchical campus topology introduced when networks outgrew simple star or extended star designs, but did not yet require a full 3-tier core.

---

# **2. Visualization (All Types)**

### **A. Basic Two-Tier (Access → Distribution)**

```less
               [Distribution Switch]
              /        |        \
         [Access1] [Access2] [Access3]
          / |  \        ...      / | \
       PCs APs Phones         PCs APs Phones
```

**B. Two-Tier with Redundancy (Best Practice)**

```less
           [Dist A]=================[Dist B]
               |   \            /    |
         [Access1][Access2]...[AccessN]
              \______ Dual uplinks _______/
```

**C. Building Example**

```less
Main Closet (Distribution)
  ├── Floor1 Access Switches
  ├── Floor2 Access Switches
  └── Floor3 Access Switches
```

# **3. Describe the Architecture in Detail (Bullet Points)**

- Two functional layers: **Access** and **Distribution**.    
- **Access Layer** connects all endpoints: PCs, APs, VoIP phones, printers, IoT.    
- **Distribution Layer** aggregates all access switches and provides **L3 routing**.    
- Provides a predictable **north–south** traffic model.    
- Broadcast domains constrained by VLAN boundaries at distribution.    
- STP/RSTP or LACP used to manage redundancy.    
- Redundant distribution switches prevent single-point failures.    
- Forms the architectural base for small and medium enterprise networks.    
- Represents the real-world scalable version of **Extended Star** + **Tree** topologies.    

---

# **4. Variants (Commons → Specific Differences)**

## **Common Characteristics**

- Access switches operate mainly at Layer 2.    
- Distribution switches operate mainly at Layer 3.    
- VLANs terminate at the distribution (SVIs).    
- Gateway redundancy is handled via HSRP/VRRP/GLBP.    
- Uplinks between layers use fiber or EtherChannel.    

---

## **Variant A: Single Distribution Switch**

- Cheapest option.    
- Simple but **not redundant** — distribution failure = outage.    
- Suitable only for small offices or lab networks.    

## **Variant B: Dual Distribution Switches (High Availability)**

- Access switches dual-homed to two distribution switches.    
- STP or LACP handles loop prevention.    
- Distribution switches share a virtual gateway via **HSRP/VRRP/GLBP**.    
- Allows hitless failover if one distribution switch dies.    

## **Variant C: Collapsed Distribution**

- Distribution layer also acts as WAN edge.    
- Used in small/medium buildings with limited hardware.    

---

# **5. Involved Devices, Media, and Protocols**

### **Devices**

- L2 Access switches (PoE for APs/phones)    
- L3 Distribution switches (core-like roles for small networks)    
- Wireless controllers, firewalls, routers connected at distribution    
- APs, hosts, printers, IP phones    
- UPS, rack infrastructure, structured cabling    

### **Media**

- Copper (Cat5e–Cat6A) for endpoints    
- Fiber (MMF/SMF) for uplinks (1/10/25Gbps)    
- LACP EtherChannel bundles    
- PoE for edge devices    

### **Protocols**

- VLANs (802.1Q)    
- STP / RSTP / MSTP    
- LACP (802.1AX)    
- Inter-VLAN routing (SVIs at distribution)    
- HSRP / VRRP / GLBP    
- OSPF/EIGRP for connecting WAN or data center    
- DHCP Snooping, DAI, Port Security    
- QoS (VoIP tagging/queuing)    

---

# **6. How It Works (Step-by-Step)**

## **A. Access → Distribution Basic Flow**

1. Endpoint sends frame to Access switch.    
2. If staying in same VLAN → Access switch handles it locally.    
3. If different VLAN → Access switch sends to Distribution.    
4. Distribution switch routes and sends traffic back down to the destination access switch.    
5. All north–south traffic passes through distribution.    
6. Broadcast traffic stays within VLAN boundaries (restricted by distribution SVIs).    

---

## **B. Dual-Homed Redundant Distribution Flow**

1. Access switch links to both Dist-A and Dist-B.    
2. LACP or STP decides the active forwarding path.    
3. Distribution switches run HSRP/VRRP for gateway redundancy.    
4. If Dist-A fails → default gateway remains available via Dist-B.    
5. Convergence is fast (milliseconds with RSTP + routing).    
6. No user interruption.    

---

## **C. WAN/Firewall Integration**

1. WAN router or firewall connects directly to the distribution layer.    
2. Distribution handles routing between LAN and WAN subnets.    
3. Policy (QoS, ACLs) applied at distribution for central control.    

---

# **7. Best Practices & No-Goes**

### **Best Practices**

- Deploy **two distribution switches** whenever possible.    
- Use **LACP** for uplink aggregation.    
- Limit VLANs per access switch; avoid spanning VLANs across the entire network.    
- Default gateways should be at distribution using **HSRP/VRRP**.    
- Core-like responsibilities (L3) should be concentrated at distribution.    
- Keep access switches simple (L2-focused).    
- Use **fiber** for uplinks; avoid copper for long distances.    
- Enable **STP enhancements**: PortFast, BPDU Guard, Root Guard.    

### **No-Goes**

- Do NOT daisy-chain access switches horizontally.    
- Never create redundant uplinks without STP or LACP → loops.    
- Don’t put ACLs or heavy filtering on access switches → centralize at distribution.    
- Avoid excessive VLAN sprawl.    
- Never rely on a single distribution switch for critical networks.    

---

# **8. Importance**

- Foundation of modern enterprise networks under 1000 users.    
- Simplifies troubleshooting and administration.    
- Supports modular building/floor designs.    
- Offers redundancy without the cost of a 3-tier core.    
- The default architecture for Cisco enterprise deployments.    
- Directly relevant to CCNA concepts: VLANs, STP, LACP, routing, redundancy.    

---

# **9. Pros & Cons (Technical Reasoning)**

### **Pros**

- **Scalable** for medium-sized networks.    
- **Simpler** than three-tier, easier to manage.    
- Distribution provides strong **policy control** and **L3 boundaries**.    
- Redundant distribution gives **high availability**.    
- Efficient use of structured cabling and fiber.    
- Compatible with all modern enterprise features.    

### **Cons**

- Distribution layer becomes a **performance bottleneck** at scale.    
- Less optimal for heavy **east–west** traffic.    
- Not ideal for very large campuses.    
- Requires careful VLAN and STP design.    
- Redundancy adds cost and complexity.    

---

# **10. TL;DR**

Two-Tier Architecture = **Access switches feeding into Distribution switches**.  
Simple, scalable, affordable, and ideal for small/medium enterprises.  
Provides VLAN routing, redundancy, and clean LAN segmentation.  
Forms the stepping stone before adopting a full Three-Tier design.

---

# **11. Sources**

- Cisco Validated Design: Campus LAN — Two-Tier Architecture    
- Cisco CCNA 200-301 Official Cert Guide Vol. 1 (Hierarchical Design)    
- IEEE 802.1D/802.1w/802.1s (STP/RSTP/MSTP)    
- Cisco Enterprise Network Architecture (Collapsed Core Model)    
- CCNA Exam Blueprint v1.1 (Network Fundamentals 1.2.a)


# **THREE-TIER ARCHITECTURE (≈ Early 2000s → Today — Large-Scale Enterprise / Campus Standard)**

**Article 8 / CCNA 1.2.b — According to your new schema**

---

# **1. Title + Date**

**Three-Tier Network Architecture — Access, Distribution, Core (≈ 2000–Present)**

A proven hierarchical design used in **large enterprises, universities, hospitals, and multi-building campuses**. Introduced when two-tier designs could no longer scale due to traffic load, redundancy requirements, and broadcast management.

---

# **2. Visualization (All Types)**

### **A. Classic Three-Layer Model**

```less
                         [Core 1]=======[Core 2]
                           /   \       /    \
                      [Dist1] [Dist2] [Dist3] [Dist4]
                      / | \      |       / | \
               [Acc1][Acc2][Acc3]    [Acc4][Acc5][Acc6]
```

**B. Multi-Building Campus Example**

```less
           [Core Pair in MDF]
           /        |        \
       Building A   Building B   Building C
       [Dist A]     [Dist B]     [Dist C]
         / \           |           / \
     Access SWs   Access SWs   Access SWs
```

**C. Triple Core with Load-Balanced Distribution**

```less
    [Core A]=====[Core B]=====[Core C]
         \           |           /
        [Dist Pairs]---Aggregating---[Dist Pairs]
```

# **3. Description in Detail (Bullet Points)**

- Introduces a **dedicated core layer** above distribution to handle high-speed L3 forwarding.    
- Layers have **strict functional separation**:    
    - **Access:** endpoint connectivity (L2)        
    - **Distribution:** L3 routing, policy, filtering        
    - **Core:** fast, resilient switching with no filtering        
- Scales to **thousands of endpoints** and multiple buildings.    
- Highly modular and suitable for campus-wide extensions.    
- Provides clean design boundaries, predictable traffic flows, and high availability.    
- Separation of control improves redundancy and improves change isolation.    

---

# **4. Variants (Commons → Specific Differences)**

## **Common Characteristics Across All Three-Tier Models**

- Distribution connects Access to Core.    
- Core provides fast transit between distribution blocks/buildings.    
- Redundant devices at each layer increase availability.    
- Layers reflect both **physical cabling hierarchy** and **logical forwarding hierarchy**.    

---

## **Variant A: Traditional Three-Tier**

- The **textbook Cisco design**.    
- Access L2, Distribution L3, Core L3.    
- VLANs do not typically span beyond access blocks.    
- Ideal for large campus deployments.    

## **Variant B: Collapsed Core Three-Tier**

- Distribution and Core partially merge but still maintain separate hardware or roles.    
- Common in mid-size networks transitioning to full three-tier.    

## **Variant C: Multi-Core Regional Design**

- Multiple core switches serving different buildings or departments.    
- Used in hospital complexes and universities.    

---

# **5. Involved Devices, Media, and Protocols**

### **Devices**

- Access switches (L2/PoE)    
- Distribution switches (L3 aggregation, larger chassis or modular switches)    
- Core switches (high-speed backbone switches; often chassis-based)    
- WAN routers, firewalls, load balancers (hang off distribution or core)    
- Wireless controllers    
- Access points and end devices    

### **Media**

- Copper for access cabling    
- Fiber (MMF/SMF) for distribution→core uplinks    
- EtherChannel bundles (LACP)    
- 10/40/100G backbone links    

### **Protocols**

**Layer 2:**

- VLANs (802.1Q)    
- STP / RSTP / MSTP    
- BPDU Guard, Root Guard    
- LACP / EtherChannel    

**Layer 3:**

- OSPF / EIGRP for campus routing    
- HSRP/VRRP/GLBP for gateway redundancy at distribution    
- ECMP in core    

**Other:**

- DHCP Snooping, DAI, IP Source Guard    
- QoS and traffic marking    
- SNMP, Syslog, NTP    

---

# **6. How It Works (Step-by-Step per Variant)**

## **A. Standard Three-Tier Operation**

1. End device sends frame to Access switch.    
2. If traffic must leave VLAN → sent to Distribution switch.    
3. Distribution performs inter-VLAN routing, ACL filtering, QoS actions.    
4. For destinations in other distribution blocks:    
    - Packets go **up** to the Core.        
5. Core switches forward packets **across** the campus at high speed.    
6. Traffic then moves **down** to the destination Distribution block → Access → Endpoint.    

---

## **B. Redundant Core Operation (Dual Core)**

1. All distribution switches connect to **both** core switches.    
2. Cores run L3 routing adjacencies (OSPF/EIGRP).    
3. ECMP paths used for load sharing.    
4. If core A fails → all traffic moves to core B without outage.    
5. Convergence is extremely fast thanks to L3 routing + ECMP.    

---

## **C. Multi-Building Behavior**

1. Each building has local Access + Distribution.    
2. All buildings connect to the shared Core in the MDF.    
3. Core manages inter-building traffic.    
4. Allows VLAN isolation and routing policies per building.    
5. Enables seamless campus mobility (wireless roaming, unified DHCP, etc.).    

---

# **7. Best Practices & No-Goes**

### **Best Practices**

- Use **dual core switches** with ECMP.    
- Keep **core FREE of ACLs**, firewalling, NAT, or filtering.    
- Use **Layer 3 links** from Distribution → Core (avoid spanning-tree in the core).    
- Layer 2 should be confined to Access blocks.    
- Use consistent VLAN numbering and summarizable subnets.    
- Avoid VLANs spanning across buildings.    
- Use **fiber uplinks** with sufficient bandwidth (10–100Gbps).    
- Distribution should run HSRP/VRRP for default gateway redundancy.    

### **No-Goes**

- Do NOT extend Layer 2 across the core.    
- Never put firewall or heavy filtering inside the core → kills performance.    
- Avoid access switches connecting sideways to other access blocks.    
- Do not run STP in the core → routing is preferred for convergence.    
- Don’t design the core with low-end switches → must be high-speed.    

---

# **8. Importance**

- Essential for large-scale networks with thousands of endpoints.    
- Cleanly separates **policy**, **forwarding**, and **endpoint connectivity**.    
- Enables modular building blocks and multi-building campuses.    
- Allows fast convergence using routing protocols instead of STP.    
- The standard for hospitals, universities, corporate HQ campuses, airports, and data centers (prior to leaf–spine).    
- Teaches CCNA students how scalable, redundant networks are built in reality.    

---

# **9. Pros & Cons (Technical Reasoning)**

### **Pros**

- **Highly scalable** — supports many buildings and departments.    
- **Very resilient** — dual-core, dual-distribution, redundant links.    
- **Fast convergence** using L3 routing.    
- **Predictable traffic flows** across the hierarchy.    
- Effective broadcast domain control.    
- Modular design → easy expansion.    

### **Cons**

- More expensive than two-tier.    
- Higher operational complexity.    
- Requires careful IP design and routing structure.    
- Additional hardware (core switches) increases cost.    
- Not ideal for data-center-like east–west traffic (later solved by spine–leaf).    

---

# **10. TL;DR**

Three-tier = **Access → Distribution → Core**.  
The core adds **speed and redundancy** for large enterprises.  
It scales far beyond two-tier, separates roles cleanly, and supports high availability — becoming the long-time standard for campus networks.

---

# **11. Sources**

- Cisco Validated Design: Campus LAN Three-Tier Architecture    
- Cisco CCNA 200-301 Official Cert Guide Vol. 1 (Hierarchical Network Models)    
- IEEE 802.1D/802.1w/802.1s (STP/RSTP/MSTP)    
- Cisco Enterprise Architecture literature    
- CCNA Exam Blueprint v1.1 (Network Fundamentals 1.2.b)



# **SPINE–LEAF ARCHITECTURE (≈ 2011 → Today — Modern Data Center Standard)**

**Article 9 / CCNA 1.2.c — According to your new schema**

---

# **1. Title + Date**

**Spine–Leaf Network Architecture — Modern Data Center Fabric (≈ 2011–Present)**

Introduced by large cloud providers (Facebook, Google) and adopted by Cisco as a scalable, high-performance alternative to three-tier networks **specifically for data centers** with massive east–west traffic.

---

# **2. Visualization (All Types)**

### **A. Classic Spine–Leaf Fabric**

```less
              [Spine1]====[Spine2]====[Spine3]
               /   \        |        /   \
          [Leaf1] [Leaf2] [Leaf3] [Leaf4]
           / | \      |      |     / | \
         SRV SRV SRV  SRV   SRV  SRV SRV SRV
```

Every **Leaf connects to every Spine** — equal-cost multipath (ECMP).

---

### **B. Multi-Pod / Multi-Fabric Example**

```less
Fabric POD 1 <=====> Fabric POD 2
 (Spine/Leaf)         (Spine/Leaf)
```

**C. Spine–Leaf with L3 + VXLAN/EVPN Overlay**

```less
       L3 Underlay (Spines/Leafs form IP routed fabric)
       VXLAN/EVPN Overlay (logical L2/L3 tenant networks)
```

# **3. Description in Detail (Bullet Points)**

- A **two-layer fabric** used primarily in modern data centers.    
- **Leaf switches** connect to devices (servers, firewalls, storage).    
- **Spine switches** interconnect all leaf switches.    
- Every leaf has identical uplinks to all spines → predictable latency.    
- Designed to support “east–west” server-to-server traffic at huge scale.    
- Pure Layer 3 underlay (OSPF/BGP/IS-IS) with equal-cost multipath routing.    
- Overlay networks (VXLAN/EVPN) enable tenant segmentation and virtual L2 extension.    
- Eliminates hierarchical bottlenecks found in three-tier architecture.    
- Provides deterministic network performance regardless of where workloads sit.    

---

# **4. Variants (Commons → Specific Differences)**

## **Common Characteristics of All Spine–Leaf Designs**

- Every Leaf connects to **each Spine**.    
- No Leaf connects directly to Leaf; no Spine connects to Spine.    
- All links are **L3 routed links**.    
- ECMP used for load-balancing.    
- Highly modular → add more leaves or spines to scale.    

---

## **Variant A: Layer 3 Only Fabric (Underlay Only)**

- Simple routed infrastructure.    
- VLANs limited to local leaf.    
- No overlay → not suitable for large virtualized environments.    

## **Variant B: VXLAN/EVPN Fabric (Modern DC Standard)**

- L2 segments extended across fabric using VXLAN tunnels.    
- MP-BGP EVPN provides MAC/IP learning and tenant isolation.    
- Supports VMware, Kubernetes, OpenStack, storage architectures.    

## **Variant C: Multi-Pod or Multi-Site Spine–Leaf**

- Multiple fabrics interconnected.    
- Used by cloud providers and large enterprises.    
- Requires advanced routing/overlay integration.    

---

# **5. Involved Devices, Media, and Protocols**

### **Devices**

- **Leaf switches:** ToR (Top of Rack) or MoR (Middle of Row)    
- **Spine switches:** High-end L3 switches with high port density    
- Servers, hypervisors (VMware, Hyper-V, KVM)    
- Firewalls, load balancers, storage arrays    
- SDN controllers (Cisco ACI, EVPN controllers)    

### **Media**

- 10G/25G to servers    
- 40G/100G/400G spine links    
- Fiber (MMF/SMF) dominant    
- DAC cables for short distances    

### **Protocols**

**Underlay (L3):**

- OSPF    
- IS-IS    
- iBGP    
- ECMP    
- BFD for fast failure detection    

**Overlay (L2/L3 Virtualization):**

- VXLAN (RFC 7348)    
- EVPN (BGP-based)    
- Anycast gateway    
- ND/ARP suppression    

**Other:**

- LLDP, LACP    
- QoS and buffer management for bursty workloads    

---

# **6. How It Works (Step-by-Step per Variant)**

## **A. Basic Spine–Leaf (L3 Underlay Only)**

1. Server sends a frame to the Leaf switch.    
2. If destination is on another leaf:    
    - Leaf routes packet to one of the spines via ECMP.        
3. Spine forwards packet to destination leaf.    
4. Leaf delivers it to the target server.    
5. All paths have **equal cost**, so traffic distribution is automatic and efficient.    

---

## **B. VXLAN/EVPN Fabric (Modern Standard)**

1. Leaf switch encapsulates Ethernet frame into VXLAN (L2-in-L3).    
2. VXLAN packet routed through spines using IP underlay.    
3. Destination leaf decapsulates VXLAN → forwards L2 frame to server.    
4. EVPN (BGP) synchronizes MAC/IP tables across leaves.    
5. Any leaf can act as **default gateway** (anycast gateway).    
6. Mobility supported: VMs can move between racks without changing IPs.    

---

## **C. Multi-Pod Deployment**

1. Each POD = independent spine–leaf fabric.    
2. Inter-POD links route traffic between PODs.    
3. EVPN allows consistent MAC/IP learning across pods.    
4. Ideal for large-scale cloud deployments.    

---

# **7. Best Practices & No-Goes**

### **Best Practices**

- Use **Layer 3 connections only** between spines and leaves.    
- Standardize link speeds and configurations.    
- Use **ECMP everywhere** for predictable load balancing.    
- Implement **VXLAN/EVPN** to scale L2 networks across the fabric.    
- Keep all leaves symmetrical (same number of uplinks).    
- Use **BFD** for sub-second failure detection.    
- Keep control-plane simple (BGP preferred for underlay + overlay).    

### **No-Goes**

- Never connect leaf-to-leaf directly.    
- Never connect spine-to-spine.    
- Do not use STP across fabric — all routing-based.    
- Avoid unequal leaf uplinks → breaks predictable fabric performance.    
- Do not stretch VLANs across fabric **without** VXLAN/EVPN (old L2 methods fail at scale).    

---

# **8. Importance**

- The **modern standard** for data center networks.    
- Provides massive horizontal scalability (east–west traffic).    
- Allows rapid workload mobility → essential for virtualization and cloud.    
- Replaces traditional three-tier in data centers.    
- Supported by all major vendors: Cisco (ACI), Arista, Juniper, Nokia, Dell.    
- Optimized for automation, programmability, and SDN architectures.    

---

# **9. Pros & Cons (Technical Reasoning)**

### **Pros**

- **Massive scalability** — add more leaves or spines easily.    
- Predictable, low-latency switching between any two racks.    
- High availability — multiple parallel paths.    
- Eliminates STP → pure L3 fabric.    
- Ideal for virtualized workloads requiring mobility.    
- Works extremely well with SDN and automation.    

### **Cons**

- More complex than two-tier or three-tier.    
- Requires routing knowledge (OSPF/BGP) at every switch.    
- Hardware costs can be higher due to many uplinks.    
- VXLAN/EVPN requires specialized knowledge and capable hardware.    
- Not ideal for small networks — overkill for simple enterprise LANs.    

---

# **10. TL;DR**

Spine–Leaf = **the data center topology**.  
Every leaf connects to every spine; no loops, no STP, pure L3 fabric.  
Scalable, predictable, redundant, and perfect for cloud + virtualization.  
Uses VXLAN/EVPN overlays for multi-tenant networks.

---

# **11. Sources**

- Cisco Validated Designs: Spine–Leaf Architectures    
- Cisco ACI / VXLAN / EVPN Technical Documentation    
- Cisco CCNA 200-301 Official Cert Guide (Topology architectures overview)    
- IETF RFC 7348 (VXLAN)    
- BGP EVPN Standards (RFC 7432, vendor implementations)    
- CCNA Exam Blueprint v1.1 (Network Fundamentals 1.2.c)


# **WAN ARCHITECTURES (≈ 1980s → Today — Enterprise & Service Provider Connectivity)**

**Article 10 / CCNA 1.2.d — According to your new schema**

This section covers the fundamental WAN topology architectures used globally: **Point-to-Point**, **Hub-and-Spoke**, **Full Mesh**, **Partial Mesh**, **MPLS**, **Metro Ethernet**, and **SD-WAN**.  
These are required knowledge areas for CCNA and appear in modern enterprise deployments.

---

# **1. Title + Date**

**Wide Area Network (WAN) Topology Architectures — Evolution of Long Distance Networking (≈ 1980s–Present)**

WAN topologies evolved from leased lines and Frame Relay to MPLS and modern SD-WAN.

---

# **2. Visualization (All Major WAN Types)**

### **A. Point-to-Point (Leased Line / MetroE E-Line)**

```less
[Site A]====================[Site B]
     One logical link (L2 or L3)
```

**B. Hub-and-Spoke (Frame Relay / MPLS / DMVPN / SD-WAN)**

```less
            [HQ / Hub]
           /    |     \
      [Branch1][Branch2][Branch3]
     (spokes cannot talk directly without hub)
```

**C. Full Mesh (MPLS / SD-WAN / Enterprise VPN)**

```less
     [A]========[B]
      \ \      / /
       \ \    / /
         [C]========[D]
All sites can communicate directly.
```

**D. Partial Mesh (Most Common Real World)**

```less
     [HQ]========[B]
       \          \
        \__________[C]
Some, not all, sites interconnect directly.
```

**E. Metro Ethernet (E-Line / E-LAN / E-Tree)**

```less
E-Line:   A ------- B         (point-to-point)
E-LAN:    A === B === C === D (full mesh Ethernet)
E-Tree:   Hub --- Branches    (rooted multipoint)
```

**F. SD-WAN Overlay**

```less
      Internet/MPLS/5G (underlay)
   ----------- Fabric -----------
   |   Branch   |   HQ   | Cloud|
   -------------------------------
      SD-WAN builds virtual mesh
```

# **3. Describe the Architecture in Detail (Bullet Points)**

- WAN = interconnects **geographically distant locations**.    
- Uses service provider technologies (MPLS, MetroE, circuits, fiber, broadband).    
- Logical topology often **different** from physical network.    
- Reliability, latency, and bandwidth vary significantly across sites.    
- Supports routing protocols (BGP, OSPF, EIGRP), overlays (DMVPN, SD-WAN), tunnels (IPsec).    
- Historically built on serial links → today dominated by MetroE, MPLS, SD-WAN.    

WAN topologies solve issues that LAN designs cannot:

- Long-distance transmission    
- Carrier-managed infrastructure    
- Multi-site routing    
- VPN encryption    
- Redundancy across providers    

---

# **4. Variants (Common + Specific Differences)**

## **Common Characteristics Across All WAN Types**

- All WAN topologies interconnect **remote networks**.    
- Depend on service providers (telecom, ISP, carrier Ethernet, cloud).    
- Routing is essential (static or dynamic).    
- Often use **tunnels** to create virtual topologies (DMVPN, GRE, SD-WAN).    
- WAN links are more expensive and lower bandwidth than LAN.    

---

## **Variant A: Point-to-Point**

- Physically or logically one link between two routers.    
- Simple / predictable routing.    
- High cost for multiple-site environments.    

## **Variant B: Hub-and-Spoke**

- All branches communicate through a central hub.    
- Centralizes security and routing.    
- Efficient for star-shaped topologies (Frame Relay, DMVPN, SD-WAN).    

## **Variant C: Full Mesh**

- Every site connects to every other site.    
- Highest redundancy and lowest latency.    
- Very expensive without virtualization overlays.    

## **Variant D: Partial Mesh**

- Practical version of full mesh.    
- Key sites get redundant links; others get only hub links.    

## **Variant E: MPLS VPN**

- Carrier-managed L3 VPN service.    
- Allows any-to-any or hub-and-spoke routing.    
- No tunnels required; provider routes traffic.    

## **Variant F: Metro Ethernet**

- E-Line = point-to-point    
- E-LAN = multipoint full mesh    
- E-Tree = hub-and-spoke    
- Ethernet handoff simplifies enterprise LAN/WAN integration.    

## **Variant G: SD-WAN**

- Overlay fabric built on top of any underlay (MPLS/Internet/5G).    
- Centralized control, app-aware routing, encrypted tunnels, virtual mesh.    
- Replaces traditional MPLS in many networks.    

---

# **5. Involved Devices, Media, and Protocols**

### **Devices**

- WAN routers    
- SD-WAN appliances    
- Firewalls (for VPN termination)    
- Edge switches / MetroE NIDs    
- Carrier PE routers (provider side)    
- LTE/5G gateways    

### **Media**

- Fiber    
- MPLS circuits    
- Copper (legacy)    
- Broadband, coax, DSL    
- LTE/5G wireless WAN    

### **Protocols**

**Routing:**

- BGP    
- OSPF    
- EIGRP    
- Static routes    

**Tunneling / Overlays:**

- GRE    
- IPsec    
- DMVPN (mGRE + NHRP)    
- SD-WAN virtual fabric (DTLS/IPsec tunnels)    
- VXLAN for MetroE-like services    

**Ethernet WAN Standards:**

- MEF (E-Line, E-LAN, E-Tree)    
- 802.1Q VLAN tagging    

---

# **6. How It Works (Step-by-Step per Variant)**

## **A. Point-to-Point**

1. Router A sends packet to Router B via serial, Ethernet, or MetroE circuit.    
2. Routing table has a single next-hop → simple routing.    
3. Link behaves like a private virtual cable.    

---

## **B. Hub-and-Spoke**

1. Each branch forms WAN adjacency only with the hub.    
2. Branch-to-branch traffic flows: Branch → Hub → Branch.    
3. Hub handles routing, security, NAT, QoS.    
4. Scales well to hundreds of branches (Frame Relay, DMVPN, SD-WAN).    

---

## **C. Full Mesh**

1. Each router peers with all other routers.    
2. Direct communication reduces latency.    
3. Routing complexity increases (many adjacencies).    
4. Usually implemented with MPLS or SD-WAN overlays.    

---

## **D. MPLS WAN**

1. Enterprise router forms BGP or static routing with provider PE router.    
2. Provider handles all routing inside MPLS cloud.    
3. Sites can be any-to-any or hub-and-spoke.    
4. Traffic stays isolated per customer VRF.    

---

## **E. Metro Ethernet (Carrier Ethernet)**

1. Provider delivers Ethernet handoff.    
2. E-Line: layer 2 point-to-point (virtual cable).    
3. E-LAN: multi-site broadcast domain.    
4. E-Tree: HQ connects to branch sites only.    

---

## **F. SD-WAN**

1. Creates encrypted tunnels over any transport (Internet, MPLS, 5G).    
2. Central controller programs policies.    
3. Edge devices automatically form virtual mesh between sites.    
4. Performs application-aware routing and load balancing.    

---

# **7. Best Practices & No-Goes**

### **Best Practices**

- Use **redundant links** (dual ISP, dual MPLS).    
- Use **dynamic routing protocols** where possible.    
- For hub-and-spoke: deploy **DMVPN Phase 3** or **SD-WAN** for on-demand spoke-to-spoke.    
- Encrypt WAN traffic (IPsec/SD-WAN).    
- Use SLA monitoring for path selection.    
- Use fiber or high-bandwidth links at hub/core locations.    

### **No-Goes**

- Never stretch VLANs over WAN unless using specialized technologies.    
- Avoid single WAN router at important sites.    
- Avoid static-only WAN routing in large designs.    
- Don’t rely on Internet-only without redundancy.    
- Avoid full mesh at scale **without** SD-WAN/MPLS overlays.    

---

# **8. Importance**

WAN topologies are critical because they:
- Connect branch offices, data centers, clouds, and remote users.    
- Influence routing design, redundancy, latency, and performance.    
- Are heavily tested on CCNA (topology recognition, routing effects, failure behavior).    
- Form the backbone of nearly all enterprise networks today.    

Understanding WAN architectures is essential for:

- Troubleshooting routing loops, asymmetric paths, and latency issues.    
- Designing scalable multi-site networks.    
- Working with service providers and cloud networks.    

---

# **9. Pros & Cons (Technical Reasoning)**

### **Point-to-Point**

- Simple, high-performance    
- Secure, predictable  
    – Not scalable for many sites  
    – Expensive    

### **Hub-and-Spoke**

- Low cost    
- Centralized security  
    – Hub is single point of failure  
    – High latency between branches    

### **Full Mesh**

- Optimal performance    
- Best redundancy  
    – High cost without virtual overlays  
    – Many routing adjacencies    

### **MPLS**

- Carrier-managed    
- Any-to-any routing possible    
- High quality, SLAs  
    – Expensive  
    – Vendor lock-in    

### **Metro Ethernet**

- Simple Ethernet handoff    
- Flexible topology (E-Line/E-LAN/E-Tree)  
    – Expensive at long distances  
    – Not always available    

### **SD-WAN**

- Topology-independent    
- App-aware routing    
- Uses cheap Internet links    
- Virtual full-mesh  
    – Requires new devices + controller  
    – More complex than traditional WAN    

---

# **10. TL;DR**

WAN topologies define how sites connect across long distances:

- **P2P** = simple link    
- **Hub-and-Spoke** = central hub    
- **Full/Partial Mesh** = high redundancy    
- **MPLS** = carrier-managed VRF network    
- **MetroE** = Ethernet-based WAN    
- **SD-WAN** = modern overlay mesh with centralized control    

Modern enterprises increasingly combine MPLS + Internet + 5G using **SD-WAN**.

---

# **11. Sources**

- Cisco CCNA 200-301 Official Cert Guide (WAN topologies)    
- Cisco Design Zone — WAN & SD-WAN Architectures    
- MEF Carrier Ethernet Standards (E-Line/E-LAN/E-Tree)    
- Cisco DMVPN, MPLS, and SD-WAN technical documents    
- CCNA Exam Blueprint v1.1 (Network Fundamentals 1.2.d)



# **SOHO ARCHITECTURE (≈ 2000s → Today — Small Office / Home Office Networking)**

**Article 11 / CCNA 1.2.e — According to your new schema**

---

# **1. Title + Date**

**SOHO Network Architecture — Integrated Small Office/Home Office Design (≈ 2000–Present)**

SOHO networks became widespread with broadband Internet, Wi-Fi, VoIP, and affordable integrated routers.  
This topology is standardized in CCNA as the simplest possible network architecture.

---

# **2. Visualization (All SOHO Types)**

### **A. Basic SOHO Router All-in-One**

```less
              [SOHO Router]
        (Switch + AP + Firewall + NAT)
             /     |        \
          Wired   Wi-Fi    WAN
         Devices Devices   (ISP)
```

**B. SOHO + Wireless Mesh**

```less
        [Main Router/AP]
            /       \
     [Mesh Node]   [Mesh Node]
        /  \          /  \
     Clients        Clients
```

**C. SOHO + Modem + Separate Router**

```less
 [ISP Modem] --- [SOHO Router] --- Switch/Access Points --- Devices
```

**D. SOHO with VLAN Segmentation**

```less
             [SOHO Router]
            /      |       \
    VLAN10-LAN  VLAN20-IoT  VLAN30-Guest
```

# **3. Description in Detail (Bullet Points)**

- Designed for **small environments** (1–20 users).    
- Combines multiple network functions into a **single device**:    
    - Router        
    - Firewall        
    - DHCP/NAT        
    - Wireless Access Point        
    - Switch (4–8 ports)        
- Provides **LAN, WLAN, and WAN** connectivity.    
- Easy setup, minimal configuration required.    
- Primarily uses **NAT/PAT** for IP address sharing.    
- Wi-Fi often the main access medium.    
- Increasingly includes mesh systems for coverage.    
- Supports simple VLAN segmentation on higher-end SOHO devices.    
- Based on star topology with optional wireless mesh extensions.    

---

# **4. Variants (Common → Specific Differences)**

## **Common Characteristics Across All SOHO Designs**

- One integrated router that performs NAT.    
- LAN devices use private IPv4 ranges (RFC 1918).    
- WAN side receives IP from ISP (DHCP or PPPoE).    
- Wi-Fi as primary connectivity for mobile devices.    
- Simple routing: mostly default route → ISP.    

---

## **Variant A: All-in-One SOHO Router (Most Common)**

- One device for everything (router, firewall, AP, switch).    
- Easy to install → ideal for homes and micro-offices.    
- Examples: AVM Fritz!Box, TP-Link Archer, ASUS, Netgear.    

## **Variant B: SOHO with Separate Modem and Router**

- ISP modem in bridge mode.    
- Dedicated router handles NAT & firewall.    
- Better performance and features.    

## **Variant C: SOHO with Wireless Mesh**

- Multiple APs form a mesh or star-mesh hybrid.    
- Used to increase Wi-Fi coverage.    
- Examples: Google WiFi, D-Link Covr, ASUS AiMesh, TP-Link Deco.    

## **Variant D: Advanced SOHO with VLANs**

- Segments network into multiple logical networks:    
    - IoT devices isolated        
    - Guests on separate SSID/VLAN        
    - Work-from-home office VLAN        
- Often found in professional remote-worker setups.    

---

# **5. Involved Devices, Media, and Protocols**

### **Devices**

- SOHO router (AP + switch + firewall)    
- ISP modem (coax/DSL/fiber)    
- Wireless extenders or mesh nodes    
- Small unmanaged or smart switches    
- End devices: PCs, laptops, IoT, printers, smart TVs, cameras    

### **Media**

- Ethernet (Cat5e–Cat6)    
- Wi-Fi 4/5/6/6E/7    
- DSL, cable, fiber WAN connections    
- Powerline adapters (optional)    

### **Protocols**

**LAN/WLAN:**

- Ethernet (802.3)    
- Wi-Fi (802.11 a/b/g/n/ac/ax)    
- WPA2/WPA3 security    
- DHCP (LAN side)    
- NAT/PAT    

**WAN:**

- PPPoE    
- DHCP    
- DOCSIS (cable)    
- GPON/EPON (fiber)    

**Optional:**

- VLANs (802.1Q)    
- IPv6 (SLAAC/DHCPv6)    
- VPN passthrough or built-in VPN endpoints    
- QoS for VoIP/video    

---

# **6. How It Works (Step-by-Step per Variant)**

## **A. All-in-One SOHO Router**

1. ISP assigns WAN IP via DHCP/PPPoE.    
2. SOHO router activates NAT/PAT for private LAN.    
3. DHCP on LAN gives private IPv4 addresses.    
4. Wi-Fi AP creates SSID(s) for clients.    
5. Firewall rules block unsolicited inbound WAN traffic.    
6. Traffic from LAN → NAT → WAN.    
7. Optional: guest Wi-Fi uses separate network.    

---

## **B. SOHO with Separate Modem + Router**

1. ISP modem bridges raw connection.    
2. Router handles PPPoE or DHCP WAN side.    
3. Router performs NAT/firewall.    
4. LAN & Wi-Fi work same as above but with improved performance.    

---

## **C. Wireless Mesh SOHO**

1. Main router is root node.    
2. Mesh nodes join wirelessly or wired backhaul.    
3. Nodes auto-route traffic across mesh.    
4. Clients roam between nodes using 802.11 standards.    

---

## **D. Advanced SOHO with VLANs**

1. SOHO router or smart switch defines VLANs.    
2. Separate SSIDs mapped to specific VLANs.    
    - Example: Guest → VLAN30        
    - IoT → VLAN20        
3. Router enforces firewall policies between VLANs.    
4. Adds security and isolation to small networks.    

---

# **7. Best Practices & No-Goes**

### **Best Practices**

- Use strong Wi-Fi security: **WPA3**, disable WPS.    
- Separate IoT devices into isolated VLANs/SSIDs.    
- Always update router firmware.    
- Use wired backhaul for mesh nodes when possible.    
- Change default admin credentials.    
- Enable DoS protection and intrusion detection if supported.    
- Use UPS for power stability.    

### **No-Goes**

- Never expose router management to WAN.    
- Avoid mixing too many smart/IoT devices on one network without segmentation.    
- Don’t rely on cheap extenders — use real mesh/ethernet backhaul instead.    
- Avoid disabling firewall/NAT unless you understand consequences.    
- Don’t operate on 2.4 GHz only if heavy Wi-Fi usage (congestion).    

---

# **8. Importance**

SOHO is important because:
- It’s the **simplest topology**, foundational for learning enterprise architecture.    
- Demonstrates core networking concepts: DHCP, NAT, routing, wireless, VLANs.    
- Millions of remote workers depend on secure SOHO setups.    
- Many small businesses run “micro-enterprises” on SOHO gear.    
- Understanding SOHO prepares for troubleshooting home and branch offices.    
- SOHO is often the _entry point_ for new technicians and CCNA candidates.    

---

# **9. Pros & Cons (Technical Reasoning)**

### **Pros**

- **Cheap and easy** to deploy.    
- Single device performs multiple roles.    
- Good Wi-Fi coverage with modern mesh systems.    
- Low maintenance; ideal for non-IT users.    
- Supports most modern features (NAT, DHCP, VPN).    

### **Cons**

- Limited scalability (not for >20–50 devices).    
- Performance bottlenecks (CPU, Wi-Fi).    
- Simple firewalls → fewer enterprise features.    
- Reliability depends on a single device.    
- Not ideal for professional environments with high SLA requirements.    

---

# **10. TL;DR**

SOHO networks = **one integrated router doing Wi-Fi, switching, NAT, DHCP, and firewalling**.  
Simple, cheap, and perfect for homes/small offices.  
Can be extended with mesh or VLANs but has limited scalability.

---

# **11. Sources**

- Cisco CCNA 200-301 Official Cert Guide (SOHO architecture concepts)    
- Cisco Small Business Design Guides    
- IEEE 802.11 (Wi-Fi) and 802.3 (Ethernet) documentation    
- CCNA Exam Blueprint v1.1 (Network Fundamentals 1.2.e)    
- Cisco Meraki SOHO Best Practices




# **ON-PREMISES & CLOUD ARCHITECTURES (≈ 2010s → Today — Hybrid IT & Modern Enterprise)**

**Article 12 / CCNA 1.2.f — According to your new schema**

---

# **1. Title + Date**

**On-Premises and Cloud Network Architectures — Hybrid IT Infrastructure (≈ 2010–Present)**

As enterprises migrated from traditional private datacenters to hosted and cloud services, network architecture evolved into a hybrid model where **local infrastructure (on-prem)** integrates tightly with **cloud platforms (public, private, hybrid)**.

---

# **2. Visualization (All Types)**

### **A. On-Premises Only (Traditional Model)**

```less
 Users ── Access ── Distribution ── Core ── Datacenter
                   (All local, owned, controlled)
```

**B. Cloud-Only Architecture (SaaS / Cloud-native)**

```less
Users ── Internet ── Cloud Provider (SaaS/IaaS)
```

**C. Hybrid On-Prem + Cloud**

```less
              ┌────────── Cloud (AWS/Azure/GCP) ────────────┐
Users ─ Access ─ Distr ─ Core ─ Firewall/VPN ─── Hybrid WAN ─┤
               └──────────────── On-Prem Datacenter ─────────┘
```

**D. Multi-Cloud Architecture**

```less
                [AWS]
                  ||
Users ── LAN/WAN ==== SD-WAN ==== [Azure] ==== [GCP]
                  ||                 ||
                On-Prem DC     SaaS/IaaS/PaaS
```

# **3. Describe the Architecture in Detail (Bullet Points)**

### **On-Premises Architecture**

- All computing resources stored in company-owned buildings.    
- Uses **two-tier or three-tier LAN architecture**.    
- WAN edge connects branch offices and remote users via VPN.    
- Enterprise firewalls and ACLs control all inbound/outbound flows.    

---

### **Cloud Architecture**

- Workloads run in **public cloud** (AWS, Azure, GCP) or private cloud.    
- Uses virtual network constructs: VPCs/VNETs, subnets, security groups.    
- Network topologies often use **hub-and-spoke**, **flat L3**, or **virtual routers**.    
- Traffic reaches cloud over Internet, VPN, or dedicated circuits like:    
    - AWS Direct Connect        
    - Azure ExpressRoute        
    - Google Cloud Interconnect        

---

### **Hybrid Architecture**

- Combines **on-prem datacenter + cloud environment** seamlessly.    
- Uses encryption tunnels and routing integration.    
- Enables workload mobility: on-prem ↔ cloud.    
- Uses SD-WAN, BGP, VPN, VXLAN, firewalls to interconnect environments.    

---

### **Multi-Cloud Architecture**

- Enterprise operates across **multiple cloud providers**.    
- Traffic is routed between clouds via SD-WAN overlays or cloud gateways.    
- Prevents vendor lock-in and increases high availability.    

---

# **4. Variants (Commons → Specific Differences)**

## **Common Characteristics Across All On-Prem and Cloud Designs**

- All require **identity**, **routing**, **security**, and **WAN connectivity**.    
- VPN/IPsec commonly used to bridge networks.    
- BGP often used for dynamic routing between clouds and on-prem.    
- Cloud networking resembles traditional L3 routing more than L2 LANs.    
- Firewalls and access policies exist locally and in cloud security groups/ACLs.    

---

## **Variant A: On-Prem Only**

- Classic enterprise design with physical infrastructure.    
- Highest control, highest cost.    
- Typically uses three-tier or spine-leaf for datacenters.    

## **Variant B: Cloud-Only**

- No datacenter hardware.    
- All services consumed from SaaS/IaaS providers.    
- Minimal on-prem footprint (just Internet access).    

## **Variant C: Hybrid Cloud**

- Most common today.    
- On-prem and cloud networks integrated.    
- Supports cloud bursting, DR, microservices, containers, VM migration.    

## **Variant D: Multi-Cloud**

- Uses multiple cloud vendors for redundancy or cost optimization.    
- Requires advanced routing and SD-WAN solutions.    

---

# **5. Involved Devices, Media, and Protocols**

### **Devices (On-Prem)**

- L2/L3 switches    
- Firewalls & VPN devices    
- Servers, hypervisors (VMware/Hyper-V/KVM)    
- WAN routers, load balancers    
- Storage appliances    

### **Devices (Cloud)**

- Virtual routers (AWS TGW, Azure VWAN Hub)    
- Virtual firewalls (Cisco, Palo Alto, Fortinet)    
- Gateways (VPN/IPSec/SD-WAN)    
- Cloud load balancers    
- VPC/VNet components    

---

### **Media (Connections)**

- Fiber/Ethernet for on-prem    
- Broadband/5G/MPLS/MetroE for WAN    
- VPN tunnels (IPsec, GRE)    
- Dedicated circuits (ExpressRoute/Direct Connect/Interconnect)    

---

### **Protocols**

#### **On-Prem LAN**

- VLANs (802.1Q)    
- RSTP/MSTP    
- OSPF/EIGRP    
- HSRP/VRRP/GLBP    

#### **WAN / Hybrid**

- BGP (main protocol for on-prem ↔ cloud routing)    
- IPsec VPNs    
- SD-WAN overlays    
- NAT & Firewall rules    
- DNS for service resolution    
- DHCP/DHCPv6    
- IPv6 transition technologies    

#### **Cloud Networking**

- VPC/VNet routing tables    
- Security groups & NACLs    
- VXLAN/EVPN overlays (private cloud)    
- API-driven provisioning    

---

# **6. How It Works (Step-by-Step per Variant)**

## **A. On-Prem Architecture**

1. Traffic flows from Access → Distribution → Core.    
2. Core forwards to datacenter or WAN edge.    
3. Routing decisions made locally on enterprise routers.    
4. Firewall enforces security.    

---

## **B. Cloud-Only Architecture**

1. User traffic exits LAN → Internet.    
2. Reaches SaaS/IaaS endpoint.    
3. Cloud provider handles internal routing and load balancing.    
4. Security enforced using cloud-native policies.    

---

## **C. Hybrid Cloud Operation**

1. On-prem router creates **IPsec or Direct Connect/ExpressRoute** link to cloud.    
2. BGP advertises internal on-prem prefixes to the cloud.    
3. Cloud advertises VPC/VNet subnets to on-prem.    
4. Both sides communicate as if part of same enterprise network.    
5. SD-WAN may optimize routing and failover.    

---

## **D. Multi-Cloud Operation**

1. Enterprise SD-WAN controller builds virtual fabric across multiple clouds.    
2. Traffic uses encrypted tunnels or cloud backbone connections.    
3. Routing between clouds via BGP, static routes, or cloud-native hubs.    
4. Applications can run in any cloud while keeping consistent access.    

---

# **7. Best Practices & No-Goes**

### **Best Practices**

- Never extend Layer 2 across Internet unless using proper encapsulation (VXLAN/EVPN).    
- Use BGP for scalable on-prem ↔ cloud routing.    
- Use MFA and zero-trust security models.    
- Segment cloud networks using VPC/VNet subnets.    
- Use separate networks for:    
    - Production        
    - Development        
    - DMZ        
    - Management        
- Enable logging and monitoring on both on-prem and cloud.    

### **No-Goes**

- Do NOT rely on a single VPN tunnel for critical workloads.    
- Never expose cloud workloads directly to the Internet without security groups or firewalls.    
- Avoid mixing IP addressing schemes between on-prem and cloud.    
- Do NOT use overlapping RFC1918 networks.    
- Don’t assume cloud networks behave like LANs — they are routed networks.    

---

# **8. Importance**

On-prem + cloud architecture is crucial for CCNA-level engineers because:
- Almost every modern enterprise uses hybrid infrastructure.    
- Routing between cloud and on-prem is a common real-life task.    
- Understanding cloud networks helps troubleshoot VPNs, SD-WAN, and application performance issues.    
- It connects LAN, WAN, data center, and cloud design principles.    
- Cloud architecture affects security, cost, redundancy, and scaling models.    

---

# **9. Pros & Cons (Technical Reasoning)**

### **On-Prem Only**

- Full control    
- High security  
    – High CAPEX + OPEX  
    – Hard to scale globally    

### **Cloud-Only**

- Fast deployment    
- Minimal hardware    
- Global reach  
    – Less control  
    – Operational dependency on provider    

### **Hybrid Cloud**

- Best of both worlds    
- Flexible scaling    
- Ability to modernize gradually  
    – Routing/security complexity  
    – Requires expertise in multiple platforms    

### **Multi-Cloud**

- Avoids vendor lock-in    
- High resilience  
    – Operationally complex  
    – Consistent policy enforcement is hard    

---

# **10. TL;DR**

On-premises networks = classic, controlled, hardware-based.  
Cloud networks = virtual, flexible, scalable.  
Hybrid = **industry standard**, combining both with VPN/BGP/SD-WAN.  
Modern enterprises integrate LAN, WAN, cloud, and datacenter into **one logical network**.

---

# **11. Sources**

- Cisco CCNA 200-301 Official Cert Guide (Cloud and on-prem architecture overview)    
- Cisco Enterprise Network Architecture documentation    
- Cisco SD-WAN and Hybrid Cloud Integration guides    
- AWS, Azure, Google Cloud network architecture whitepapers    
- CCNA Exam Blueprint v1.1 (Network Fundamentals 1.2.f)






