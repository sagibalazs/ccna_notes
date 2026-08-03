# 1.5 · TCP vs UDP — Reliability vs Speed 🔁⚡

> **CCNA 200‑301 Domain 1.0 (Network Fundamentals) · topic 1.5** — "Compare TCP to UDP"
> **A transport‑layer (L4) comparison** — standard **14‑section study template**. The **bit‑level segment/datagram headers** were already dissected in Block 0 (`0.11.2 Segment‑Datagram PDU`); this leaf is the **comparison**: *how* each behaves, *why*, and *when to use which*.

---

## 1 · Base Data 📇

| Field | Value |
|---|---|
| **Title** | TCP vs UDP (transport‑layer comparison) |
| **Doc ID** | `1.5` |
| **Date / Era** | 2 Aug 2026 · TCP per **RFC 9293** (2022); UDP per **RFC 768** (1980); QUIC/HTTP‑3 mainstream |
| **Topic** | The two transport protocols — **TCP** (reliable, ordered, connection‑oriented) vs **UDP** (fast, connectionless) — and choosing between them |
| **Scope** | 🟢 CCNA 1.5: compare TCP/UDP, reliability mechanisms, ports, when to use each · 🔵 engineer: congestion/flow control, head‑of‑line blocking, QUIC |
| **Core trade‑off** | **Reliability & ordering (TCP)** vs **speed & low latency (UDP)** — the choice depends on the **application**, not the protocol name |
| **Layer** | OSI **L4 (Transport)** / TCP‑IP **Transport** (from Block 0 models) |
| **References** | `0.11.2` (bit‑level TCP/UDP headers) · Block 0 (OSI/TCP‑IP models, ports intro) · builds toward Block 5 (Transport) & Block 6 (IP services: DNS/DHCP/NTP) |

---

## 2 · Visualization — the two behaviours 🗺️

```
   TCP — connection-oriented, reliable            UDP — connectionless, fire-and-forget
   ┌───────────────────────────────────┐          ┌───────────────────────────────────┐
   │ 1) 3-WAY HANDSHAKE (setup, 1 RTT)  │          │  (no setup — send immediately)     │
   │    Client ──SYN──►      Server     │          │    Client ──datagram──► Server     │
   │    Client ◄─SYN-ACK─     Server     │          │    Client ──datagram──► Server     │
   │    Client ──ACK──►      Server     │          │    Client ──datagram──► Server     │
   │ 2) DATA: every byte numbered,      │          │  no ACK · no retransmit · no order │
   │    ACKed; lost → retransmit        │          │  if lost → gone (app's problem)    │
   │    in-order delivery (HOL blocking)│          │  arrives as-is, any order          │
   │    flow control + congestion ctrl  │          │  no flow/congestion control        │
   │ 3) TEARDOWN: 4-way (FIN/ACK…)      │          │  (nothing to tear down)            │
   └───────────────────────────────────┘          └───────────────────────────────────┘

   HEADER (detail in 0.11.2):                      WHEN TO USE:
   ┌──────────────┬─────────┬─────────┐            TCP → web, email, file xfer, SSH
   │ field        │ TCP     │ UDP     │            (accuracy > speed)
   │ min size     │ 20 B    │ 8 B     │            UDP → VoIP, live video, gaming,
   │ src/dst port │ ✓       │ ✓       │                   DNS, DHCP, TFTP, NTP, SNMP
   │ seq / ack #  │ ✓       │ ✗       │            (speed/timeliness > completeness)
   │ flags/window │ ✓       │ ✗       │
   │ checksum     │ ✓       │ ✓       │            "Right answer depends on the app,
   │ length       │ (hdr)   │ ✓       │             not the protocol name."
   └──────────────┴─────────┴─────────┘
```

---

## 3 · TL;DR ⚡

- Both **TCP** and **UDP** are **transport‑layer (L4)** protocols that carry application data over **IP**, and both use **ports** to deliver to the right *process* on a host. The difference is **everything TCP does on top of that** to guarantee delivery.
- **TCP (RFC 9293) = connection‑oriented + reliable + ordered.** It opens a connection with a **3‑way handshake (SYN → SYN‑ACK → ACK)**, **numbers every byte**, **ACKs** what arrives, **retransmits** what's lost, delivers **in order**, and runs **flow control** (don't overwhelm the receiver) and **congestion control** (don't swamp the network). Header **≥20 bytes**.
- **UDP (RFC 768) = connectionless + fast.** It **fires a datagram immediately** — **no handshake, no ACK, no retransmission, no ordering, no flow/congestion control**. Header is just **8 bytes** (src port, dst port, length, checksum). If a datagram is lost, **the app deals with it.**
- 🔑 **The master trade‑off: reliability vs speed.** TCP's guarantees cost a **round‑trip to set up**, **per‑packet overhead**, and **latency when loss happens** (it waits and retransmits). UDP skips all of that for **minimal overhead and immediate, low‑latency** delivery. *You buy reliability with time and overhead.*
- 🔑 **Retransmission is useless for real‑time.** A resent voice/video/game packet arrives **too late to use** — so real‑time apps *prefer* UDP's "drop it and move on." And TCP's **in‑order delivery causes head‑of‑line blocking**: one lost packet **stalls the whole stream** even though later packets already arrived. Both reasons push real‑time to UDP.
- **Use TCP** when **correctness matters more than speed**: web (HTTP/HTTPS), email, file transfer, SSH. **Use UDP** when **timeliness matters more than completeness**: VoIP, live video, gaming, DNS, DHCP, TFTP, NTP, SNMP.
- 🔵 **QUIC (RFC 9000)** rebuilds TCP's reliability **inside UDP** (over 443) to get reliability *without* head‑of‑line blocking — the basis of **HTTP/3**, blurring the old TCP/UDP line.

---

## 4 · Importance — the problem first 🎯

**The problem:** IP (L3, Block 3) gets a packet from one **host** to another **host** across the internet — but it makes **two promises it can't keep** and **leaves one job undone**:

1. **IP is unreliable.** Packets can be **lost** (a congested router drops them), **corrupted**, **duplicated**, or **reordered** (they take different paths). IP doesn't notice or fix any of it — it's "best effort."
2. **IP delivers to a host, not a process.** A packet reaches a machine's IP address, but that machine is running a browser, an email client, a video call, and a dozen background services at once. **Which one gets the data?** IP has no idea.

The **transport layer** exists to solve these — and here's the crucial design insight: **not every application wants the same solution.** So there are **two** transport protocols, each making a different bet:

- A **file download** or a **bank transaction** must be **perfect** — every byte, in order, no corruption. A missing byte ruins the file. For these, **waiting to retransmit a lost packet is completely acceptable**. → **TCP** adds all the machinery to guarantee correctness.
- A **live voice call** must be **timely** — the audio for "hello" must play *now*. If a packet is lost, **retransmitting it is pointless** (by the time it arrives, that moment of speech is long past); better to **skip it** and keep the conversation flowing. → **UDP** stays out of the way and lets the app decide.

Both need the **process‑addressing** part (ports), so both have it. But TCP adds **reliability, ordering, flow control, and congestion control** on top; UDP deliberately adds **nothing** — because for its use cases, that machinery would only add harmful **delay**.

So the transport layer's importance is that it's where the network stops being "best‑effort host‑to‑host" and becomes **application‑aware, process‑to‑process delivery with a choice**: *pay for reliability, or pay for speed.* Get the choice wrong — TCP for live video, or UDP for a bank transfer — and the application either stutters or corrupts.

🔵 **Why the depth?** Because "TCP is reliable, UDP is fast" is the exam answer, but the *engineering* answer is knowing **why retransmission helps a download and hurts a call**, **why in‑order delivery causes head‑of‑line blocking**, and **why QUIC put reliability back into UDP**. That's the difference between memorizing and designing.

---

## 5 · Description 📋

**Both protocols** 🟢 provide **process‑to‑process** delivery via **ports** (multiplexing many apps over one IP) and a **checksum** for error *detection*.

**TCP (Transmission Control Protocol)** 🟢 — **RFC 9293**:
- **Connection‑oriented** — a session is established (3‑way handshake) before data flows.
- **Reliable** — lost/corrupted data is **retransmitted** until acknowledged.
- **Ordered** — bytes delivered to the app in the order sent (reassembled).
- **Flow control** — a **sliding window** stops a fast sender from overwhelming a slow receiver.
- **Congestion control** — the sender **backs off** when the network shows loss.
- **Stateful** — each side tracks connection state; header **≥20 bytes**.

**UDP (User Datagram Protocol)** 🟢 — **RFC 768**:
- **Connectionless** — no handshake; datagrams sent immediately, independently.
- **Unreliable** — no ACK, no retransmission (loss recovery is the app's job).
- **Unordered** — datagrams may arrive in any order.
- **No flow/congestion control** — sends at whatever rate the app chooses.
- **Stateless, lightweight** — **8‑byte** header (src port, dst port, length, checksum).

**Ports** 🟢 — a 16‑bit number identifying the process. **Well‑known 0–1023**, **registered 1024–49151**, **dynamic/ephemeral 49152–65535**. A **socket** = IP + port. A TCP port and a UDP port with the **same number are different endpoints** (a port is only meaningful with its protocol).

---

## 6 · How It Works — the mechanisms ⚙️

### 6.1 TCP connection: handshake → data → teardown 🟢
```
SETUP (3-way handshake — synchronize sequence numbers, agree "connected"):
   Client ──SYN (seq=x)──────────►  Server
   Client ◄──SYN-ACK (seq=y,ack=x+1) Server
   Client ──ACK (ack=y+1)────────►  Server        → connection open (cost: 1 RTT)

DATA (reliable, ordered):
   • every byte is NUMBERED (sequence numbers)
   • receiver ACKs contiguous ranges it has received
   • if an ACK doesn't return within the retransmission timeout (RTO) → RESEND
   • receiver delivers to the app STRICTLY IN ORDER (buffers out-of-order data)

TEARDOWN (4-way): FIN → ACK → FIN → ACK   (an RST aborts abruptly)
```

### 6.2 TCP flow control vs congestion control 🔵
- **Flow control** protects the **receiver**: the receiver advertises a **window** (how much it can buffer); the sender never sends more unACKed data than the window → a slow receiver isn't overwhelmed.
- **Congestion control** protects the **network**: the sender watches for **loss/RTT increase** and **reduces its rate** (slow start → congestion avoidance), then probes back up. Deployed algorithms in 2026: **CUBIC, BBR, Reno**.
- *Two different "don't send too fast" mechanisms — one guards the receiver, one guards the network.*

### 6.3 UDP: there is (almost) nothing to explain 🟢
```
app hands UDP a message → UDP prepends an 8-byte header (ports, length, checksum) → hands to IP → done.
No handshake. No ACK. No retransmit. No ordering. No rate control.
If it's lost, UDP never knows. If two arrive reversed, they stay reversed.
```
Everything TCP does for free, a UDP app must build itself **if it needs it** (RFC 8085 exists to explain exactly that).

### 6.4 Ports & multiplexing 🟢
```
One host, one IP, many conversations:
   browser  ↔ 443 (HTTPS)     ─┐
   email    ↔ 993 (IMAPS)      ├─ distinguished by (src IP, src port, dst IP, dst port, protocol)
   DNS      ↔ 53  (UDP)        ─┘   = the 5-tuple that identifies each flow
```
Ports are how one machine runs dozens of services/connections at once without confusion.

### 6.5 ⚡ TCP↔UDP relationships (the "why" behind the choice) 🔑
> The comparison is a set of trade-offs. These are the ones that decide which protocol an app uses.

**① Reliability ↔ overhead & latency — the master trade‑off.** TCP's handshake, ACKs, retransmits, ordering, and flow/congestion control **guarantee correctness** but cost **a setup round‑trip, per‑packet overhead, and delay when loss occurs**. UDP drops all of it for **minimal overhead and immediate delivery**. *You pay for reliability with time; you pay for speed with guarantees.*

**② Connection setup ↔ latency.** TCP needs **≥1 RTT before any data** (more once TLS stacks on top); UDP **sends on the first packet**. For **short** exchanges (a DNS query) or **latency‑critical** ones (a game input), that handshake tax is decisive → UDP. *For tiny/urgent flows, setup cost dominates.*

**③ Retransmission ↔ real‑time uselessness.** TCP resends lost data — great for a file, **useless for a live stream**: by the time the resend arrives, that video frame/audio moment has **already passed**. Real‑time apps would rather **skip** the loss (a brief glitch) than **stall** for a resend → UDP. *Late data is worse than no data in real time.*

**④ In‑order delivery ↔ head‑of‑line blocking.** TCP delivers **strictly in order**, so **one lost packet stalls the entire stream** until it's retransmitted — even though later packets already arrived and wait in the buffer (**head‑of‑line blocking**). For real‑time/multiplexed traffic that's crippling → UDP avoids it, and **QUIC** was built to eliminate it. *Ordering guarantees have a stall cost.*

**⑤ Flow control ↔ receiver protection.** TCP's window keeps a **fast sender** from drowning a **slow receiver**; UDP has none, so a UDP app **can** overwhelm a receiver → the app must self‑limit. *No flow control means the application inherits the job.*

**⑥ Congestion control ↔ network fairness.** TCP **backs off** under congestion — polite, but **slower** when the network is stressed. UDP **keeps firing** regardless — faster, but it can **contribute to congestion** and is **unfair** to TCP flows sharing the link → UDP apps at scale must add their own control. *TCP is a good network citizen; UDP is only as polite as the app makes it.*

**⑦ Header size ↔ per‑packet overhead.** **20 B (TCP)** vs **8 B (UDP)** per segment — for high‑rate small‑packet traffic (voice, telemetry) the difference matters. *Smaller header = less overhead per packet.*

**⑧ State ↔ scalability.** TCP is **stateful** — each connection costs the endpoints **memory/tracking** (a server with millions of connections feels this). UDP is **stateless** — cheap to fan out to huge numbers of clients. *Statelessness scales; state costs.*

**⑨ QUIC ↔ blurring the line.** 🔵 **QUIC (RFC 9000)** runs **over UDP** but rebuilds **numbering, ACKs, retransmission, and congestion control** inside itself — getting TCP‑like reliability **without** TCP's head‑of‑line blocking, and folding in TLS for **fewer setup round‑trips**. It's the transport under **HTTP/3**. *The modern answer isn't "TCP or UDP" but "UDP plus exactly the reliability I want."*

---

## 7 · Pros & Cons ⚖️

| Aspect | **TCP** | **UDP** |
|---|---|---|
| **Delivery** | ✅ guaranteed (retransmits) | ❌ best‑effort |
| **Ordering** | ✅ in‑order | ❌ any order |
| **Setup** | ⚠️ 3‑way handshake (1 RTT) | ✅ none (immediate) |
| **Latency (steady)** | good | ✅ lowest |
| **Latency (on loss)** | ⚠️ stalls/retransmits (HOL) | ✅ unaffected (skips) |
| **Flow/congestion control** | ✅ built‑in | ❌ app's job |
| **Header overhead** | ⚠️ ≥20 B | ✅ 8 B |
| **State/scalability** | ⚠️ stateful | ✅ stateless |
| **Best for** | web, email, file, SSH | VoIP, video, gaming, DNS/DHCP/TFTP/NTP/SNMP |

---

## 8 · Variants — flavours & related 🌿

- **TCP congestion‑control algorithms** 🔵 — **Reno** (classic) → **CUBIC** (default on most OSes) → **BBR** (Google, model‑based). Same TCP, different "back‑off" behaviour.
- **TCP extensions** 🔵 — **SACK** (selective ACK, RFC 2018), **window scaling/timestamps** (RFC 7323), **fast retransmit/recovery**.
- **UDP + reliability rebuilt** 🔵 — **QUIC** (RFC 9000) / **HTTP‑3** (RFC 9114) over UDP 443; **RTP** (real‑time media) over UDP with app‑level sequencing.
- **Port ranges** 🟢 — **well‑known (0–1023)** → **registered (1024–49151)** → **dynamic/ephemeral (49152–65535)**.
- **Reliability spectrum** — raw UDP (none) → RTP (sequencing, no retransmit) → QUIC (full, no HOL) → TCP (full, in‑order).

---

## 9 · Devices / Media / Protocols 🔌

| Item | Transport | Port(s) | Notes |
|---|---|---|---|
| **HTTP / HTTPS** | TCP (HTTP/3 = UDP) | 80 / 443 | web; 443 also QUIC/UDP |
| **SSH / Telnet** | TCP | 22 / 23 | remote mgmt (Block 6/9) |
| **FTP** | TCP | 20/21 | file transfer |
| **SMTP / IMAP / POP3** | TCP | 25 / 143 / 110 | email |
| **DNS** | **UDP** (TCP for large/zone) | 53 | name resolution (Block 6) |
| **DHCP** | **UDP** | 67/68 | addressing (Block 6) |
| **TFTP** | **UDP** | 69 | simple file xfer |
| **NTP** | **UDP** | 123 | time sync (Block 6) |
| **SNMP** | **UDP** | 161/162 | monitoring (Block 6) |
| **Syslog** | **UDP** | 514 | logging (Block 6) |
| **VoIP / RTP** | **UDP** | dynamic | real‑time media |
| **Socket** | — | IP:port | endpoint identity |
| **5‑tuple** | — | src/dst IP+port+proto | identifies a flow |

---

## 10 · Best Practices ✅

- **Choose by what the app values:** correctness/completeness → **TCP**; timeliness/low latency → **UDP** (§6.5 ①③).
- **Use UDP for real‑time** (voice/video/gaming) and let the app tolerate loss — never force TCP where a resend arrives too late (§6.5 ③④).
- **Use TCP for anything that must be byte‑perfect** — files, transactions, email, remote shells (§4).
- **If you build on UDP, add what you need** (sequencing, loss handling, rate limiting) per **RFC 8085** — don't ignore congestion at scale (§6.5 ⑥).
- **Remember DNS/DHCP/TFTP/NTP/SNMP/syslog are UDP** — key infrastructure services you'll configure in Block 6 (§9).
- **Account for the handshake tax** in latency budgets for short flows; consider **QUIC/HTTP‑3** where head‑of‑line blocking hurts (§6.5 ②④⑨).
- **Match firewall/ACL rules to the right protocol** — a TCP‑80 rule doesn't cover UDP‑53 (a port is meaningless without its protocol) (§6.4; Block 9 ACLs).

---

## 11 · No‑Goes 🚫

- ❌ **Using TCP for live real‑time media.** Retransmission + in‑order delivery cause stalls (head‑of‑line blocking) — glitches become freezes (§6.5 ③④).
- ❌ **Using UDP for data that must be complete/ordered** (files, transactions) without rebuilding reliability yourself (§4).
- ❌ **Assuming "UDP loses packets."** The **network** loses packets; TCP recovers, UDP leaves recovery to the app (§6.5 ①).
- ❌ **Ignoring congestion in UDP apps at scale.** No back‑off means you can flood the link and starve TCP flows (§6.5 ⑥).
- ❌ **Treating TCP port N and UDP port N as the same thing.** They're separate endpoints; ACL/firewall rules must specify the protocol (§6.4).
- ❌ **Forgetting the handshake cost** in latency‑sensitive short exchanges (§6.5 ②).
- ❌ **Re‑dissecting headers here** — the bit‑level layout lives in `0.11.2`; this doc is the comparison.

---

## 12 · Terms 📖

- **TCP (Transmission Control Protocol)** — reliable, ordered, connection‑oriented. 🔗 [RFC 9293](https://www.rfc-editor.org/rfc/rfc9293) · `./terms/tcp.md`
- **UDP (User Datagram Protocol)** — fast, connectionless, unreliable. 🔗 [RFC 768](https://www.rfc-editor.org/rfc/rfc768) · `./terms/udp.md`
- **Three‑way handshake** — SYN / SYN‑ACK / ACK connection setup. 🔗 [RFC 9293](https://www.rfc-editor.org/rfc/rfc9293) · `./terms/three-way-handshake.md`
- **Sequence / acknowledgment numbers** — byte numbering + confirmation. 🔗 [RFC 9293](https://www.rfc-editor.org/rfc/rfc9293) · `./terms/seq-ack.md`
- **Retransmission / RTO** — resend of unACKed data. 🔗 [RFC 9293](https://www.rfc-editor.org/rfc/rfc9293) · `./terms/retransmission.md`
- **Flow control (sliding window)** — protects a slow receiver. 🔗 [RFC 9293](https://www.rfc-editor.org/rfc/rfc9293) · `./terms/flow-control.md`
- **Congestion control** — protects the network (Reno/CUBIC/BBR). 🔗 [RFC 5681](https://www.rfc-editor.org/rfc/rfc5681) · `./terms/congestion-control.md`
- **Connection teardown (FIN/RST)** — orderly/abrupt close. 🔗 [RFC 9293](https://www.rfc-editor.org/rfc/rfc9293) · `./terms/tcp-teardown.md`
- **Head‑of‑line blocking** — one lost packet stalls the ordered stream. 🔗 [ref](https://localtonet.com/blog/tcp-vs-udp) · `./terms/hol-blocking.md`
- **Port (well‑known/registered/dynamic)** — process identifier. 🔗 [IANA ports](https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml) · `./terms/ports.md`
- **Socket / 5‑tuple** — endpoint / flow identity. 🔗 [ref](https://inventivehq.com/blog/what-is-the-difference-between-tcp-and-udp-ports) · `./terms/socket-5tuple.md`
- **Multiplexing** — many app flows over one IP via ports. 🔗 [ref](https://www.rfc-editor.org/rfc/rfc768) · `./terms/multiplexing.md`
- **Checksum** — L4 error detection. 🔗 [RFC 9293](https://www.rfc-editor.org/rfc/rfc9293) · `./terms/l4-checksum.md`
- **QUIC / HTTP‑3** 🔵 — reliability over UDP, no HOL blocking. 🔗 [RFC 9000](https://www.rfc-editor.org/rfc/rfc9000) · `./terms/quic.md`
- **RFC 8085 (UDP usage guidelines)** 🔵 — what to add atop UDP. 🔗 [RFC 8085](https://www.rfc-editor.org/rfc/rfc8085) · `./terms/rfc8085.md`

---

## 13 · Practical Tasks 🧪

1. **Capture a 3‑way handshake** 🟢 — Wireshark filter `tcp.flags.syn==1`; watch SYN → SYN‑ACK → ACK open a connection; note the 1‑RTT setup before data (§6.1).
2. **See retransmission** 🔵 — Wireshark `tcp.analysis.retransmission` during a lossy transfer; correlate a lost segment with the resend and the ACK (§6.1).
3. **UDP has nothing to show** 🟢 — capture a DNS query/response (`udp.port==53`): one request, one reply, no handshake, no ACK — contrast with TCP (§6.3).
4. **Head‑of‑line blocking on paper** 🔑 — diagram a TCP stream where packet 3 of 6 is lost; show how 4–6 wait in the buffer until 3 is resent; explain why this ruins live video (§6.5 ④).
5. **Protocol‑choice drill** 🔑 — for (a) a bank transfer, (b) a Zoom call, (c) a DNS lookup, (d) a 4 GB file download, pick TCP or UDP and justify via reliability‑vs‑latency (§6.5 ①③).
6. **Ports & multiplexing** 🟢 — `netstat`/`ss` on your machine: list active sockets, identify local/remote IP:port pairs and whether each is TCP or UDP (§6.4).
7. **QUIC in the wild** 🔵 — capture an HTTPS visit to a modern site; spot **QUIC over UDP 443** vs classic TCP 443; note the reduced setup round‑trips (§6.5 ⑨).

---

## 14 · Sources 📚

- IETF — **RFC 9293** (TCP, 2022, obsoletes RFC 793), **RFC 768** (UDP, 1980), **RFC 5681** (congestion control), **RFC 2018** (SACK), **RFC 7323** (window scaling/timestamps), **RFC 8085** (UDP usage guidelines), **RFC 9000** (QUIC), **RFC 9114** (HTTP/3).
- TCP/UDP comparison references (InventiveHQ, ForaSoft, Localtonet, ITU Online, Pinggy, NetworkCheckr, Netalith, Diffen): 3‑way handshake, seq/ACK/retransmit/RTO, in‑order delivery & **head‑of‑line blocking**, flow vs congestion control (CUBIC/BBR/Reno), **20 B vs 8 B** headers, connectionless fire‑and‑forget, **QUIC** rebuilding reliability over UDP, when‑to‑use mapping.
- IANA — service names & port numbers (well‑known/registered/dynamic).
- **Blueprint anchor:** CCNA 200‑301 v1.1 **1.5** (compare TCP to UDP); bit‑level headers in `0.11.2`; transport layer in Block 0 models; app services (DNS/DHCP/NTP/SNMP/syslog/TFTP) in Block 6; transport deep‑dive in Block 5.

---

> **CCNA Domain 1.0 continues**, on your **go**, one at a time:
> - `1.6–1.7` **IPv4 addressing & subnetting** + private addressing
> - `1.8–1.9` **IPv6 addressing, prefixes & address types**
> - `1.10` **Verify IP parameters on client OS** (Windows/macOS/Linux)
> - `1.11` **Wireless principles** (channels, SSID, RF, encryption)
> - `1.12` **Virtualization fundamentals** (VMs, containers, VRFs)
> - `1.13` **Switching concepts** (MAC learning/aging, flooding, MAC table)
>
> Tell me which, and I'll build it.
