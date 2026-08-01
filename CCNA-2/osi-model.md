# 🌐 The OSI Reference Model

> **Block 0 · Models & Framing** — the map every later topic is pinned to.

---

## 1️⃣ Base Data

- **Topic:** OSI (Open Systems Interconnection) 7-layer reference model + encapsulation.
- **Document date:** 2026-08-01 · **Topic era:** ISO drafts ~1977 → first published **1984** → current edition **ISO/IEC 7498-1:1994**.
- **Exam scope:** CCNA 200-301 v1.1 → underpins *1.1 components, 1.5 TCP/UDP, 1.13 switching concepts*, and every "at which layer…?" question. Returns and deepens at CCNP.

**Marker legend (protocols only):**
🟢 CCNA · 🔵 CCNP ENCOR · 🟣 CCNP ENARSI · 🟠 CCNP ENSLD/ENSDWI · ⚪ context-only. Two dots = introduced then deepened.

---

## 2️⃣ Visualization 👁️

### 🗺️ The Hub — Layers × Devices × Protocols

Every device and protocol links to its own future doc (`./devices/…`, `./protocols/…`). This table is the index of the whole study system — the big picture at a glance.

| 🧩 Layer | 🖧 Devices that operate here | 📡 Protocols that live here |
|---|---|---|
| **7 · Application** 📨 | [NGFW](./devices/next-generation-firewall.md) · [WAF](./devices/web-application-firewall.md) · [Proxy](./devices/proxy-server.md) · [L7 Load Balancer](./devices/load-balancer.md) | [HTTP](./protocols/http.md) 🟢 · [HTTPS](./protocols/https.md) 🟢 · [DNS](./protocols/dns.md) 🟢 · [DHCP](./protocols/dhcp.md) 🟢 · [FTP](./protocols/ftp.md) 🟢 · [TFTP](./protocols/tftp.md) 🟢 · [SSH](./protocols/ssh.md) 🟢 · [Telnet](./protocols/telnet.md) 🟢 · [SNMP](./protocols/snmp.md) 🟢 · [NTP](./protocols/ntp.md) 🟢 · [Syslog](./protocols/syslog.md) 🟢 · [RADIUS](./protocols/radius.md) 🟢 · [TACACS+](./protocols/tacacs-plus.md) 🟢 · [REST/RESTCONF](./protocols/restconf.md) 🟢🔵 · [NETCONF](./protocols/netconf.md) 🔵 · [SMTP](./protocols/smtp.md) ⚪ · [POP3/IMAP](./protocols/imap.md) ⚪ |
| **6 · Presentation** 🔐 | *(no dedicated box — [NGFW](./devices/next-generation-firewall.md) does TLS proxy here)* | [TLS/SSL](./protocols/tls.md) 🟢🔵 ¹ · [JPEG](./protocols/jpeg.md) ⚪ · [MPEG](./protocols/mpeg.md) ⚪ · [ASCII/Unicode](./protocols/character-encoding.md) ⚪ · [MIME](./protocols/mime.md) ⚪ |
| **5 · Session** 🤝 | *(no dedicated box — session state lives in hosts)* | [RPC](./protocols/rpc.md) ⚪ · [NetBIOS](./protocols/netbios.md) ⚪ · [SIP](./protocols/sip.md) ⚪ · [L2TP](./protocols/l2tp.md) ⚪ · [PPTP](./protocols/pptp.md) ⚪ |
| **4 · Transport** 🚚 | [L4 Load Balancer](./devices/load-balancer.md) · [Stateful Firewall](./devices/stateful-firewall.md) *(L3–L4)* | [TCP](./protocols/tcp.md) 🟢 · [UDP](./protocols/udp.md) 🟢 · [QUIC](./protocols/quic.md) ⚪ ² |
| **3 · Network** 🧭 | [Router](./devices/router.md) · [Layer 3 / Multilayer Switch](./devices/layer-3-switch.md) · [Packet-filter Firewall](./devices/packet-filter-firewall.md) | [IPv4](./protocols/ipv4.md) 🟢 · [IPv6](./protocols/ipv6.md) 🟢 · [ICMP](./protocols/icmp.md) 🟢 · [ICMPv6](./protocols/icmpv6.md) 🟢 · [OSPF](./protocols/ospf.md) 🟢🔵🟣 · [EIGRP](./protocols/eigrp.md) 🔵🟣 · [BGP](./protocols/bgp.md) 🔵🟣 · [IS-IS](./protocols/is-is.md) 🟠 · [RIP](./protocols/rip.md) ⚪ · [IGMP](./protocols/igmp.md) 🔵 · [IPsec (ESP/AH)](./protocols/ipsec.md) 🟢🔵 · [GRE](./protocols/gre.md) 🔵🟣 · [HSRP](./protocols/hsrp.md) 🟢🔵 · [VRRP](./protocols/vrrp.md) 🟢🔵 · [GLBP](./protocols/glbp.md) 🔵 · [NAT](./protocols/nat.md) 🟢 |
| **2 · Data Link** 🔗 | [Switch](./devices/switch.md) · [Bridge](./devices/bridge.md) · [NIC](./devices/nic.md) · [Wireless AP](./devices/wireless-access-point.md) · [WLC](./devices/wireless-lan-controller.md) *(partly)* | [Ethernet 802.3](./protocols/ethernet.md) 🟢 · [Wi-Fi 802.11](./protocols/wifi-802-11.md) 🟢 · [802.1Q](./protocols/dot1q.md) 🟢 · [STP](./protocols/stp.md) 🟢 · [RSTP](./protocols/rstp.md) 🟢 · [LACP](./protocols/lacp.md) 🟢 · [PAgP](./protocols/pagp.md) 🟢 · [CDP](./protocols/cdp.md) 🟢 · [LLDP](./protocols/lldp.md) 🟢 · [ARP](./protocols/arp.md) 🟢 ³ · [VTP](./protocols/vtp.md) 🟢 · [DTP](./protocols/dtp.md) 🟢 · [MACsec 802.1AE](./protocols/macsec.md) 🔵 · [PPP](./protocols/ppp.md) ⚪ · [HDLC](./protocols/hdlc.md) ⚪ |
| **1 · Physical** ⚡ | [Hub](./devices/hub.md) · [Repeater](./devices/repeater.md) · [Media Converter](./devices/media-converter.md) · [Transceiver/SFP](./devices/transceiver-sfp.md) · [Modem](./devices/modem.md) · cabling & connectors | [Ethernet PHY (10/100/1000/10GBASE)](./protocols/ethernet-phy.md) 🟢 · [RJ45](./protocols/rj45.md) 🟢 · [Single-mode Fiber](./protocols/single-mode-fiber.md) 🟢 · [Multimode Fiber](./protocols/multimode-fiber.md) 🟢 · [SFP/SFP+](./protocols/sfp.md) 🟢 · [RF / 802.11 PHY](./protocols/rf-radio.md) 🟢 · [DWDM](./protocols/dwdm.md) 🟠 · [RS-232](./protocols/rs-232.md) ⚪ · [Coaxial](./protocols/coaxial.md) ⚪ |

> **Contested placements** (the honest nuance): ¹ **TLS** — classic L6 home but runs over TCP; many call it L5/"L4.5" → [`tls.md`](./protocols/tls.md). ² **QUIC** — L4 work delivered *inside* [UDP](./protocols/udp.md). ³ **ARP** — usually **L2** (uses MAC frames) but resolves L3→L2, so texts argue "L2.5" → [`arp.md`](./protocols/arp.md).

### 🧅 Encapsulation — data grows a shell at every layer (top → bottom)

```
 L7-5   │                    [   DATA   ]                    │  PDU: DATA
        │            ↓ add L4 header (ports, seq)            │
 L4     │        [ TCP/UDP hdr │   DATA   ]                  │  PDU: SEGMENT (TCP) / DATAGRAM (UDP)
        │            ↓ add L3 header (src/dst IP)            │
 L3     │    [ IP hdr │ TCP/UDP hdr │   DATA   ]             │  PDU: PACKET
        │            ↓ add L2 header + trailer (MAC, FCS)    │
 L2     │ [ Eth hdr │ IP hdr │ TCP/UDP hdr │ DATA │ FCS ]    │  PDU: FRAME
        │            ↓ encode as signal for the medium       │
 L1     │ 101011010101…copper volts / fiber light / RF…0101 │  PDU: BITS / SYMBOLS
```

**Memory hook (top-down):** `All People Seem To Need Data Processing` → **A**pp · **P**resentation · **S**ession · **T**ransport · **N**etwork · **D**ata Link · **P**hysical.

### 🪜 OSI (reference) vs TCP/IP (what actually runs)

```
   OSI (7 layers)                 TCP/IP 5-layer          TCP/IP 4-layer (RFC 1122)
 ┌────────────────┐            ┌────────────────┐        ┌────────────────────────┐
 │ 7 Application  │ ┐          │                │        │                        │
 │ 6 Presentation │ ├────────► │  Application   │ ─────► │      Application       │
 │ 5 Session      │ ┘          │                │        │                        │
 ├────────────────┤            ├────────────────┤        ├────────────────────────┤
 │ 4 Transport    │ ─────────► │  Transport     │ ─────► │      Transport         │
 ├────────────────┤            ├────────────────┤        ├────────────────────────┤
 │ 3 Network      │ ─────────► │  Network       │ ─────► │  Internet              │
 ├────────────────┤            ├────────────────┤        ├────────────────────────┤
 │ 2 Data Link    │ ┐          │  Data Link     │ ┐      │                        │
 │ 1 Physical     │ ├────────► │  Physical      │ ├────► │  Network Access (Link) │
 └────────────────┘ ┘          └────────────────┘ ┘      └────────────────────────┘
```

---

## 3️⃣ TL;DR ⚡

> The **OSI model** is a **7-layer reference map** (🔝 Application → Presentation → Session → Transport → Network → Data Link → Physical 🔻) for how systems communicate. Going **down**, each layer **adds its own header** (L2 also adds a trailer) — Data → **Segment** → **Packet** → **Frame** → **Bits** = **encapsulation**; going **up**, each layer reads and strips its own header. A **device operates at the highest layer whose header it reads**: hubs = L1, switches = L2 (MAC), routers = L3 (IP), NGFWs = up to L7. That one idea explains the payoffs — a **switch** opens one header and rewrites nothing (fast); a **router** opens two and rebuilds the frame every hop (more work); an **NGFW** beats a legacy firewall simply by *reading more layers*. OSI is the **teaching ruler**; **TCP/IP** is the stack that actually runs. Learn the common per-layer placement — and know which protocols ([TLS](./protocols/tls.md), [ARP](./protocols/arp.md), [QUIC](./protocols/quic.md)) refuse to sit still. 🧠

---

## 4️⃣ Importance 🎯

**🔧 The problem it was invented to kill.** In the 1970s every vendor shipped its own proprietary, incompatible networking stack (IBM **SNA**, DEC **DECnet**, Xerox **XNS**, …). Gear from one vendor often simply *could not talk* to another's, and — just as bad — there was **no shared language** to describe *where* in the communication process something worked or failed. Engineers needed two things: (a) a **vendor-neutral blueprint** so interoperable systems could be built, and (b) a **common mental grid** to reason about and troubleshoot them. ISO's answer, in 1984, was the OSI model. It won as the *model* even though its own protocol suite lost to TCP/IP — because the **grid** turned out to be the valuable part.

Once you see that problem, the value is obvious:

- 🧪 **Exam:** underpins CCNA 1.1 (components by layer), 1.5 (TCP vs UDP), 1.13 (switching), and every "at which layer does X operate?" item across the blueprint — then returns at CCNP.
- 🛠️ **Real work:** it *is* the troubleshooting method. Outages get solved by isolating the failing layer, fast.
- 🗣️ **Communication:** the profession's shared vocabulary — design reviews, tickets, vendor calls, interviews.
- 🏗️ **Design reasoning:** knowing what each layer *costs* (open more headers = more work) lets you reason about latency, security depth, and where a function belongs — the "decide and justify it yourself" level.

---

## 5️⃣ Description 📝

- 📏 A **7-layer conceptual framework** that standardises how any two systems communicate, independent of vendor or medium.
- 🧱 Built on **layered abstraction**: each layer does one job, consumes the service of the layer below, and offers a service to the layer above — never needing to know *how* the layer below works, only *that* it works.
- 🪞 **Peer layers talk logically.** L3 on host A "talks to" L3 on host B — but physically the data went all the way down A's stack, across the wire, and back up B's. The peer conversation is an illusion built on encapsulation.
- 🎁 **Encapsulation / de-encapsulation** is the mechanism: descending = add your header (L2 also a trailer); ascending = read and strip your header, hand the rest up.
- 🏷️ Each layer's data unit is a **PDU**: Data → Segment/Datagram → Packet → Frame → Bits.
- 🧰 It is **not software you install** — it's a reference model. The stack that actually runs on your machine is **TCP/IP**.

---

## 6️⃣ How It Works ⚙️ — a packet's life, top to bottom (and back up)

Scenario: you type a URL and hit **Enter**. Follow the data down host A's stack, across the wire, up host B's. Headers shown bit-by-bit where it matters.

### ⬇️ Encapsulation on the sender

**L7-5 · produce the DATA.** The browser forms an HTTP request; if HTTPS, [TLS](./protocols/tls.md) encrypts it and the session is tracked. Output = application data, *no network headers yet*.

**L4 · wrap into a SEGMENT.** The kernel prepends a [TCP](./protocols/tcp.md) header — **ports** identify the app, **sequence/ACK** give reliability:

```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|          Source Port          |       Destination Port        |   ← e.g. 49152 → 443
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                        Sequence Number                        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Acknowledgment Number                      |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|Offset |Rsrvd|C|E|U|A|P|R|S|F|            Window               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|           Checksum            |         Urgent Pointer        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```
> Flags C E U A P R S F = **CWR ECE URG ACK PSH RST SYN FIN**. The handshake rides these: `SYN → SYN/ACK → ACK`. Min header **20 bytes**. If it were [UDP](./protocols/udp.md): just **8 bytes** (Src Port, Dst Port, Length, Checksum) — no seq/ACK/flags. That leanness is *why* UDP is faster (DNS/DHCP/VoIP/NTP). That's CCNA 1.5 in one picture.

**L3 · wrap into a PACKET.** The kernel prepends an [IPv4](./protocols/ipv4.md) header carrying **source/destination IP** — the addresses that survive end-to-end:

```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|Version|  IHL  |   DSCP    |ECN|         Total Length          |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|         Identification        |Flags|     Fragment Offset     |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|      TTL      |    Protocol   |        Header Checksum        |   ← Protocol: 6=TCP 17=UDP 1=ICMP
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                       Source IP Address                       |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Destination IP Address                     |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```
> **TTL** is decremented by every router (loop protection). **Protocol** tells the receiver which L4 to hand up to. Min header **20 bytes**. IPv6 fixes this at a flat **40 bytes** → [`ipv6.md`](./protocols/ipv6.md).

**L2 · wrap into a FRAME.** The host needs the next hop's **MAC** (its gateway), resolved by [ARP](./protocols/arp.md). It adds an [Ethernet](./protocols/ethernet.md) header *and a trailer* (the only layer that adds a trailer):

```
+----------+-----+----------+----------+-----------+------------------+-------+
| Preamble | SFD | Dest MAC | Src MAC  | EtherType |     Payload      |  FCS  |
|  7 bytes | 1 B |  6 bytes |  6 bytes |   2 bytes | 46 – 1500 bytes  |  4 B  |
+----------+-----+----------+----------+-----------+------------------+-------+
                 └──────────── FRAME (DA … FCS) = 64 – 1518 bytes ───────────┘
   EtherType:  0x0800 = IPv4   0x86DD = IPv6   0x0806 = ARP   0x8100 = 802.1Q tag
```
> **FCS** = CRC-32; the receiver recomputes it and silently drops a corrupt frame — that's your "CRC / input errors" counter on `show interfaces`. An [802.1Q](./protocols/dot1q.md) VLAN tag inserts 4 bytes after Src MAC (max frame → 1522). Preamble + SFD are clocking and sit outside the frame proper.

**L1 · turn the FRAME into SIGNALS.** The PHY serialises the bits and encodes them for the medium: **voltage** on [copper](./protocols/rj45.md), **light** on [fiber](./protocols/single-mode-fiber.md), **radio symbols** on [RF](./protocols/rf-radio.md). Nothing here understands MACs or IPs.

### ⬆️ De-encapsulation on the receiver

Mirror image: L1 recovers bits → L2 checks FCS + dest MAC, strips the frame → L3 checks dest IP, reads *Protocol* → L4 checks dest port, reassembles the stream → L7 hands clean data to the app. **Each layer reads only its own header, strips it, passes the rest up.**

### ⚡ Payoff — why switching is faster than routing

| Step | 🔁 Switch (L2) | 🧭 Router (L3) |
|---|---|---|
| Read | Destination **MAC** only | Strip L2, read **dest IP**, longest-prefix-match lookup |
| Rewrite? | **No** — frame forwarded unchanged | **Yes** — decrement TTL, recompute IP checksum, build a **brand-new L2 frame** (new src/dst MAC) |
| Lookup | Exact-match MAC table (CAM) | Longest-prefix-match (TCAM/FIB) — heavier |

A switch touches **one layer** and rewrites **nothing**; a router touches **two** and *rewrites the frame every hop*. More layers + more rewriting = more latency. That's why L2 switching runs at line rate in ASICs, and why [multilayer switches](./devices/layer-3-switch.md) exist: push the L3 decision into the same ASIC so "routing" runs at switching speed. You now understand the trade-off from first principles. ✅

---

## 7️⃣ Pros & Cons ⚖️

**OSI model — pros ✅**
- Vendor-neutral common language and a clean troubleshooting grid.
- Strict layering is superb for *teaching* abstraction and isolating faults.
- Granular (7 layers) — separates presentation/session concerns TCP/IP lumps together.

**OSI model — cons ⚠️**
- **Never implemented as-is** — a reference, not a running stack; the OSI *protocol suite* lost to TCP/IP.
- L5/L6 are fuzzy in practice; real protocols refuse to sit in one clean box.
- Its tidy layering can mislead you about messier reality (cross-layer protocols, hardware offload, TLS-in-the-middle).

> 🕵️ **Dirty secret — why TCP/IP won:** it was *running code* on the early Internet while OSI was still a committee document. Rough consensus and working implementations beat an elegant-but-late standard. OSI survived as the *model*; TCP/IP became the *stack*.

---

## 8️⃣ Variants 🔀 (common → specific)

- 🧱 **OSI 7-layer (ISO/IEC 7498-1)** — the reference/teaching model. Exhaustive, vendor-neutral, rarely implemented literally.
- 🌍 **TCP/IP 4-layer (DoD / RFC 1122)** — the original Internet model: Application, Transport, Internet, Link. The *authoritative* Internet architecture.
- 🎓 **TCP/IP 5-layer (hybrid)** — the teaching compromise that splits "Link" back into Data Link + Physical to line up with OSI L1/L2. What most CCNA courses draw.
- 🗂️ **Layer groupings:** **Upper/Host = 5-7** (software on the endpoint) · **Lower/Media = 1-4** (where the network *moves* the data). *(Some texts split 4–7 / 1–3 — grouping is convenience, not standard.)*

---

## 9️⃣ Devices · Media · Protocols 🖧 (the "where does it live" rule)

Rule of thumb: **a device operates at the highest layer whose header it reads and acts on.** That one sentence explains most of networking.

- ⚡ **L1:** [hubs](./devices/hub.md), [repeaters](./devices/repeater.md), [media converters](./devices/media-converter.md), [transceivers/SFPs](./devices/transceiver-sfp.md), cabling — move *symbols*, not addresses. A hub repeats bits out every port, blind to MACs.
- 🔗 **L2:** [switches](./devices/switch.md), [bridges](./devices/bridge.md), [NICs](./devices/nic.md), [APs](./devices/wireless-access-point.md) — read the **frame header** (destination MAC) and forward via the MAC table. Home of MAC learning/aging, frame switching, flooding (CCNA 1.13).
- 🧭 **L3:** [routers](./devices/router.md) and [multilayer/L3 switches](./devices/layer-3-switch.md) — strip L2, read the **IP header**, longest-prefix-match, then re-frame for the next hop.
- 🚚–📨 **L4-L7:** [stateful firewalls](./devices/stateful-firewall.md) track the L3/L4 5-tuple; [L4 load balancers](./devices/load-balancer.md) balance on ports; [NGFWs](./devices/next-generation-firewall.md), [WAFs](./devices/web-application-firewall.md), [proxies](./devices/proxy-server.md) climb to L7 to see actual application data.

> 🔎 **Payoff — why an NGFW beats a legacy firewall.** A *legacy* stateful firewall reads only to **L3/L4**: allow/deny by IP + port + connection state. It sees "TCP to 443" and nothing more. An **NGFW climbs to L7**: identifies the *actual app* (App-ID), inspects the payload (IPS/DPI), can decrypt & re-inspect [TLS](./protocols/tls.md), and blocks malware *inside* an otherwise-allowed port-443 flow. Same packet — the NGFW just *reads more layers*. Full detail → [`next-generation-firewall.md`](./devices/next-generation-firewall.md).

---

## 🔟 Best Practices 🌟

- 🩺 **Troubleshoot by layer, on purpose:**
  - **Bottom-up** — start at L1 (plugged in? link light? duplex?) and climb. Best for suspected physical faults.
  - **Top-down** — start at the app and descend. Best for "the site is down but ping works."
  - **Divide-and-conquer** — start at L3 (`ping`, then `traceroute`); works → look up, fails → look down. The pro default.
- 🧭 **Speak in layers:** "that's an L2 loop," "L3 reachability is fine, it's an L4 firewall drop" — the shared language of every NOC and interview.
- 🔢 **Map each tool to a layer:** `show interface` counters → L1/L2 · `show mac address-table` → L2 · `ping`/`traceroute` → L3 · socket/port state → L4 · `curl`/dev tools → L7.

---

## 1️⃣1️⃣ No-Goes ❌ (mistakes + troubleshooting traps)

- 🚫 **"OSI is the software on my PC."** No — your PC runs **TCP/IP**. OSI is the ruler you measure it with.
- 🚫 **"Every protocol sits in exactly one layer."** Reality is messy: [TLS](./protocols/tls.md) (L5/L6), [ARP](./protocols/arp.md) (L2/L3), [QUIC](./protocols/quic.md) (L4-in-L4). Memorise the *common* placement, know the argument.
- 🚫 **"OSI and TCP/IP map one-to-one."** TCP/IP folds OSI 5/6/7 into one Application layer and 1/2 into one Link layer.
- 🚫 **"A switch is L3 because it has an IP."** Its management IP manages the box; its *forwarding* is L2. Forwarding layer ≠ management layer.
- 🚫 **Wrong PDU name.** L4 **segment**/datagram, L3 **packet**, L2 **frame**. Using the wrong word is an instant tell.
- 🩺 **Troubleshooting trap:** a symptom felt at L7 ("website down") often lives lower — DNS (L7) failing *looks* like "no internet," a duplex mismatch (L1/L2) *looks* like random slowness. Always confirm the layer before you fix it.

---

## 1️⃣2️⃣ Terms 📖

Short definitions of every term used above. Internal link = its own future doc (`./terms/…`); external = authoritative source. *(External URLs pending verification when each term doc is built.)*

| Term | One-line meaning | Links |
|---|---|---|
| **Encapsulation** | Wrapping upper-layer data in this layer's header (+ L2 trailer) as it descends. | [internal](./terms/encapsulation.md) · [ext: Wikipedia](https://en.wikipedia.org/wiki/Encapsulation_(networking)) |
| **De-encapsulation** | The reverse: each layer reads and strips its own header on the way up. | [internal](./terms/de-encapsulation.md) |
| **PDU (Protocol Data Unit)** | The named data unit at a layer: Data/Segment/Packet/Frame/Bits. | [internal](./terms/pdu.md) · [ext: Wikipedia](https://en.wikipedia.org/wiki/Protocol_data_unit) |
| **Header** | Control info prepended by a layer (addresses, ports, flags). | [internal](./terms/header.md) |
| **Trailer** | Control info *appended* — only L2 does this (the FCS). | [internal](./terms/trailer.md) |
| **Payload** | The data a layer is carrying for the layer above. | [internal](./terms/payload.md) |
| **Segment / Datagram** | L4 PDU — segment (TCP) / datagram (UDP). | [internal](./terms/segment.md) |
| **Packet** | L3 PDU — IP header + payload. | [internal](./terms/packet.md) |
| **Frame** | L2 PDU — MAC header + payload + FCS trailer. | [internal](./terms/frame.md) · [ext: Wikipedia](https://en.wikipedia.org/wiki/Ethernet_frame) |
| **MAC address** | 48-bit hardware address identifying a NIC on an L2 segment. | [internal](./terms/mac-address.md) · [ext: Wikipedia](https://en.wikipedia.org/wiki/MAC_address) |
| **FCS (Frame Check Sequence)** | CRC-32 trailer used to detect frame corruption. | [internal](./terms/fcs.md) · [ext: Wikipedia](https://en.wikipedia.org/wiki/Frame_check_sequence) |
| **TTL (Time To Live)** | IP-header hop counter, decremented per router; hits 0 → dropped. | [internal](./terms/ttl.md) · [ext: RFC 791](https://www.rfc-editor.org/rfc/rfc791) |
| **MTU** | Largest payload a layer will carry in one PDU (Ethernet default 1500 B). | [internal](./terms/mtu.md) · [ext: Wikipedia](https://en.wikipedia.org/wiki/Maximum_transmission_unit) |
| **EtherType** | 2-byte field naming the L3 protocol inside a frame (0x0800 = IPv4). | [internal](./terms/ethertype.md) · [ext: Wikipedia](https://en.wikipedia.org/wiki/EtherType) |
| **Port number** | 16-bit L4 identifier for an application/service (e.g. 443). | [internal](./terms/port-number.md) |
| **3-way handshake** | TCP session setup: SYN → SYN/ACK → ACK. | [internal](./terms/three-way-handshake.md) · [ext: RFC 9293](https://www.rfc-editor.org/rfc/rfc9293) |
| **Longest-prefix match** | Route-selection rule: most specific matching prefix wins. | [internal](./terms/longest-prefix-match.md) |
| **ASIC** | Purpose-built forwarding chip enabling line-rate switching. | [internal](./terms/asic.md) |
| **Reference model** | An abstract framework (like OSI) — describes, doesn't implement. | [internal](./terms/reference-model.md) |

---

## 1️⃣3️⃣ Practical Tasks 🛠️

Brief list now; each gets a detailed, in-context walkthrough later.

- 🦈 **See encapsulation live** — capture your own traffic in **Wireshark**, pick one packet, expand the tree, and watch the Ethernet → IP → TCP → HTTP nesting in the flesh.
- 🧭 **Watch TTL work** — `ping` then `traceroute` a host; observe TTL decrementing hop by hop (L3 loop protection you can *see*).
- 🔗 **Inspect L2↔L3 resolution** — run `arp -a` on your PC and read the live IP-to-MAC table.
- 🔁 **MAC learning** — in Packet Tracer 8 / GNS3, `show mac address-table` on a switch, generate traffic, watch entries appear and age out.
- ⚖️ **TCP vs UDP by eye** — compare a TCP handshake against a UDP DNS query in Wireshark; contrast the fat header + flags vs. the lean 8-byte one.
- 🎯 **Isolate handshakes** — Wireshark filter `tcp.flags.syn==1` to pull just the SYNs.
- ⚡ **L1/L2 counters** — bounce an interface and read `show interfaces` (CRC/errors, duplex, up/down) to feel where L1 ends and L2 begins.

---

## 1️⃣4️⃣ Sources 📚

*Primary standards & RFCs (authoritative — verify header bit-fields here):*
- **ISO/IEC 7498-1:1994** — OSI Basic Reference Model.
- **RFC 1122** — Requirements for Internet Hosts (TCP/IP 4-layer architecture).
- **RFC 791** — IPv4 · **RFC 8200** — IPv6 · **RFC 9293** — TCP (obsoletes 793) · **RFC 768** — UDP.
- **RFC 792 / 4443** — ICMP / ICMPv6 · **RFC 826** — ARP.
- **IEEE 802.3** — Ethernet (frame, FCS, PHY) · **IEEE 802.1Q** — VLAN tagging · **IEEE 802.11** — Wireless LAN.

*Vendor & learning references:*
- **Cisco** — Networking Basics (OSI & TCP/IP models); CCNA 200-301 Official Cert Guide (switching concepts, TCP/UDP).
- **Cisco Learning Network** — CCNA 200-301 v1.1 exam blueprint (scope tagging).

> 🔗 **Leaf docs to build next** (most-referenced nodes): [`tcp.md`](./protocols/tcp.md), [`udp.md`](./protocols/udp.md), [`ipv4.md`](./protocols/ipv4.md), [`ethernet.md`](./protocols/ethernet.md), [`arp.md`](./protocols/arp.md), [`switch.md`](./devices/switch.md), [`router.md`](./devices/router.md).
