# 1.6 · IPv4 Addressing & Subnetting 🔢🌐

> **CCNA 200‑301 Domain 1.0 · topic 1.6** — "Configure and verify IPv4 addressing and subnetting"
> **The single highest‑value CCNA skill.** A concept + hands‑on‑math topic → standard **14‑section study template**, built to be *teachable*: the **block‑size (magic‑number) method** with worked examples you can do by hand in under 30 seconds. **Private addressing (RFC 1918)** is topic **1.7** — mentioned here, detailed there.

---

## 1 · Base Data 📇

| Field | Value |
|---|---|
| **Title** | IPv4 Addressing & Subnetting |
| **Doc ID** | `1.6` |
| **Date / Era** | 2 Aug 2026 · classless (CIDR) addressing; IPv4 still ubiquitous alongside IPv6 |
| **Topic** | The **32‑bit IPv4 address**, the **subnet mask / CIDR prefix**, and **subnetting** — dividing one network into many, sized to need |
| **Scope** | 🟢 CCNA 1.6: address structure, masks/CIDR, subnetting, VLSM, configure & verify · 🔵 engineer: the fast hand‑method, /31, route/mask interaction |
| **Standards** | **RFC 791** (IPv4) · **RFC 4632** (CIDR, 1993) · **RFC 3021** (/31) · **RFC 1918** (private → `1.7`) · RFC 5735/IANA (special‑use) |
| **Core idea** | An address is **network portion + host portion**; the **mask** decides where the split falls — and *you* control the split |
| **References** | Block 0 (binary, L3 packet PDU) · `1.1.6` (routing/longest‑prefix match, mask in FIB) · `1.7` (private addressing) · `1.8–1.9` (IPv6) |

---

## 2 · Visualization — the address, the split, the method 🗺️

```
 A 32-BIT ADDRESS (4 octets, dotted decimal):     192   .   168   .   1   .   50
                                                  11000000.10101000.00000001.00110010
 THE MASK DECIDES THE SPLIT (here /24):           └──── NETWORK ────┘ └─ HOST ─┘
   /24 = 255.255.255.0 = 11111111.11111111.11111111.00000000
         (1 = network bit)                        (0 = host bit)

 ┌──────────── THE BLOCK-SIZE (MAGIC NUMBER) METHOD ────────────┐
 │ 1. block size = 256 − (last non-zero mask octet)             │
 │ 2. network = round the IP's octet DOWN to a multiple of block│
 │ 3. broadcast = network + block − 1                           │
 │ 4. usable hosts = network+1 … broadcast−1  (= 2^h − 2)       │
 └───────────────────────────────────────────────────────────────┘

 CIDR QUICK TABLE (4th-octet subnets of a /24):
 ┌──────┬─────────────────┬───────┬────────┬──────────┬────────────┐
 │ CIDR │ mask            │ block │ hosts  │ subnets  │ wildcard   │
 ├──────┼─────────────────┼───────┼────────┼──────────┼────────────┤
 │ /24  │ 255.255.255.0   │ 256   │ 254    │ 1        │ 0.0.0.255  │
 │ /25  │ 255.255.255.128 │ 128   │ 126    │ 2        │ 0.0.0.127  │
 │ /26  │ 255.255.255.192 │ 64    │ 62     │ 4        │ 0.0.0.63   │
 │ /27  │ 255.255.255.224 │ 32    │ 30     │ 8        │ 0.0.0.31   │
 │ /28  │ 255.255.255.240 │ 16    │ 14     │ 16       │ 0.0.0.15   │
 │ /29  │ 255.255.255.248 │ 8     │ 6      │ 32       │ 0.0.0.7    │
 │ /30  │ 255.255.255.252 │ 4     │ 2      │ 64       │ 0.0.0.3    │
 │ /31* │ 255.255.255.254 │ 2     │ 2*     │ 128      │ 0.0.0.1    │  *RFC 3021 P2P
 │ /32  │ 255.255.255.255 │ 1     │ 1      │ —        │ 0.0.0.0    │  host route
 └──────┴─────────────────┴───────┴────────┴──────────┴────────────┘

 WORKED: 192.168.1.50 /27 → block 32 → network .32 → broadcast .63 → hosts .33–.62
```

---

## 3 · TL;DR ⚡

- An **IPv4 address is 32 bits** = four 8‑bit **octets** in dotted decimal (0–255 each). It splits into a **network portion** and a **host portion**.
- The **subnet mask** (or **CIDR prefix /N**) marks the split: **N leading 1s = network**, the rest **0s = host**. `/24` = `255.255.255.0`. Classless (**CIDR**) means the split can fall **anywhere**, not just on class boundaries.
- **Subnetting = borrowing host bits to make more networks.** Every borrowed bit **doubles the subnets** and **halves the hosts** — a fixed 32‑bit budget split between "how many subnets" and "how many hosts each."
- 🔑 **The fast method is block size (magic number):** **block = 256 − (last non‑zero mask octet)**. Subnets start at **multiples of the block**; **network** = round the address down to a multiple; **broadcast** = network + block − 1; **usable hosts** = the addresses between them (**2^host‑bits − 2**).
- **Two addresses per subnet are reserved:** the **network address** (all host bits 0) and the **broadcast** (all host bits 1) — that's the "**− 2**." Exception: **/31** (RFC 3021) uses **both** addresses on a **point‑to‑point** link (no network/broadcast).
- **Host counts to memorize:** /30 = 2, /29 = 6, /28 = 14, /27 = 30, /26 = 62, /25 = 126, /24 = 254, /23 = 510, /22 = 1022 (each prefix doubles).
- **VLSM** = use **different mask sizes** in the same space so each subnet is **right‑sized** (a /30 or /31 for a 2‑host router link, a /26 for a 60‑host LAN) → **no waste**.
- **Configure & verify:** `ip address <addr> <mask>` on an interface; check with `show ip interface brief`, `show ip route`, `show running-config`.

---

## 4 · Importance — the problem first 🎯

**The problem:** you have one block of addresses and a whole organization to connect — and if you put **every device on one flat network**, three things break:

1. **Broadcasts drown everyone.** Every ARP, DHCP, and broadcast reaches **every** device in the network. At a few thousand hosts, the broadcast noise alone melts performance (the broadcast‑domain problem from `1.1`). → you must **split one big network into many smaller ones**.
2. **There's no structure to route or secure.** A flat network has no boundaries — you can't put the sales floor on its own segment, apply a firewall rule per department, or summarize routes. → subnets create **boundaries** you can route between and apply policy to.
3. **Addresses get wasted or run out.** Hand every link a huge block and you exhaust your space; hand every LAN the same size and a 2‑host router link wastes 250 addresses. → you must **size each subnet to its actual need**.

**Subnetting** is the tool that solves all three: it takes one network and **divides it into right‑sized pieces**, each its own **broadcast domain**, each a **boundary** for routing and security. The **subnet mask** is how a device knows where its own network ends — so it can decide, for any destination, *"is this local (send directly) or remote (send to my gateway)?"* That single decision, made by masking, is the foundation of all IP routing.

And here's why it's *the* CCNA skill: **subnetting is not memorization — it's a repeating pattern.** Every subnet has a **block size**; subnets always start at **multiples of that block**. Once you know the block size for a prefix, you can find any subnet's network, broadcast, and host range **in your head in seconds**. The exam tests exactly this, and so does every real IP‑planning task an engineer does — the day you can subnet fluently is the day IP networks stop being mysterious.

🔵 **Why the depth?** Because the difference between "using a subnet calculator" and *understanding* subnetting is knowing **why** each borrowed bit doubles subnets and halves hosts, **why** two addresses are always reserved, **why** /31 is the exception, and **why** VLSM prevents waste. That understanding is what lets you *design* an addressing plan, not just look one up.

---

## 5 · Description 📋

**The address** 🟢
- **32 bits**, four **octets** (0–255), dotted decimal (e.g. `192.168.1.50`). Each octet = 8 bits; binary ↔ decimal is the foundation (Block 0).
- Split into **network portion** (identifies the subnet) + **host portion** (identifies the device on it).

**The mask / CIDR prefix** 🟢
- **Subnet mask** — a 32‑bit value with **N leading 1s** (network) then **0s** (host), written dotted‑decimal (`255.255.255.0`). Must be **contiguous** 1s then 0s.
- **CIDR notation** — `/N` = the number of network bits (`/24`). Any prefix **/0–/32** is legal (classless).

**Classful (historical context)** 🔵 — the first octet used to fix the class: **A** (1–126, /8), **B** (128–191, /16), **C** (192–223, /24), **D** (224–239, multicast), **E** (240–255, experimental). Classful **wasted** huge space, so **CIDR (RFC 4632, 1993)** replaced it with **variable prefixes**. Classful boundaries still explain **default masks** and the **private ranges** (`1.7`).

**Special addresses** 🟢 — **network address** (all host bits 0, unusable) · **broadcast** (all host bits 1, unusable) · `127.0.0.0/8` (loopback) · `169.254.0.0/16` (link‑local / **APIPA**, self‑assigned when DHCP fails) · `0.0.0.0` (default/this‑host) · `255.255.255.255` (limited broadcast).

**VLSM** 🟢 — **Variable Length Subnet Masking**: different mask sizes within one address space, so each subnet fits its host count.

---

## 6 · How It Works — the method & the math ⚙️

### 6.1 The block‑size (magic‑number) method 🔑
```
Given an IP and a prefix, find network / broadcast / host range:

1. BLOCK SIZE  = 256 − (last non-zero octet of the mask)
                 (or 2^(8 − subnet-bits-in-that-octet))
2. Identify the "interesting octet" (where the mask isn't 255 or 0).
3. NETWORK     = round that octet DOWN to the nearest multiple of the block size.
4. BROADCAST   = network + block size − 1  (in the interesting octet).
5. HOST RANGE  = network+1  …  broadcast−1.
6. USABLE HOSTS= 2^(host bits) − 2.
```

### 6.2 Worked examples 🟢
**A) Find everything for `192.168.1.50 /27`:**
```
mask /27 = 255.255.255.224 → block = 256 − 224 = 32
50 rounds down to 32 (multiples: 0,32,64,96…)
NETWORK  = 192.168.1.32
BROADCAST= 32 + 32 − 1 = 63 → 192.168.1.63
HOSTS    = 192.168.1.33 … 192.168.1.62   (2^5 − 2 = 30 usable)
```
**B) Same‑subnet check — can `172.16.50.200/27` reach `172.16.50.220` directly?**
```
block = 32 → 200 rounds down to 192 → network .192, broadcast .223, range .193–.222
Is 220 in .193–.222?  Yes (192 ≤ 220 ≤ 223).
→ SAME /27 subnet → they talk directly, no router needed.
```
**C) Size a subnet for exactly 20 hosts (from a /24):**
```
need 2^h − 2 ≥ 20 →  2^5 − 2 = 30 ✓  (2^4 − 2 = 14 ✗ too small)
h = 5 host bits → prefix = 32 − 5 = /27  (30 usable hosts — smallest that fits 20)
```
**D) A router‑to‑router link (2 hosts):** `/30` (2 usable) traditionally, or **`/31`** (RFC 3021) to save an address — both addresses usable, no network/broadcast, **point‑to‑point only**.

### 6.3 Subnetting = borrowing bits 🔑
```
Start: 192.168.1.0/24  → 8 host bits → 254 hosts, 1 network
Borrow 2 bits (→ /26)  → 2^2 = 4 subnets, each 2^6 − 2 = 62 hosts
Borrow 3 bits (→ /27)  → 2^3 = 8 subnets, each 2^5 − 2 = 30 hosts
   … every borrowed bit DOUBLES subnets and HALVES hosts (§6.5 ①)
```

### 6.4 VLSM — right‑size each subnet 🟢
```
Given 192.168.1.0/24, allocate: LAN-A 60 hosts, LAN-B 28, LAN-C 12, two P2P links (2 each)
1. SORT largest first: 60, 28, 12, 2, 2
2. LAN-A: needs /26 (62) → 192.168.1.0/26   (.0–.63)
3. LAN-B: needs /27 (30) → 192.168.1.64/27  (.64–.95)
4. LAN-C: needs /28 (14) → 192.168.1.96/28  (.96–.111)
5. P2P-1: /30 (2) → 192.168.1.112/30 (.112–.115)
6. P2P-2: /30 (2) → 192.168.1.116/30 (.116–.119)
   → no overlap, minimal waste (a fixed /27-everywhere plan would run out or waste)
```

### 6.5 ⚡ Subnetting relationships (the "why" behind the math) 🔑
> Subnetting is one fixed 32‑bit budget, split between competing demands. These are the trade‑offs.

**① Subnet bits ↔ host bits — the master trade‑off.** The 32 bits are **fixed**. Every bit you give to the **network** side (borrowing) is one **taken from hosts**: **+1 network bit = ×2 subnets and ÷2 hosts**. You cannot have more of both. *Choosing a mask is choosing where on this seesaw to sit.*

**② Prefix length ↔ block size ↔ scale.** Longer prefix (more network bits) → **smaller block** → **more subnets, fewer hosts each**; shorter prefix → bigger block → fewer, larger subnets. *The prefix is a single dial from "many tiny subnets" to "few huge ones."*

**③ The "− 2" ↔ reserved network & broadcast.** Every subnet reserves its **first** address (network ID, all host bits 0) and **last** (broadcast, all host bits 1) → usable = **2^h − 2**. *You always lose two per subnet — which is exactly why over‑subnetting into many tiny subnets wastes addresses (§①).*

**④ /31 ↔ reclaiming the reserved pair.** On a **point‑to‑point** link there's only one possible destination, so a broadcast is pointless → **RFC 3021 /31** uses **both** addresses as hosts (no network/broadcast). *The one place the "− 2" rule is suspended — because P2P needs neither reserved address.*

**⑤ Mask ↔ the local/remote decision.** A host/router **ANDs** a destination IP with the mask: same network portion → **local** (deliver directly via ARP); different → **remote** (send to the **default gateway**). *The mask isn't just math — it's how every device decides "do I route this or not."* (Ties to longest‑prefix match, `1.1.6`.)

**⑥ VLSM ↔ efficiency.** One fixed mask everywhere either **wastes** (a /24 on a 2‑host link) or **runs out** (a /28 on a 60‑host LAN). VLSM **sizes each subnet to its need**, packing them contiguously. *Match the subnet to the demand and the address space stretches far further.*

**⑦ Contiguous bits ↔ valid mask.** A mask must be **1s then 0s, no gaps** (255.255.255.192 ✓; 255.0.255.0 ✗). *The network/host boundary is a single line, not scattered — so masks are always contiguous.*

**⑧ Wildcard ↔ inverse mask.** ACLs and OSPF use the **wildcard** (bitwise inverse: /24 → `0.0.0.255`; 0 = "must match," 1 = "don't care"). *Same boundary, flipped, for matching instead of addressing* (deep in Blocks 4/9).

---

## 7 · Pros & Cons ⚖️

| Aspect | ✅ Benefit | ⚠️ Cost |
|---|---|---|
| **Subnetting** | Smaller broadcast domains; routing/security boundaries; hierarchy | Two addresses lost per subnet; planning effort |
| **Classless (CIDR)** | Precise sizing; route aggregation; no class waste | Requires understanding masks (not just class) |
| **VLSM** | Minimal waste; fits real host counts | More complex plan; must avoid overlaps |
| **/30 P2P** | Simple, universally supported | Wastes 2 of 4 addresses |
| **/31 P2P** | Saves an address per link (RFC 3021) | P2P‑only; some old gear won't support it |
| **Bigger subnets (/23, /22)** | Fewer subnets to manage; more hosts | Larger broadcast domains |

---

## 8 · Variants — prefixes, links, special addresses 🌿

- **The full CIDR ladder** (/8 → /32): each step changes the network/host split by one bit. Common: **/24** (254 hosts, the default LAN), **/23** (510), **/22** (1022), **/25–/29** (subnets of a /24), **/30** & **/31** (P2P), **/32** (host route / loopback).
- **Point‑to‑point:** **/30** (2 usable) or **/31** (RFC 3021, 2 usable, no network/broadcast).
- **Host route:** **/32** — a single address (loopbacks, specific routes).
- **VLSM:** mixed masks in one space (largest‑first allocation).
- **Special‑use:** loopback `127.0.0.0/8`, link‑local/APIPA `169.254.0.0/16`, default `0.0.0.0/0`, limited broadcast `255.255.255.255`, **private RFC 1918** (→ `1.7`), documentation/TEST‑NET ranges.
- **Classful (legacy context):** A/B/C default masks, still behind default behaviour and private ranges.

---

## 9 · Devices / Media / Protocols 🔌

| Item | Type | Role |
|---|---|---|
| **Subnet mask / CIDR** | Addressing | Splits network vs host |
| **Wildcard mask** | Matching | ACLs/OSPF (inverse of mask) |
| **`ip address <a> <mask>`** | Config | Assign an interface address |
| **`interface … / no shutdown`** | Config | Bring the interface up |
| **`show ip interface brief`** | Verify | Per‑interface IP + status |
| **`show ip route`** | Verify | Connected/learned subnets |
| **`show running-config`** | Verify | Configured addresses |
| **`ping` / `traceroute`** | Verify | Reachability across subnets |
| **Network / broadcast address** | Reserved | First / last per subnet |
| **Default gateway** | Role | Exit for off‑subnet traffic (§6.5 ⑤) |

**Config example:**
```
Router(config)# interface GigabitEthernet0/0
Router(config-if)# ip address 192.168.1.1 255.255.255.192   ! a /26 gateway
Router(config-if)# no shutdown
Router# show ip interface brief        ! verify: 192.168.1.1  up/up
Router# show ip route                  ! verify: C 192.168.1.0/26 connected
```

---

## 10 · Best Practices ✅

- **Learn the block‑size method cold** — block = 256 − mask octet; subnets on multiples; it makes any subnet a 30‑second head calculation (§6.1).
- **Size subnets to real host counts with VLSM** — allocate **largest first**, pack contiguously, avoid overlaps (§6.4, §6.5 ⑥).
- **Use /30 or /31 for point‑to‑point links** — don't burn a /24 on a 2‑host router link (§6.5 ④⑥).
- **Plan the addressing hierarchy** so subnets summarize cleanly (aids routing later, Blocks 3–4).
- **Reserve the gateway consistently** (e.g. first usable) so it's predictable across subnets.
- **Verify after configuring:** `show ip interface brief` (up/up + correct IP) and `show ip route` (the connected subnet appears) (§9).
- **Remember the local/remote test:** same network portion → local; different → gateway. It explains most "can't reach that host" issues (§6.5 ⑤).

---

## 11 · No‑Goes 🚫

- ❌ **Assigning the network or broadcast address to a host.** They're reserved — the device won't work right (§6.5 ③).
- ❌ **Forgetting the "− 2."** A /28 has 16 addresses but **14** usable hosts — plan for usable, not total (§6.5 ③).
- ❌ **Applying normal host math to a /31.** RFC 3021 uses **both** addresses; no network/broadcast (§6.5 ④).
- ❌ **Non‑contiguous masks** (255.0.255.0) — invalid; masks are 1s‑then‑0s (§6.5 ⑦).
- ❌ **One fixed mask everywhere.** Wastes on tiny links, runs out on big LANs — use VLSM (§6.5 ⑥).
- ❌ **Overlapping subnets in a VLSM plan.** Allocate largest‑first and check ranges (§6.4).
- ❌ **Confusing wildcard and subnet mask.** Wildcard is the **inverse**, for matching — not for addressing (§6.5 ⑧).
- ❌ **Assuming two hosts with different network portions can talk directly.** Different subnets → they need a **router** (§6.5 ⑤).

---

## 12 · Terms 📖

- **IPv4 address** — 32‑bit host identifier. 🔗 [RFC 791](https://www.rfc-editor.org/rfc/rfc791) · `./terms/ipv4-address.md`
- **Octet** — 8‑bit segment of an address. 🔗 [ref](https://www.rfc-editor.org/rfc/rfc791) · `./terms/octet.md`
- **Subnet mask** — marks network vs host bits. 🔗 [Cisco: IP addressing & subnetting](https://www.cisco.com/c/en/us/support/docs/ip/routing-information-protocol-rip/13788-3.html) · `./terms/subnet-mask.md`
- **CIDR / prefix (/N)** — classless notation. 🔗 [RFC 4632](https://www.rfc-editor.org/rfc/rfc4632) · `./terms/cidr.md`
- **Network portion / host portion** — the two parts of an address. 🔗 [ref](https://calcfort.com/learn/engineering/subnet) · `./terms/network-host-portion.md`
- **Block size (magic number)** — 256 − mask octet; subnet spacing. 🔗 [ref](https://thelineman.ca/articles/article-7-cidr-cheat-sheet) · `./terms/block-size.md`
- **Network address** — first address, all host bits 0. 🔗 [Cisco](https://www.cisco.com) · `./terms/network-address.md`
- **Broadcast address** — last address, all host bits 1. 🔗 [Cisco](https://www.cisco.com) · `./terms/broadcast-address.md`
- **Usable hosts (2^h − 2)** — assignable addresses per subnet. 🔗 [ref](https://itstudyhub.org/subnetting-guide.html) · `./terms/usable-hosts.md`
- **VLSM** — variable‑length subnet masking. 🔗 [ref](https://calcfort.com/learn/engineering/subnet) · `./terms/vlsm.md`
- **/30 / /31 (RFC 3021)** — point‑to‑point subnets. 🔗 [RFC 3021](https://www.rfc-editor.org/rfc/rfc3021) · `./terms/p2p-subnets.md`
- **Classful addressing (A/B/C/D/E)** — legacy fixed classes. 🔗 [ref](https://basicfreetools.com/blog/cidr-notation-and-subnetting-explained/) · `./terms/classful.md`
- **Wildcard mask** — inverse mask for matching. 🔗 [Cisco](https://www.cisco.com) · `./terms/wildcard-mask.md`
- **APIPA / link‑local (169.254/16)** — self‑assigned when DHCP fails. 🔗 [ref](https://computingforgeeks.com/subnetting-cheat-sheet/) · `./terms/apipa.md`
- **Loopback (127/8)** — the host's own address. 🔗 [RFC 5735](https://www.rfc-editor.org/rfc/rfc5735) · `./terms/loopback.md`
- **Default gateway** — exit for off‑subnet traffic. 🔗 [Cisco](https://www.cisco.com) · `./terms/default-gateway.md`

---

## 13 · Practical Tasks 🧪

1. **Block‑size drill** 🔑 — for `10.20.30.77/26`, `172.16.5.130/25`, `192.168.4.200/28`, find network, broadcast, and host range by hand using §6.1. Verify with a subnet calculator.
2. **Size subnets to need** 🔑 — pick the smallest prefix for 100, 50, 25, and 2 hosts; state usable counts (§6.2 C).
3. **VLSM plan** 🔑 — carve `192.168.10.0/24` into 100‑, 50‑, 20‑host LANs and three P2P links with no overlap and minimal waste (§6.4).
4. **Same‑subnet check** 🟢 — are `172.16.8.20/22` and `172.16.11.200/22` on the same subnet? Show the block‑size reasoning (§6.2 B).
5. **Configure & verify** 🟢 — in Packet Tracer, set a /26 gateway on a router interface, put two PCs in that subnet, confirm they ping each other and the gateway; check `show ip interface brief` and `show ip route` (§9).
6. **/30 vs /31 link** 🔵 — connect two routers, first with a /30, then a /31 (RFC 3021); confirm both work and note the address saved (§6.5 ④).
7. **Prove the gateway rule** 🔵 — put two PCs in *different* /27 subnets on the same switch with no router; show they can't ping until an L3 device routes between them (§6.5 ⑤; ties to `1.1.6`).

---

## 14 · Sources 📚

- IETF — **RFC 791** (IPv4), **RFC 4632** (CIDR, 1993), **RFC 3021** (/31 point‑to‑point), **RFC 5735** (special‑use), **RFC 1918** (private → `1.7`); IANA IPv4 Special‑Purpose Registry.
- Subnetting references (ComputingForGeeks, ITStudyHub, TheLineman, Calculators.im, RouterHax, CalcFort, BasicFreeTools): **block‑size/magic‑number method**, **2^h − 2** hosts, host‑count table (/30=2 … /22=1022), **network = round‑down, broadcast = network+block−1**, worked same‑subnet checks, **VLSM** largest‑first allocation, **/31 RFC 3021**, wildcard = inverse mask, APIPA/loopback special ranges.
- Cisco — "IP Addressing and Subnetting for New Users"; Wendell Odom OCG (subnetting + CIDR, Vol 1 ch. 11–13).
- **Blueprint anchor:** CCNA 200‑301 v1.1 **1.6** (configure & verify IPv4 addressing & subnetting); binary in Block 0; routing/mask interaction in `1.1.6`; **private addressing in `1.7`**; IPv6 in `1.8–1.9`.

---

> **CCNA Domain 1.0 continues**, on your **go**, one at a time:
> - `1.7` **Private IPv4 addressing** (RFC 1918 ranges, NAT preview) — the natural companion to this doc
> - `1.8–1.9` **IPv6 addressing, prefixes & types**
> - `1.10` **Verify IP parameters on client OS**
> - `1.11` **Wireless principles** · `1.12` **Virtualization** · `1.13` **Switching concepts**
>
> Tell me which, and I'll build it.
