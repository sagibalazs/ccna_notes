# 📖 Master Table of Contents

### *"Networking, Zero → Engineer" — the atomic curriculum*

> **Date:** 2026-08-01 · **Primary target:** CCNA 200-301 v1.1 → **bridge:** CCNP Enterprise.
> This is the book's spine. Every entry below is **one self-contained document** built on the [14-section template](./00-how-we-learn-the-method.md). Read top to bottom and nothing ever depends on an untaught topic.

---

## 🧭 How this book is organised

Two axes, like a real technical book:

- **📚 The linear read (Blocks 0–10)** — the *chapters*. An OSI climb from the model up to automation. This is the study order.
- **🗂️ The reference library (Appendix R)** — the *glossary + component encyclopedia*. Shared leaf docs under `devices/`, `protocols/`, `terms/`, each linked from wherever it's mentioned. Built on demand, not read linearly.

**Sizing law:** one concept per document, sized for a *single study session* — read, understand, memorise, done. **When in doubt, split.** A doc that needs scrolling to see its own diagram is already too big.

**Status keys:** ✅ done · 🔨 in progress · ⬜ planned. **Scope markers** as defined in the [method](./00-how-we-learn-the-method.md): 🟢 CCNA · 🔵🟣🟠 CCNP · ⚪ context.

**Blueprint tags** (e.g. `[1.13]`) point to the exact CCNA 200-301 v1.1 exam section a doc satisfies, so coverage is provable and gap-free.

---

## 🗂️ Front Matter

| # | Document | Purpose | Status |
|---|---|---|---|
| 00 | [How We Learn — The Method](./00-how-we-learn-the-method.md) | The framework: 14-section template, conventions, markers, curriculum | ✅ |
| 0C | This Table of Contents | The book's spine | ✅ |

---

## 📗 Block 0 · Models, Framing & Reference — *the map*

> *Bigger than it looks:* the OSI model isn't one topic — it's the coordinate system for the entire book. Every later block plugs into a layer defined here. Each layer gets its own doc so the concept is airtight before any real technology lands on it.

| # | Document | Covers | Status |
|---|---|---|---|
| 0.1 | [OSI Reference Model](./osi-model.md) | the 7-layer map, encapsulation, hub table | ✅ |
| 0.2 | OSI Layer 7 — Application (role & concepts) | `[1.1]` what L7 *is*, services, gateways | ⬜ |
| 0.3 | OSI Layer 6 — Presentation (encoding, encryption, compression) | representation concepts | ⬜ |
| 0.4 | OSI Layer 5 — Session (dialog control, sockets) | session concepts | ⬜ |
| 0.5 | OSI Layer 4 — Transport (segmentation, ports, reliability) | `[1.5]` role of L4 | ⬜ |
| 0.6 | OSI Layer 3 — Network (logical addressing, path selection) | role of L3 | ⬜ |
| 0.7 | OSI Layer 2 — Data Link (framing, MAC/LLC sublayers, error detect) | `[1.13]` role of L2 | ⬜ |
| 0.8 | OSI Layer 1 — Physical (bits → signals, encoding, media) | role of L1 | ⬜ |
| 0.9 | The TCP/IP Model | `[1.1]` the stack that actually runs | ⬜ |
| 0.10 | OSI vs TCP/IP — side by side | mapping, why TCP/IP won | ⬜ |
| 0.11 | PDUs & Encapsulation — deep dive (per layer) | Data→Segment→Packet→Frame→Bits, bit-level | ⬜ |

---

## 📘 Block 1 · Physical Layer & Network Components (L1)

> *Bigger than it looks:* "cabling" hides copper categories, fiber modes, transceivers, connectors, duplex/speed negotiation, and the whole family of L1 devices — plus the topology architectures the exam loves.

| # | Document | Covers | Status |
|---|---|---|---|
| 1.1 | Network Components & Their Roles | `[1.1.a–g]` routers, switches, FW/IPS, AP, WLC, endpoints, servers → device leaves | ⬜ |
| 1.2 | Power over Ethernet (PoE) | `[1.1.h]` | ⬜ |
| 1.3 | Topology Architectures — 2-tier, 3-tier, spine-leaf | `[1.2.a–c]` | ⬜ |
| 1.4 | Topologies — WAN, SOHO, on-prem vs cloud | `[1.2.d–f]` | ⬜ |
| 1.5 | Copper Media — UTP/STP, categories, pinouts | `[1.3.a]` | ⬜ |
| 1.6 | Fiber Media — SMF vs MMF, transceivers/SFPs | `[1.3.a]` | ⬜ |
| 1.7 | Connection Types — shared media vs point-to-point | `[1.3.b]` | ⬜ |
| 1.8 | Interface & Cable Issues — collisions, errors, duplex/speed mismatch | `[1.4]` | ⬜ |
| 1.9 | Signalling & Encoding — how bits become voltage/light/RF | L1 mechanics | ⬜ |

---

## 📙 Block 2 · Ethernet & Switching (L2)

> *Bigger than it looks:* the heaviest CCNA block. It owns the switching guts of two 20% blueprint domains — frames, VLANs, trunking, discovery, aggregation, and the entire Spanning Tree family.

| # | Document | Covers | Status |
|---|---|---|---|
| 2.1 | Ethernet Fundamentals (802.3, MAC addressing) | frame basics | ⬜ |
| 2.2 | Ethernet Frame Anatomy — bit level | frame dissection | ⬜ |
| 2.3 | CSMA/CD, collisions & the road to full-duplex | evolution/why | ⬜ |
| 2.4 | Switch Operation — MAC learning, aging, flooding, frame switching | `[1.13.a–c]` | ⬜ |
| 2.5 | The MAC Address Table | `[1.13.d]` | ⬜ |
| 2.6 | VLANs — concept & configuration | `[2.1]` | ⬜ |
| 2.7 | Access Ports — data & voice | `[2.1.a–b]` | ⬜ |
| 2.8 | Trunking & 802.1Q Tagging | `[2.2.a–b]` | ⬜ |
| 2.9 | Native VLAN | `[2.2.c]` | ⬜ |
| 2.10 | Inter-VLAN Routing — router-on-a-stick & SVI | `[2.1.c]` | ⬜ |
| 2.11 | DTP — Dynamic Trunking Protocol | trunk negotiation | ⬜ |
| 2.12 | VTP — VLAN Trunking Protocol | VLAN sync | ⬜ |
| 2.13 | Discovery — CDP | `[2.3]` | ⬜ |
| 2.14 | Discovery — LLDP | `[2.3]` | ⬜ |
| 2.15 | EtherChannel — LACP & PAgP | `[2.4]` | ⬜ |
| 2.16 | STP — the loop problem & how STP solves it | `[2.5]` | ⬜ |
| 2.17 | STP — roles, states, root bridge election | `[2.5.a–b]` | ⬜ |
| 2.18 | Rapid PVST+ (RSTP) | `[2.5]` | ⬜ |
| 2.19 | STP Toolkit — PortFast, BPDU Guard/Filter, Root/Loop Guard | `[2.5.c–d]` | ⬜ |

---

## 📕 Block 3 · IPv4 & IPv6 Addressing (L3 addressing)

> *Bigger than it looks:* subnetting alone is a multi-session skill, and IPv6 is a parallel address world with its own types, autoconfig, and neighbour discovery replacing ARP.

| # | Document | Covers | Status |
|---|---|---|---|
| 3.1 | IPv4 Addressing Fundamentals — structure, binary, classes (history) | `[1.6]` | ⬜ |
| 3.2 | Subnetting — the concept & the "why" | `[1.6]` | ⬜ |
| 3.3 | Subnetting — host/subnet math & VLSM (practice-heavy) | `[1.6]` | ⬜ |
| 3.4 | CIDR & Summarisation basics | `[1.6]` | ⬜ |
| 3.5 | Private IPv4 (RFC 1918) | `[1.7]` | ⬜ |
| 3.6 | Special IPv4 Ranges — loopback, APIPA, multicast, broadcast | reference | ⬜ |
| 3.7 | IPv6 — why, structure, hex, prefix | `[1.8]` | ⬜ |
| 3.8 | IPv6 Types — global unicast, ULA, link-local | `[1.9.a]` | ⬜ |
| 3.9 | IPv6 — anycast & multicast | `[1.9.b–c]` | ⬜ |
| 3.10 | IPv6 — EUI-64 & SLAAC | `[1.9.d]` | ⬜ |
| 3.11 | IPv6 — Neighbor Discovery (NDP) replaces ARP | context | ⬜ |
| 3.12 | Verifying IP Parameters — Windows / macOS / Linux | `[1.10]` | ⬜ |

---

## 📗 Block 4 · Routing (L3 forwarding)

> *Bigger than it looks:* the 25% blueprint domain. Routing table anatomy, forwarding-decision logic, static routing in v4 *and* v6, single-area OSPF end-to-end, and first-hop redundancy.

| # | Document | Covers | Status |
|---|---|---|---|
| 4.1 | Routing Fundamentals — routed vs routing protocols | overview | ⬜ |
| 4.2 | The Routing Table — anatomy | `[3.1.a–g]` code, prefix, mask, next hop, AD, metric, GoLR | ⬜ |
| 4.3 | The Forwarding Decision — longest-prefix, AD, metric | `[3.2.a–c]` | ⬜ |
| 4.4 | Static Routing — IPv4 | `[3.3]` | ⬜ |
| 4.5 | Static Routing — IPv6 | `[3.3]` | ⬜ |
| 4.6 | Route Types — default, network, host, floating static | `[3.3.a–d]` | ⬜ |
| 4.7 | Dynamic Routing Overview — IGP/EGP, distance-vector vs link-state | concepts | ⬜ |
| 4.8 | OSPF Fundamentals — concepts, LSAs, single area | `[3.4]` | ⬜ |
| 4.9 | OSPF — adjacencies & network types (P2P, broadcast, DR/BDR) | `[3.4.a–c]` | ⬜ |
| 4.10 | OSPF — Router ID, config & verification | `[3.4.d]` | ⬜ |
| 4.11 | First Hop Redundancy Protocols — HSRP / VRRP / GLBP | `[3.5]` | ⬜ |

---

## 📘 Block 5 · Transport (L4)

> *Bigger than it looks:* one blueprint line (`1.5`) but the beating heart of reliability. TCP's handshake, windowing and teardown deserve their own deep dive — this is where "why is it slow?" gets answered.

| # | Document | Covers | Status |
|---|---|---|---|
| 5.1 | Transport Layer Role — ports, sockets, multiplexing | overview | ⬜ |
| 5.2 | TCP — deep dive (handshake, seq/ack, windowing, flags, teardown) | `[1.5]` | ⬜ |
| 5.3 | UDP — deep dive (connectionless, when & why) | `[1.5]` | ⬜ |
| 5.4 | TCP vs UDP — comparison | `[1.5]` | ⬜ |
| 5.5 | Common Ports & Services — reference | reference | ⬜ |

---

## 📙 Block 6 · IP Services

> *Bigger than it looks:* "10%" that touches everything you actually run — name resolution, addressing, translation, time, logging, monitoring, QoS and file transfer, each a real operational skill.

| # | Document | Covers | Status |
|---|---|---|---|
| 6.1 | DNS — name resolution | `[4.3]` | ⬜ |
| 6.2 | DHCP — DORA & client | `[4.3, 4.6]` | ⬜ |
| 6.3 | DHCP Relay | `[4.6]` | ⬜ |
| 6.4 | NAT / PAT — static, pool, overload | `[4.1]` | ⬜ |
| 6.5 | NTP — client/server, stratum | `[4.2]` | ⬜ |
| 6.6 | SNMP — v2c/v3, manager/agent/MIB | `[4.4]` | ⬜ |
| 6.7 | Syslog — facilities & severity levels | `[4.5]` | ⬜ |
| 6.8 | QoS — classification, marking, queuing, policing, shaping (PHB) | `[4.7]` | ⬜ |
| 6.9 | SSH — secure remote access | `[4.8]` | ⬜ |
| 6.10 | FTP & TFTP | `[4.9]` | ⬜ |

---

## 📕 Block 7 · Virtualization & Cloud

> *Bigger than it looks:* the exam asks little, but the concepts (hypervisors, containers, VRFs, cloud models) are the gateway to everything modern — and to the CCNP.

| # | Document | Covers | Status |
|---|---|---|---|
| 7.1 | Server Virtualization — hypervisors (type 1/2), VMs | `[1.12]` | ⬜ |
| 7.2 | Containers | `[1.12]` | ⬜ |
| 7.3 | VRFs | `[1.12]` | ⬜ |
| 7.4 | Cloud Fundamentals — service/deployment models, on-prem vs cloud | `[1.2.f]` | ⬜ |

---

## 📗 Block 8 · Wireless

> *Bigger than it looks:* RF physics, 802.11 operation, Cisco's controller architectures, AP modes, the WLC/AP plumbing, *and* the wireless-security stack — pulled together from three separate blueprint domains.

| # | Document | Covers | Status |
|---|---|---|---|
| 8.1 | RF Fundamentals — spectrum, channels, non-overlapping | `[1.11.a,c]` | ⬜ |
| 8.2 | 802.11 Basics — SSID, BSS, association | `[1.11.b]` | ⬜ |
| 8.3 | Cisco Wireless Architectures — autonomous, cloud, split-MAC | `[2.6]` | ⬜ |
| 8.4 | AP Modes | `[2.6]` | ⬜ |
| 8.5 | WLC & AP Physical Connections, LAG | `[2.7]` | ⬜ |
| 8.6 | Wireless Security — WPA / WPA2 / WPA3 | `[5.9]` | ⬜ |
| 8.7 | WLAN GUI Configuration (WPA2 PSK) | `[2.9, 5.10]` | ⬜ |

---

## 📘 Block 9 · Security Fundamentals

> *Bigger than it looks:* 15% of the exam and the widest topic spread — concepts, hardening, AAA, management access, ACLs, the Layer-2 defence trio, and VPNs.

| # | Document | Covers | Status |
|---|---|---|---|
| 9.1 | Security Concepts — threats, vulnerabilities, exploits, mitigation | `[5.1]` | ⬜ |
| 9.2 | Security Program Elements — awareness, training, physical access | `[5.2]` | ⬜ |
| 9.3 | Device Access Control & Hardening — local passwords, privilege | `[5.3]` | ⬜ |
| 9.4 | Password Policy — MFA, certificates, biometrics | `[5.4]` | ⬜ |
| 9.5 | AAA — authentication/authorization/accounting, RADIUS & TACACS+ | `[5.8, 2.8]` | ⬜ |
| 9.6 | Device Management Access — Telnet/SSH/HTTP(S)/console/cloud | `[2.8]` | ⬜ |
| 9.7 | ACLs — standard & extended | `[5.6]` | ⬜ |
| 9.8 | Layer 2 Security — Port Security | `[5.7]` | ⬜ |
| 9.9 | Layer 2 Security — DHCP Snooping | `[5.7]` | ⬜ |
| 9.10 | Layer 2 Security — Dynamic ARP Inspection | `[5.7]` | ⬜ |
| 9.11 | VPNs — IPsec site-to-site & remote access (concepts) | `[5.5]` | ⬜ |

---

## 📙 Block 10 · Automation & Programmability

> *Bigger than it looks:* the future of the job. SDN architecture, APIs, data formats and config-management tools — the on-ramp to controller-based networking and the CCNP automation track.

| # | Document | Covers | Status |
|---|---|---|---|
| 10.1 | Why Automation — impact on network management | `[6.1]` | ⬜ |
| 10.2 | Traditional vs Controller-Based Networking | `[6.2]` | ⬜ |
| 10.3 | SDN Architecture — control/data plane, overlay/underlay/fabric | `[6.3, 6.3.a]` | ⬜ |
| 10.4 | Northbound & Southbound APIs | `[6.3.b]` | ⬜ |
| 10.5 | REST APIs — CRUD, HTTP verbs, auth, data encoding | `[6.5]` | ⬜ |
| 10.6 | JSON — structure & syntax | `[6.7]` | ⬜ |
| 10.7 | Config Management — Ansible & Terraform | `[6.6]` | ⬜ |
| 10.8 | AI & ML in Network Operations | `[6.4]` | ⬜ |

---

## 🗂️ Appendix R · Reference Library (cross-cutting leaves)

Built on demand and linked from every doc that mentions them. Not read linearly.

- **`devices/`** — router, switch, layer-3-switch, bridge, NIC, hub, repeater, media-converter, transceiver-sfp, modem, wireless-access-point, wireless-lan-controller, next-generation-firewall, web-application-firewall, stateful-firewall, packet-filter-firewall, proxy-server, load-balancer.
- **`protocols/`** — every protocol in the OSI hub table (HTTP, DNS, DHCP, TCP, UDP, IPv4/6, ICMP, OSPF, ARP, Ethernet, 802.1Q, STP, LACP, CDP/LLDP, …) with its scope marker and depth ladder.
- **`terms/`** — the running glossary (encapsulation, PDU, MAC, FCS, TTL, MTU, longest-prefix-match, …), each a short def + authoritative external link.

---

## 🌉 Appendix C · The CCNP Bridge — *Volume II preview*

Where this book keeps climbing after CCNA. Each becomes its own block-set later, reusing the same 14-section template and markers.

- **ENCOR 350-401** 🔵 — enterprise core: advanced OSPF/EIGRP/BGP, multicast, advanced STP, SD-WAN & SD-Access concepts, wireless, network assurance (NetFlow, SPAN, IP SLA), security, NETCONF/RESTCONF, Python/JSON/YANG.
- **ENARSI 300-410** 🟣 — advanced routing & troubleshooting: deep OSPF/EIGRP/BGP, redistribution, route-maps/PBR, VRF-Lite, DMVPN, MPLS L3VPN, infrastructure security & services.
- **ENSLD 300-420** 🟠 — enterprise *design*: structured addressing, IS-IS, SD-Access & SD-WAN design, WAN, QoS strategy, multicast design.
- **ENSDWI 300-415** 🟠 — Catalyst SD-WAN: architecture, controllers, WAN-edge, policies, security, QoS, operations.

---

## 📐 Sizing & Sourcing Notes

- **Modeled on** the CCNA 200-301 v1.1 blueprint (scope authority) and cross-checked against the standard atomic curricula: Cisco Press *Official Cert Guide* (Odom), Cisco Networking Academy, Jeremy's IT Lab, and NetworkLessons — so the decomposition matches recognised professional literature, not an ad-hoc split.
- **Every blueprint sub-item is claimed by at least one document** (the `[x.y]` tags). If an exam item has no tag anywhere, that's a gap to fix — this TOC doubles as the coverage audit.
- **~110 atomic topic docs** across Blocks 0–10, each a single study session. Small on purpose: easier to read, understand, memorise, and to teach back point-by-point (the mastery test).

---

## ✅ Immediate next candidates

Per the "one topic at a time, on your go-ahead" rule — likely next docs, in reading order:
**0.2–0.8** (the seven OSI layer docs) → **0.9** TCP/IP model → **0.10** OSI vs TCP/IP → **0.11** PDUs deep dive. That finishes Block 0, then Block 1 opens.
