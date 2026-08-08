# 1.8–1.9 · IPv6 Addressing, Prefixes & Address Types 🌍6️⃣

> **CCNA 200‑301 Domain 1.0 · topics 1.8 + 1.9** — "Configure and verify IPv6 addressing and prefix" (1.8) + "Describe IPv6 address types" (1.9: unicast/anycast/multicast + modified EUI‑64)
> **The real fix for the exhaustion `1.7` worked around.** Combined into one doc because you can't configure IPv6 addressing without knowing the **types** you're configuring. Standard **14‑section study template**.

---

## 1 · Base Data 📇

| Field | Value |
|---|---|
| **Title** | IPv6 Addressing, Prefixes & Address Types |
| **Doc ID** | `1.8–1.9` |
| **Date / Era** | 2 Aug 2026 · dual‑stack ubiquitous; IPv6 the default on modern OSes/mobile |
| **Topic** | The **128‑bit IPv6 address**, its **compression & prefix** rules, the **address types**, **EUI‑64**, and **SLAAC/config** |
| **Scope** | 🟢 CCNA 1.8 (configure/verify addressing & prefix) + 1.9 (unicast global/ULA/link‑local, anycast, multicast, modified EUI‑64) · 🔵 engineer: SLAAC, NDP, solicited‑node multicast, privacy IIDs |
| **Standards** | **RFC 4291** (addressing architecture) · **RFC 4193** (ULA) · **RFC 4862** (SLAAC) · **RFC 4861** (NDP) · RFC 8981 (privacy IIDs) · RFC 3849 (docs 2001:db8::/32) |
| **Core idea** | 128 bits → **340 undecillion** addresses → every device gets a **global** address → **no NAT for scarcity**; addressing is **type‑rich** (many addresses per interface) |
| **References** | `1.7` (IPv4 exhaustion this solves) · `1.6` (subnetting concepts) · `1.1` (MAC→EUI‑64), NDP replaces ARP · Block 0 (L3 packet) |

---

## 2 · Visualization — structure, types, EUI‑64 🗺️

```
 A 128-BIT ADDRESS = 8 hextets (4 hex digits each), colon-separated:
   2001:0db8:85a3:0000:0000:8a2e:0370:7334
   └─ compress: drop leading zeros + one run of all-zero hextets → :: ─┘
   → 2001:db8:85a3::8a2e:370:7334

 THE /64 SPLIT (the near-universal subnet size):
   2001:db8:85a3:0001 :  0000:0000:0000:0001
   └──── 64-bit PREFIX (network) ────┘ └── 64-bit INTERFACE ID (host) ──┘

 ADDRESS TYPES (1.9):
 ┌───────────────┬──────────────┬───────────────────────────────────────────┐
 │ type          │ prefix       │ role (IPv4 analog)                        │
 ├───────────────┼──────────────┼───────────────────────────────────────────┤
 │ Global Unicast│ 2000::/3     │ internet-routable (≈ public IPv4)         │
 │ Unique Local  │ FC00::/7→FD  │ private (≈ RFC 1918)                       │
 │ Link-Local    │ FE80::/10    │ this-link-only, AUTO on every interface    │
 │ Multicast     │ FF00::/8     │ one-to-many (REPLACES broadcast)           │
 │ Anycast       │ (from GUA)   │ one-to-nearest (same addr, many devices)   │
 │ Loopback      │ ::1/128      │ localhost (≈ 127.0.0.1)                     │
 │ Unspecified   │ ::/128       │ "no address yet"                          │
 └───────────────┴──────────────┴───────────────────────────────────────────┘
 NO BROADCAST in IPv6 — multicast does its job (e.g. FF02::1 all-nodes, FF02::2 all-routers)

 MODIFIED EUI-64 (build a 64-bit interface ID from a 48-bit MAC):
   MAC 00:1A:2B:3C:4D:5E
   1) split:            001A2B | 3C4D5E
   2) insert FFFE:      001A2B  FFFE  3C4D5E
   3) flip 7th bit of 1st byte (U/L): 00→02
   → interface ID 021A:2BFF:FE3C:4D5E   →  link-local FE80::021A:2BFF:FE3C:4D5E
```

---

## 3 · TL;DR ⚡

- An **IPv6 address is 128 bits** = **8 hextets** of 4 hex digits, colon‑separated. That's **~340 undecillion** addresses — enough that **every device can have a global one**, so **NAT‑for‑scarcity disappears** and end‑to‑end reachability returns.
- **Compress** to stay sane: **drop leading zeros** in a hextet, and replace **one** run of all‑zero hextets with **::** (only once per address).
- **Prefix / interface‑ID split:** a subnet is almost always a **/64** — 64‑bit **network prefix** + 64‑bit **interface ID**. Allocations: **/48** enterprise, **/56** home, **/32** ISP.
- **Address types (1.9):**
  - **Unicast** (one‑to‑one): **Global (2000::/3)** = internet‑routable (≈ public IPv4); **Unique Local / ULA (FC00::/7, in practice FD00::/8)** = private (≈ RFC 1918); **Link‑Local (FE80::/10)** = **auto‑configured on every interface**, this‑link‑only, used for **NDP** and routing‑protocol adjacencies.
  - **Multicast (FF00::/8)** = one‑to‑many. 🔑 **IPv6 has NO broadcast** — multicast replaces it (FF02::1 all‑nodes, FF02::2 all‑routers, and **solicited‑node** for neighbor discovery).
  - **Anycast** = one‑to‑nearest (the **same** address on multiple devices; routed to the closest). Taken from the unicast pool — indistinguishable by format.
- 🔑 **Modified EUI‑64** builds the 64‑bit interface ID from a 48‑bit MAC: **split, insert FFFE, flip the 7th (U/L) bit**. Modern OSes often use **random privacy IIDs** instead (MAC exposure concern).
- **SLAAC** lets a host **auto‑build its own address** from a router's advertised prefix — no DHCP needed. **NDP** (ICMPv6) **replaces ARP** and does much more (router/prefix discovery, DAD).
- An IPv6 interface normally holds **several addresses at once** (a link‑local *and* one or more global, maybe a temporary) — that's normal, not a bug.
- **Configure:** `ipv6 unicast-routing`; `ipv6 address 2001:db8:1::1/64` (or `… eui-64`, or `autoconfig`). **Verify:** `show ipv6 interface brief`, `show ipv6 route`, `show ipv6 neighbors`. **Dual‑stack** = run IPv4 + IPv6 together during transition.

---

## 4 · Importance — the problem first 🎯

**The problem:** `1.7` told the first half of the story — IPv4's **4.3 billion** addresses ran out, and **private addressing + NAT** were the workaround. But NAT is exactly that: a **workaround**, and it broke something valuable. When every host hides behind a shared public address:

1. **End‑to‑end reachability is lost.** Two hosts can't simply connect to each other by address — NAT rewrites and hides them. Peer‑to‑peer, VoIP, gaming, and IoT all fight NAT with hole‑punching, relays, and STUN/TURN hacks.
2. **The address space is *still* fundamentally too small.** NAT stretched IPv4 but didn't add addresses — and the world keeps adding billions of devices (IoT). You cannot NAT your way to enough.
3. **NAT adds state, complexity, and fragility** at every edge — a translation table that must be maintained, that breaks certain protocols, and that becomes a choke point.

**IPv6 is the actual fix, not a workaround:** it makes the address **128 bits** instead of 32. That's not "4× bigger" — it's **2⁹⁶ times** bigger, about **340 undecillion** (3.4×10³⁸) addresses. To picture it: you could give **every grain of sand on Earth** billions of addresses and not run out. With that abundance:

- **Every device gets a real, globally unique address** — no sharing, no NAT‑for‑scarcity, **end‑to‑end restored**.
- **Autoconfiguration becomes practical** — a host can safely invent its own address (SLAAC) because collisions are astronomically unlikely.
- **The protocol got redesigned while they were at it** — a simpler fixed header, **multicast replacing broadcast** (more efficient), **NDP replacing ARP** (richer), and built‑in autoconfiguration.

So IPv6's importance isn't just "more addresses." It's **removing the scarcity that forced NAT**, and with it restoring the internet's original end‑to‑end model — plus a cleaner, more automated addressing architecture. Learning it isn't optional: it's **already the default** on mobile networks and modern operating systems, running **dual‑stack** alongside IPv4 everywhere.

🔵 **Why the depth?** Because IPv6 *looks* intimidating (long hex strings) but is **more regular** than IPv4 once you see the pattern: master the **prefixes** (2000::/3, FE80::/10, FC00::/7, FF00::/8) and the **/64 split**, and the rest follows. The engineer‑grade payoff is understanding **why /64**, **why link‑local is mandatory**, **why there's no broadcast**, and **how EUI‑64 and SLAAC build addresses automatically**.

---

## 5 · Description 📋

**Structure & notation** 🟢
- **128 bits**, **8 hextets** (4 hex digits, 16 bits each), colon‑separated.
- **Compression:** (a) omit **leading zeros** per hextet (`0db8`→`db8`, `0000`→`0`); (b) replace **one** run of consecutive all‑zero hextets with **::** (once only, to stay unambiguous).
- **Prefix:** `/N` network bits; a subnet is almost always **/64**.

**Unicast (one‑to‑one)** 🟢
- **Global Unicast (GUA) — 2000::/3** — internet‑routable, like public IPv4 (`2001:db8::/32` is documentation, RFC 3849).
- **Unique Local (ULA) — FC00::/7**, in practice **FD00::/8** (L‑bit=1 = locally assigned) — private, like RFC 1918.
- **Link‑Local (LLA) — FE80::/10** — **auto‑generated on every interface** the moment it powers on; **not routed** beyond the link; used for **NDP, router discovery, and routing‑protocol adjacencies**.

**Multicast (one‑to‑many) — FF00::/8** 🟢 — **replaces broadcast** (IPv6 has none). Second byte carries **flags + scope**. Well‑known: **FF02::1** (all‑nodes), **FF02::2** (all‑routers), **FF02::1:FFxx:xxxx** (**solicited‑node**, used by NDP).

**Anycast (one‑to‑nearest)** 🟢 — the **same** address configured on **multiple** devices; routing delivers to the **closest**. Drawn from the **unicast** pool — **indistinguishable** from unicast by format. (Subnet‑router anycast = interface ID all zeros.)

**Special** 🟢 — **::1/128** (loopback, ≈127.0.0.1) · **::/128** (unspecified) · **::/0** (default route) · **::ffff:0:0/96** (IPv4‑mapped).

**Modified EUI‑64 (1.9.d)** 🟢🔵 — a way to build the 64‑bit interface ID from a 48‑bit MAC (§6.3).

---

## 6 · How It Works — the mechanics ⚙️

### 6.1 Compression, worked 🟢
```
Full:        2001:0db8:0000:0000:0000:ff00:0042:8329
Drop zeros:  2001:db8:0:0:0:ff00:42:8329
Collapse ::  2001:db8::ff00:42:8329        (one run of zeros → ::, once only)
```
⚠️ **:: only once** — `2001::85a3::7334` is ambiguous (you couldn't tell how many zero‑hextets each `::` hides).

### 6.2 The /64 split & prefix 🟢
```
2001:db8:acad:0001 : 0000:0000:0000:0100  /64
└──── network prefix (64) ────┘ └─ interface ID (64) ─┘
/48 = enterprise (65,536 /64s) · /56 = home (256 /64s) · /32 = ISP
```

### 6.3 Modified EUI‑64 (MAC → interface ID) 🔑
```
MAC (48-bit):        00:1A:2B : 3C:4D:5E
1) SPLIT in half:    001A2B | 3C4D5E
2) INSERT FFFE:      001A2B  FFFE  3C4D5E      (→ 64 bits)
3) FLIP the 7th bit of the 1st byte (U/L bit):
       00 = 0000 0000 → flip bit 7 → 0000 0010 = 02
→ interface ID = 021A:2BFF:FE3C:4D5E
→ link-local   = FE80::021A:2BFF:FE3C:4D5E
```
🔵 **Privacy note:** EUI‑64 **embeds the MAC** → it can track a device across networks. Modern OSes therefore default to **random/temporary interface IDs** (privacy extensions, RFC 8981), keeping a stable one for servers and rotating ones for clients.

### 6.4 SLAAC & NDP (autoconfiguration + ARP's replacement) 🔵
```
SLAAC (Stateless Address Autoconfiguration, RFC 4862):
   host powers on → makes a LINK-LOCAL (FE80 + interface ID)
   → sends Router Solicitation (RS) → router replies Router Advertisement (RA) with the /64 prefix
   → host forms its GLOBAL address = advertised /64 prefix + its interface ID  (no DHCP needed)

NDP (Neighbor Discovery, RFC 4861 — replaces ARP, over ICMPv6):
   RS / RA  → find routers, learn prefix/gateway
   NS / NA  → find a neighbor's MAC (ARP's job) + Duplicate Address Detection (DAD)
```

### 6.5 Solicited‑node multicast (why there's no broadcast) 🔑
```
IPv4 ARP: BROADCAST "who has 10.0.0.5?" → EVERY host on the LAN is interrupted.
IPv6 NDP: MULTICAST to the SOLICITED-NODE group FF02::1:FF + last 24 bits of the target
          → only hosts whose address shares those last 24 bits are interrupted.
→ far fewer devices woken up per lookup than an IPv4 broadcast.
```

### 6.6 Configure & verify 🟢
```
Router(config)# ipv6 unicast-routing                     ! enable IPv6 routing globally
Router(config)# interface GigabitEthernet0/0
Router(config-if)# ipv6 address 2001:db8:1::1/64          ! static global
Router(config-if)# ipv6 address 2001:db8:1::/64 eui-64    ! auto interface ID via EUI-64
Router(config-if)# ipv6 address fe80::1 link-local        ! (optional) manual link-local
Router(config-if)# ipv6 address autoconfig                ! SLAAC on this interface
Router# show ipv6 interface brief     ! GUA + FE80 per interface
Router# show ipv6 route               ! connected/learned prefixes
Router# show ipv6 neighbors           ! the NDP cache (IPv6's "ARP table")
```

### 6.7 ⚡ IPv6 relationships (the "why") 🔑
> IPv6's design choices all trace back to abundance and cleanup.

**① 128 bits ↔ exhaustion solved (and end‑to‑end restored).** 2⁹⁶× more addresses than IPv4 → every device gets a **global** address → **NAT‑for‑scarcity is unnecessary** → the direct, peer‑to‑peer reachability NAT broke comes back. *The address size doesn't just add space; it removes the reason NAT existed.*

**② /64 ↔ interface ID & SLAAC.** The near‑universal **/64** subnet exists because **SLAAC** (and EUI‑64) need a **64‑bit interface ID**. Deviate from /64 on a LAN and **SLAAC stops working**. *The subnet size is fixed by autoconfiguration, not by host count.*

**③ EUI‑64 ↔ MAC exposure ↔ privacy IIDs.** A deterministic MAC‑derived IID is convenient but **leaks the MAC and enables tracking** → modern OSes use **random/temporary** IIDs. *Convenience (EUI‑64) traded for privacy (random) — know which your hosts use.*

**④ No broadcast ↔ multicast efficiency.** IPv6 **removed broadcast** and uses **multicast** (esp. **solicited‑node**) so lookups interrupt **only relevant** hosts, not everyone → less wasted CPU/airtime than IPv4 ARP broadcasts. *Replacing broadcast with targeted multicast is a scalability win.*

**⑤ Link‑local mandatory ↔ the plumbing address.** Every interface **auto‑gets an FE80 address** with no config; **NDP, router discovery, and routing‑protocol adjacencies** (OSPFv3, EIGRPv6) run over it. Because it's independent of the global prefix, **renumbering the global prefix doesn't disturb the IGP**. *Link‑local is the always‑present foundation IPv6 is built on.*

**⑥ ULA ↔ RFC 1918 (but often unneeded).** ULA is IPv6's private space — but with **abundant GUA**, many designs skip private addressing entirely (use global + firewall). *IPv6 makes "private for scarcity" optional; use ULA for stability/isolation, not to save addresses.*

**⑦ Multiple addresses per interface ↔ IPv6 norm.** An interface routinely holds a **link‑local + one or more global + maybe a temporary** simultaneously, each for a different **scope/purpose** — unlike IPv4's usual one‑per‑interface. *Seeing several IPv6 addresses is correct, not a misconfiguration.*

**⑧ Dual‑stack ↔ transition.** During migration, hosts run **IPv4 and IPv6 together** and prefer IPv6 when available. *Adoption is additive — IPv6 arrives alongside IPv4, not by cutover.*

---

## 7 · Pros & Cons ⚖️

| Aspect | ✅ Benefit | ⚠️ Cost / caveat |
|---|---|---|
| **128‑bit space** | Effectively unlimited; global per device; no NAT | Long addresses; new habits |
| **SLAAC** | Zero‑touch host addressing | Less central control than DHCP (use DHCPv6 for options) |
| **EUI‑64** | Deterministic IID from MAC | Exposes MAC → privacy IIDs preferred (§6.7 ③) |
| **No broadcast (multicast)** | Efficient neighbor discovery | New mental model vs IPv4 ARP |
| **Link‑local everywhere** | Always‑on plumbing; stable IGP | Confusing at first (FE80 on every interface) |
| **Restored end‑to‑end** | P2P/VoIP/IoT without NAT hacks | Security must be explicit (no accidental NAT "hiding") |
| **Dual‑stack** | Smooth transition | Two stacks to manage/secure |

---

## 8 · Variants — types & assignment 🌿

**Address types (1.9), full set:**
- **Unicast:** Global (2000::/3), Unique Local (FC00::/7 → FD00::/8), Link‑Local (FE80::/10).
- **Multicast (FF00::/8):** all‑nodes FF02::1, all‑routers FF02::2, solicited‑node FF02::1:FFxx:xxxx; scope in the 2nd byte (FF02 link, FF05 site…).
- **Anycast:** from the unicast pool; subnet‑router anycast = IID all‑zeros.
- **Special:** loopback ::1, unspecified ::, default ::/0, IPv4‑mapped ::ffff:0:0/96.

**Assignment methods:**
- **Static** (manual) · **SLAAC** (stateless, from RA) · **DHCPv6 stateful** (server assigns full address) · **DHCPv6 stateless** (SLAAC for address + DHCPv6 for DNS/options) · **EUI‑64** vs **random/privacy** interface IDs.

**Prefix allocations:** /32 ISP → /48 enterprise → /56 home → **/64 subnet** (the standard) → /127 (P2P links, RFC 6164) → /128 (single host/loopback).

**Transition:** **dual‑stack** (both together) · tunneling (6in4, etc.) · translation (NAT64) — deep in CCNP.

---

## 9 · Devices / Media / Protocols 🔌

| Item | Type | Role |
|---|---|---|
| **NDP (Neighbor Discovery)** | Protocol (ICMPv6) | Replaces ARP; RS/RA, NS/NA, DAD |
| **ICMPv6** | Protocol | Carries NDP, errors, MLD |
| **SLAAC** | Mechanism | Host auto‑builds address from RA |
| **DHCPv6** | Protocol | Stateful/stateless address & option assignment |
| **`ipv6 unicast-routing`** | Config | Enable IPv6 routing |
| **`ipv6 address … /64 [eui-64\|link-local]`** | Config | Assign an address |
| **`ipv6 address autoconfig`** | Config | SLAAC on an interface |
| **`show ipv6 interface brief`** | Verify | GUA + FE80 per interface |
| **`show ipv6 route`** | Verify | Prefixes in the routing table |
| **`show ipv6 neighbors`** | Verify | NDP cache (≈ ARP table) |
| **Multicast groups (FF02::1/2, solicited‑node)** | Addressing | Built‑in protocol machinery |

---

## 10 · Best Practices ✅

- **Use /64 for every LAN subnet** — it's what SLAAC/EUI‑64 require; deviating breaks autoconfiguration (§6.7 ②).
- **Master the four prefixes** (2000::/3, FE80::/10, FC00::/7, FF00::/8) — everything else follows (§5).
- **Expect and accept multiple addresses per interface** (link‑local + global) — it's normal (§6.7 ⑦).
- **Prefer privacy (random) interface IDs for clients**, stable ones for servers (§6.7 ③).
- **Let routing protocols peer over link‑local** so global renumbering doesn't disrupt the IGP (§6.7 ⑤).
- **Choose SLAAC vs DHCPv6 deliberately:** SLAAC for simple addressing; DHCPv6 (stateful/stateless) when you need central control or to hand out DNS/options.
- **Deploy dual‑stack** during transition; don't treat IPv6 as "later" — it's already the default on mobile/modern OSes (§6.7 ⑧).
- **Verify with `show ipv6 interface brief` / `route` / `neighbors`** after config (§6.6).

---

## 11 · No‑Goes 🚫

- ❌ **Using `::` more than once** in an address — ambiguous and invalid (§6.1).
- ❌ **Subnetting a LAN to anything but /64** and expecting SLAAC to work — it won't (§6.7 ②).
- ❌ **Forgetting `ipv6 unicast-routing`** — the router won't route IPv6 without it (§6.6).
- ❌ **Assuming IPv6 has broadcast** — it doesn't; use multicast (FF02::1 etc.) (§6.7 ④).
- ❌ **Treating multiple interface addresses as an error** — link‑local + global coexist by design (§6.7 ⑦).
- ❌ **Relying on EUI‑64 where privacy matters** — it embeds the MAC (§6.7 ③).
- ❌ **Blocking all ICMPv6** — NDP *is* ICMPv6; kill it and IPv6 stops working (unlike IPv4 where blocking ICMP is often fine) (§9).
- ❌ **Assuming "no NAT" means "no security."** IPv6 restores reachability — you must apply explicit firewalling (§6.7 ①; `1.1.8`).

---

## 12 · Terms 📖

- **IPv6 address** — 128‑bit host identifier. 🔗 [RFC 4291](https://www.rfc-editor.org/rfc/rfc4291) · `./terms/ipv6-address.md`
- **Hextet** — 16‑bit (4‑hex) group. 🔗 [ref](https://networkingtoolbox.net/reference/ipv6-address-types) · `./terms/hextet.md`
- **Zero compression (::)** — collapse one run of zero hextets. 🔗 [RFC 4291](https://www.rfc-editor.org/rfc/rfc4291) · `./terms/ipv6-compression.md`
- **Prefix / interface ID (/64)** — network vs host split. 🔗 [RFC 4291](https://www.rfc-editor.org/rfc/rfc4291) · `./terms/ipv6-prefix.md`
- **Global Unicast (2000::/3)** — internet‑routable. 🔗 [ref](https://itintrail.com/2025/11/25/ipv6-address-types-link-local-global-ula-part2/) · `./terms/gua.md`
- **Unique Local / ULA (FC00::/7→FD00::/8)** — private. 🔗 [RFC 4193](https://www.rfc-editor.org/rfc/rfc4193) · `./terms/ula.md`
- **Link‑Local (FE80::/10)** — this‑link, auto, mandatory. 🔗 [RFC 4291](https://www.rfc-editor.org/rfc/rfc4291) · `./terms/link-local.md`
- **Multicast (FF00::/8)** — one‑to‑many; replaces broadcast. 🔗 [RFC 4291](https://www.rfc-editor.org/rfc/rfc4291) · `./terms/ipv6-multicast.md`
- **Solicited‑node multicast** — targeted NDP group. 🔗 [RFC 4291](https://www.rfc-editor.org/rfc/rfc4291) · `./terms/solicited-node.md`
- **Anycast** — one‑to‑nearest (from unicast pool). 🔗 [RFC 4291](https://www.rfc-editor.org/rfc/rfc4291) · `./terms/anycast.md`
- **Modified EUI‑64** — MAC → interface ID. 🔗 [ref](https://www.pinglabz.com/ipv6-address-types/) · `./terms/eui-64.md`
- **SLAAC** — stateless address autoconfiguration. 🔗 [RFC 4862](https://www.rfc-editor.org/rfc/rfc4862) · `./terms/slaac.md`
- **NDP (Neighbor Discovery)** — ARP's ICMPv6 replacement. 🔗 [RFC 4861](https://www.rfc-editor.org/rfc/rfc4861) · `./terms/ndp.md`
- **DHCPv6** — stateful/stateless assignment. 🔗 [Cisco](https://www.cisco.com) · `./terms/dhcpv6.md`
- **Privacy interface IDs** — random IIDs. 🔗 [RFC 8981](https://www.rfc-editor.org/rfc/rfc8981) · `./terms/privacy-iid.md`
- **Dual‑stack** — IPv4 + IPv6 together. 🔗 [Cisco](https://www.cisco.com) · `./terms/dual-stack.md`

---

## 13 · Practical Tasks 🧪

1. **Compress & expand** 🟢 — compress `2001:0db8:0000:0000:0abc:0000:0000:1234` and expand `fe80::1`; state where `::` may (and may not) go (§6.1).
2. **EUI‑64 by hand** 🔑 — from MAC `00:0C:29:AB:CD:EF`, build the interface ID and the link‑local address (split, insert FFFE, flip U/L bit) (§6.3).
3. **Configure dual‑stack** 🟢 — in Packet Tracer, `ipv6 unicast-routing`, assign `2001:db8:1::1/64` to an interface, put two hosts on it; ping over IPv6; check `show ipv6 interface brief` (both GUA + FE80) (§6.6).
4. **SLAAC in action** 🔵 — enable RAs on a router; watch a host auto‑form its global address from the advertised /64 + its interface ID; verify no DHCP was used (§6.4).
5. **Identify types** 🟢 — classify `2001:db8::5`, `fe80::a1`, `fd12:3456::1`, `ff02::1`, `::1` by type and prefix (§5).
6. **NDP vs ARP** 🔵 — capture NDP (`icmpv6`) neighbor solicitation to a **solicited‑node** multicast; contrast with an IPv4 ARP broadcast — note who gets interrupted (§6.5).
7. **See your own addresses** 🟢 — `ipconfig` / `ip -6 addr`: find your link‑local, global, and any temporary/privacy address; explain why there are several (§6.7 ⑦; `1.10`).

---

## 14 · Sources 📚

- IETF — **RFC 4291** (IPv6 Addressing Architecture: 128‑bit, hextets, ::, prefixes, GUA 2000::/3, LLA FE80::/10, multicast FF00::/8, solicited‑node, anycast, EUI‑64), **RFC 4193** (ULA FC00::/7→FD00::/8), **RFC 4862** (SLAAC), **RFC 4861** (NDP), **RFC 8981** (privacy IIDs), **RFC 3849** (docs 2001:db8::/32), RFC 6164 (/127 links).
- IPv6 references (IPCisco, PingLabz, NetworkingToolbox, ITinTrail, NetworkCheckr, ChrisGrundemann, NetworkLessons, O'Reilly CCNP): address‑type prefixes & analogs, **no broadcast → multicast**, **link‑local mandatory & used for IGP adjacencies**, **ULA = FD00::/8 in practice**, **EUI‑64 (insert FFFE, flip 7th bit)**, SLAAC vs DHCPv6, multiple addresses per interface, `show ipv6 interface brief`.
- **Blueprint anchor:** CCNA 200‑301 v1.1 **1.8** (configure/verify IPv6 addressing & prefix) + **1.9** (unicast global/ULA/link‑local, anycast, multicast, modified EUI‑64); exhaustion context from `1.7`; verify on client OS in `1.10`; IPv6 routing in Blocks 3–4.

---

> **CCNA Domain 1.0 continues**, on your **go**, one at a time:
> - `1.10` **Verify IP parameters on client OS** (Windows/macOS/Linux) — the hands‑on IPv4+IPv6 verification companion to `1.6–1.9`
> - `1.11` **Wireless principles** (channels, SSID, RF, encryption)
> - `1.12` **Virtualization fundamentals** (VMs, containers, VRFs)
> - `1.13` **Switching concepts** (MAC learning/aging, flooding, MAC table)
>
> Tell me which, and I'll build it.
