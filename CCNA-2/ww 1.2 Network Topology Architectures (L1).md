# 1.2 · Network Topology Architectures 🏛️

> **Block 1 — Physical Layer & Network Components (L1)** · CCNA 200‑301 **1.2.a–f**
> **A design topic, not a device** — so this uses the **standard 14‑section study template**. It ties together the devices from `1.1` (which box sits at which layer) and sets up the campus/DC/WAN structure the rest of the course lives inside.
> *Doc ID kept as `1.2` to match the CCNA blueprint and prior cross‑references; say the word to renumber.*

---

## 1 · Base Data 📇

| Field | Value |
|---|---|
| **Title** | Network Topology Architectures |
| **Doc ID** | `1.2` |
| **Date / Era** | 2 Aug 2026 · spine‑leaf mainstream in the DC; SD‑WAN & cloud on‑ramp standard |
| **Topic** | How networks are **structured** — the layered/mesh designs that make them scalable, redundant, and fast |
| **Scope** | 🟢 CCNA 1.2: **two‑tier · three‑tier · spine‑leaf · WAN · SOHO · on‑prem vs cloud** · 🔵 engineer: traffic patterns, oversubscription, ECMP, Clos, design trade‑offs |
| **Key idea** | **Match the topology to the traffic** — north‑south wants hierarchy; east‑west wants a fabric |
| **Devices in play** | access = **L2 switch** (`1.1.5`) · distribution/core = **L3 switch** (`1.1.6`) · WAN edge = **router** (`1.1.7`) + **NGFW** (`1.1.8`) · leaf/spine = **L3 switches** · SOHO = all‑in‑one · DC = **servers** (`1.1.2`) |
| **Related** | Block 7 (virtualization/cloud) deepens on‑prem vs cloud; CCNP ENSLD/ENCOR deepen campus & DC design |

---

## 2 · Visualization — the architectures at a glance 🗺️

```
 THREE-TIER (campus, north-south)          TWO-TIER / COLLAPSED CORE (smaller campus)
        🌐 core (backbone)                        🌐 collapsed core+distribution
        /            \                                 /          \
   distribution   distribution                    (core & dist merged into one layer)
     /     \        /     \                          /     \       /     \
  access  access access access                   access  access access access
   |  |    |  |    |  |    |  |                     |  |    |  |    |  |    |  |
  endpoints (PCs, phones, APs)                    endpoints

 SPINE-LEAF (data center, east-west)        WAN (sites across distance)
   spine ── spine ── spine   (core)          HQ ──┐    ┌── branch
    │ ╳  ╳   │ ╳  ╳  │        full mesh            │ SP │  (MPLS / Internet / 5G)
   leaf   leaf   leaf   leaf (ToR access)     DC ──┤net ├── branch   hub-and-spoke
    │       │      │      │                        │    │            or (partial) mesh
  servers servers servers                     cloud┘    └── branch

 SOHO (home/small office)                    ON-PREM vs CLOUD
  ┌──────────────────────────┐               on-prem: you own the DC (capex, control)
  │ ISP ── [ all-in-one:     │               cloud:   you rent it (opex, elastic)
  │         router+switch+   │               models:  IaaS / PaaS / SaaS
  │         AP+firewall ]    │               deploy:  private / public / hybrid / multi
  │          │   │   ((wifi))│               link:    VPN / Direct Connect / cloud on-ramp
  └──────────┴───┴──────────┘
```

**One‑line map**

| Architecture | Where | Optimized for |
|---|---|---|
| **Three‑tier** | large campus | north‑south (client↔server) |
| **Two‑tier (collapsed core)** | smaller campus | same, fewer blocks, less cost |
| **Spine‑leaf** | data center | **east‑west** (server↔server) |
| **WAN** | between sites | distance, over provider links |
| **SOHO** | home/tiny office | simplicity, one box |
| **On‑prem vs cloud** | where compute lives | control vs elasticity |

---

## 3 · TL;DR ⚡

- Networks are **structured on purpose**. The structure decides **scalability, redundancy, latency, and cost** — pick the wrong one and you get bottlenecks or waste.
- **Campus hierarchy = access → distribution → core.** **Access** connects endpoints (L2 switches, PoE); **distribution** aggregates and routes between VLANs (L3 switches, policy, redundancy); **core** is the high‑speed backbone linking distribution blocks.
  - **Three‑tier** = all three layers → for **large** campuses with many distribution blocks.
  - **Two‑tier (collapsed core)** = distribution + core merged → for **smaller** networks that don't need a separate core.
- 🔑 **Spine‑leaf** is the modern **data‑center** design: **every leaf connects to every spine** (full mesh, a **Clos** fabric), no leaf‑to‑leaf. Any server reaches any other in a **consistent leaf→spine→leaf path** → **predictable latency + ECMP** load‑balancing across all uplinks + **linear scale** (add leaves for ports, spines for bandwidth). It **routes at L3 (no STP‑blocked links)**.
- 🔑 **The reason spine‑leaf replaced three‑tier in the DC: the traffic turned sideways.** Old apps were monolithic → mostly **north‑south** (client↔server) → hierarchy was perfect. Modern apps (virtualization, microservices, distributed storage, AI/ML) are mostly **east‑west** (server↔server) → hierarchy bottlenecks, a fabric doesn't. **Match the topology to the dominant traffic direction.**
- **WAN** connects sites across distance over **provider links** (MPLS/Internet/5G), as **hub‑and‑spoke** or **(partial) mesh**, with a **router/NGFW** at the edge (increasingly **SD‑WAN**).
- **SOHO** collapses everything into **one all‑in‑one box** (router+switch+AP+firewall) — flat, simple, minimal redundancy.
- **On‑prem vs cloud** is *where the compute lives*: **on‑prem = capex + control**; **cloud = opex + elastic scale** (**IaaS/PaaS/SaaS**; private/public/**hybrid**/multicloud).

---

## 4 · Importance — the problem first 🎯

**The problem:** you can connect ten devices any old way and it works. Connect **ten thousand** the same careless way and it collapses — broadcasts drown everything, a single failure takes down half the building, you can't find where traffic is slow, and you can't grow without rebuilding. **Scale breaks unstructured networks.**

Three pressures force *structure*:

1. **Scale.** You must add hundreds of ports/sites without redesigning from scratch. → a **modular, layered** design where you grow by **adding blocks** (another access switch, another distribution block, another leaf), not by rebuilding.
2. **Resilience.** A cable, switch, or power supply *will* fail. The network must **survive** it and **contain** it. → **redundant paths** and **failure domains** bounded by layers, so one failure doesn't cascade.
3. **Performance that matches the traffic.** Traffic has a **direction** — into/out of the site (north‑south) or side‑to‑side between servers (east‑west) — and the structure must put bandwidth **where the traffic actually flows**. → hierarchy for north‑south, **fabric** for east‑west.

The **hierarchical model** (access/distribution/core) answers all three for campuses: modular growth, layered failure domains, and a clear path for client‑to‑server traffic. The **spine‑leaf fabric** answers them for the data center, where traffic went sideways. The **WAN** answers them across distance. **SOHO** deliberately *ignores* them (it's too small to need them). And **cloud** answers "do I even want to own the structure?"

Remove topology design and you get a **flat network**: one giant broadcast domain, one giant failure domain, no room to grow, and traffic taking random paths. Topology is the discipline that turns a pile of switches into a **network that scales, survives, and performs**.

🔵 **Why the depth?** Because the single most consequential design decision — *which topology* — follows from one question most beginners never ask: **which way does the traffic flow?** Get that right and everything (oversubscription, redundancy, device placement) follows. Get it wrong and you build a beautiful hierarchy that chokes on east‑west traffic.

---

## 5 · Description 📋

**The hierarchical layers (the vocabulary):**
- **Access layer** 🟢 — where **endpoints connect**: L2 switches (`1.1.5`), PoE for APs/phones, VLANs. Highest port density, lowest cost per port.
- **Distribution (aggregation) layer** 🟢 — **aggregates access switches**, performs **inter‑VLAN routing** (L3 switches, `1.1.6`), enforces **policy/ACLs/QoS**, and is the **redundancy boundary**. Where L2 meets L3.
- **Core layer** 🟢 — the **high‑speed backbone** that interconnects distribution blocks (and the DC/WAN edge). Does little policy — just forwards *fast*.

**The architectures:**
- **Three‑tier** 🟢 — access + distribution + core, as three separate layers.
- **Two‑tier / collapsed core** 🟢 — distribution and core **merged** into one layer (access + collapsed‑core).
- **Spine‑leaf** 🟢 — a two‑tier **Clos fabric**: **leaf** switches (top‑of‑rack, connect servers) each uplink to **every spine** switch (the fast transport). No endpoints on spines; no leaf‑to‑leaf/spine‑to‑spine links.
- **WAN** 🟢 — interconnects **geographically separate** sites over **service‑provider** transport.
- **SOHO** 🟢 — small office/home office: one integrated device, flat network.
- **On‑premises vs cloud** 🟢 — infrastructure you **own/operate** vs **rent** from a provider.

---

## 6 · How It Works — each architecture & its traffic ⚙️

### 6.1 Three‑tier (campus, built for north‑south) 🟢
```
Endpoint → ACCESS switch → DISTRIBUTION (L3, inter-VLAN + policy) → CORE → out (WAN/DC/Internet)
```
- Traffic flows **vertically**: endpoints up to the distribution's gateway, across the core, and **north** to servers/Internet. The core links many **distribution blocks**; add a block to grow.
- 🔵 **The catch:** redundant L2 links between layers must be managed by **STP**, which **blocks** the backups to prevent loops → **wasted bandwidth** and **variable path lengths** (§6.5 ①).

### 6.2 Two‑tier / collapsed core 🟢
When there aren't enough distribution blocks to justify a dedicated core, **merge distribution + core** into one layer. Fewer devices, less cost/complexity — but it **doesn't scale** to many blocks (the collapsed layer becomes the bottleneck). *Right‑size the hierarchy to the campus.*

### 6.3 Spine‑leaf (data center, built for east‑west) 🔑
```
server → LEAF (ToR) → SPINE → LEAF (ToR) → server     (always leaf→spine→leaf)
every leaf uplinks to EVERY spine · no leaf-leaf, no spine-spine · L3 routed (no STP)
```
- **Any server reaches any other via one spine** — a **consistent, predictable path** → **uniform latency** regardless of which racks the servers are in.
- **ECMP** load‑balances flows across **all** spine uplinks (an L3 underlay running OSPF/IS‑IS/**BGP**; L2 services via **VXLAN/EVPN** overlay). **No STP‑blocked links** → all bandwidth is usable.
- **Scales horizontally:** add **leaves** for more server ports; add **spines** for more inter‑rack bandwidth. Growth is **linear**, not a redesign (§6.5 ④).

### 6.4 WAN, SOHO, cloud 🟢
- **WAN** — sites across distance connected over **provider** transport (MPLS L3VPN, Metro Ethernet, Internet VPN, broadband, **4G/5G**, DWDM). Layouts: **hub‑and‑spoke** (all branches to HQ — cheap, but HQ is a chokepoint and spoke‑to‑spoke detours through it), **full mesh** (every site to every site — direct but **N²** links, expensive), **partial mesh** (a pragmatic middle). A **router/NGFW** sits at each edge; **SD‑WAN** overlays multiple transports with central policy (CCNP ENSDWI).
- **SOHO** — one **all‑in‑one** device (router+switch+AP+firewall, often the ISP modem too); a **flat** network, one/few subnets, minimal redundancy. The bottom rung of the router/firewall descents in `1.1.7`/`1.1.8`.
- **On‑prem vs cloud** — **on‑prem**: you own the DC (**capex**, full control, you maintain it). **Cloud**: rent from AWS/Azure/GCP (**opex**, elastic scale, provider maintains). **Service models:** **IaaS** (VMs/network — you manage OS‑up), **PaaS** (platform — you deploy apps), **SaaS** (finished software — you just use it). **Deployment:** private / public / **hybrid** / multicloud, connected via **VPN, Direct Connect/ExpressRoute, or cloud on‑ramp** (ties to WAN/SD‑WAN; deep in Block 7).

### 6.5 ⚡ Design relationships (the trade‑offs that decide the topology) 🔑
> Topology is a set of trade‑offs. These are the ones you actually reason about.

**① Traffic direction ↔ topology — the master relationship.** **North‑south** (client↔server, in/out of the site) → **hierarchy** (three/two‑tier) is ideal. **East‑west** (server↔server inside the DC) → **spine‑leaf fabric**. Building a hierarchy for east‑west traffic creates bottlenecks; building a fabric for a small north‑south campus is overkill. *First ask which way the traffic flows, then choose.*

**② Redundancy ↔ cost & complexity.** More paths (toward full mesh) = more resilience **and** more links, devices, and configuration. Full mesh is **N² links**; hub‑and‑spoke is cheap but fragile at the hub. Hierarchy and partial mesh are deliberate **middles**. *You buy resilience with links and complexity — spend it where failure hurts most.*

**③ STP‑blocked links ↔ usable bandwidth (three‑tier).** L2 redundancy needs **STP**, which **blocks** backup links to kill loops → you **pay for links you can't use** and get **variable paths**. Spine‑leaf's **L3 + ECMP** uses **every** link at once — a core reason the DC moved to it. *Loop prevention at L2 costs you bandwidth; L3 fabrics don't pay that tax.*

**④ Scale‑out (horizontal) ↔ scale‑up (vertical).** Three‑tier often scales **vertically** (bigger core boxes) → hits a ceiling. Spine‑leaf scales **horizontally** (add leaves/spines) → **linear, non‑disruptive** growth. *Horizontal scale is why fabrics win in fast‑growing DCs.*

**⑤ Oversubscription ↔ tier bandwidth.** Every tier boundary has a ratio — access‑to‑distribution uplinks, or **leaf‑to‑spine** uplinks vs server ports. Under‑provision it and the **uplink**, not the endpoint, caps throughput (the switch‑fabric lesson from `1.1.5` §9.4 ①②, now at network scale). *Design the oversubscription to the traffic, tier by tier.*

**⑥ Latency predictability ↔ path uniformity.** In three‑tier, two endpoints may be 2 hops or 6 hops apart → **variable latency**. In spine‑leaf, **every** server pair is the same leaf→spine→leaf distance → **uniform latency** (critical for storage, HPC, AI). *Consistent structure buys consistent performance.*

**⑦ Failure domain ↔ modularity.** Layered/modular blocks **contain** failures (a distribution block or a rack fails, not the whole site). A **flat** SOHO‑style network is **one big failure domain**. *Structure is also blast‑radius control.*

**⑧ Cloud ↔ capex/opex & control/elasticity.** **On‑prem** = big upfront **capex**, full **control**, you carry the maintenance and the scaling limits. **Cloud** = **opex**, near‑infinite **elastic** scale, but less control and new concerns (latency to cloud, egress cost, compliance). **Hybrid** splits the difference. *It's an ownership‑and‑elasticity trade, not just "where the servers are."*

---

## 7 · Pros & Cons ⚖️

| Architecture | ✅ Strengths | ⚠️ Trade‑offs |
|---|---|---|
| **Three‑tier** | Scales to large campuses; clear failure domains; great for north‑south | STP‑blocked links; variable latency; more devices |
| **Two‑tier (collapsed core)** | Cheaper/simpler for smaller sites | Collapsed layer bottlenecks as you grow |
| **Spine‑leaf** | Uniform latency; ECMP uses all links; linear scale; great for east‑west | Higher initial complexity (L3/VXLAN/EVPN); more cabling |
| **WAN (hub‑spoke)** | Cheap; simple central policy | HQ is a chokepoint; spoke‑to‑spoke detours |
| **WAN (full mesh)** | Direct any‑to‑any | N² links; costly/complex |
| **SOHO** | One box; cheap; trivial to run | Flat; no redundancy; no real segmentation |
| **On‑prem** | Full control; data locality | Capex; you own scaling & maintenance |
| **Cloud** | Elastic; opex; provider‑maintained | Less control; egress cost; latency/compliance |

---

## 8 · Variants — sub‑topologies & models 🌿

- **Campus hierarchy:** flat → **two‑tier (collapsed core)** → **three‑tier** → (add SD‑Access fabric overlay, CCNP).
- **WAN layouts:** **point‑to‑point** → **hub‑and‑spoke** → **partial mesh** → **full mesh**; transports: leased line → MPLS L3VPN → Metro Ethernet → Internet VPN → **4G/5G** → DWDM; overlay: **SD‑WAN**.
- **Data‑center:** three‑tier (legacy) → **spine‑leaf (Clos)** → multi‑pod/multi‑site fabrics (Cisco **ACI**, VXLAN/EVPN).
- **Cloud service models:** **IaaS** → **PaaS** → **SaaS** (you manage progressively less).
- **Cloud deployment models:** **private** → **public** → **hybrid** → **multicloud**.
- **Cloud connectivity:** site‑to‑site **VPN** → **Direct Connect/ExpressRoute** → **cloud on‑ramp** (SD‑WAN).

---

## 9 · Devices / Media / Protocols 🔌

| Layer / role | Device (from `1.1`) | Notes |
|---|---|---|
| **Access** | **L2 switch** (`1.1.5`) + PoE | Endpoints, APs (`1.1.3`), phones; VLANs |
| **Distribution** | **L3 switch** (`1.1.6`) | Inter‑VLAN routing, policy, L2/L3 boundary |
| **Core** | fast **L3 switch / router** | High‑speed backbone; minimal policy |
| **Leaf (ToR)** | **L3 switch** | Connects servers; uplinks to all spines |
| **Spine** | high‑throughput **L3 switch** | Fast transport; connects all leaves |
| **WAN edge** | **router** (`1.1.7`) + **NGFW** (`1.1.8`) | Terminates provider links; SD‑WAN |
| **SOHO** | all‑in‑one (router+switch+AP+FW) | Fused consumer device |
| **Data center hosts** | **servers** (`1.1.2`) | Physical/virtual compute |
| **Protocols** | STP (L2 loops) · **OSPF/IS‑IS/BGP** (L3 underlay) · **ECMP** · **VXLAN/EVPN** (overlay) · SD‑WAN | Deep in later blocks |
| **Media** | copper/fiber, optics family (`1.1.5`); provider transport (MPLS/Metro‑E/5G/DWDM) | Cabling in `1.3` |

---

## 10 · Best Practices ✅

- **Choose the topology from the traffic:** north‑south → hierarchy; east‑west → spine‑leaf (§6.5 ①). Ask the traffic‑direction question *first*.
- **Right‑size the hierarchy:** collapse the core for small campuses; add a dedicated core only when you have enough distribution blocks to need one (§6.2).
- **Put the L2/L3 boundary at distribution** (or at the leaf in a fabric); keep access simple and the core fast/policy‑free.
- **Design oversubscription per tier**, tuned to the traffic — don't starve the uplinks (§6.5 ⑤; `1.1.5`).
- **Use L3 + ECMP in the DC** to use every link, instead of leaving bandwidth blocked by STP (§6.5 ③).
- **Build modularly** so growth is "add a block/leaf/spine," and failures stay contained (§6.5 ④⑦).
- **Match WAN layout to need:** hub‑and‑spoke for central apps; (partial) mesh where spoke‑to‑spoke matters; SD‑WAN to blend transports.
- **Decide on‑prem vs cloud on control, elasticity, cost model, latency, and compliance** — hybrid is often the honest answer (§6.5 ⑧; Block 7).

---

## 11 · No‑Goes 🚫

- ❌ **Building a hierarchy for an east‑west data center.** It bottlenecks at aggregation; that's what spine‑leaf fixed (§6.5 ①).
- ❌ **Over‑engineering a small site with three tiers.** Collapse the core; don't pay for a backbone you don't need (§6.2).
- ❌ **Ignoring oversubscription at tier boundaries.** The uplink caps throughput no matter how fast the access ports are (§6.5 ⑤).
- ❌ **Relying on STP to "use" redundant L2 links.** STP **blocks** them — you're paying for idle bandwidth; go L3/ECMP in the fabric (§6.5 ③).
- ❌ **Full‑meshing a large WAN "for redundancy."** N² links = cost and complexity explosion; use partial mesh/SD‑WAN (§6.5 ②).
- ❌ **Treating a SOHO all‑in‑one as an enterprise design.** Flat, no redundancy, one failure domain — fine for home, not for a campus (§6.5 ⑦).
- ❌ **Assuming cloud is automatically cheaper/better.** Egress cost, latency, and compliance can flip the math; it's a control/elasticity trade (§6.5 ⑧).
- ❌ **Scaling three‑tier vertically forever.** You hit a ceiling; fabrics scale horizontally (§6.5 ④).

---

## 12 · Terms 📖

- **Access / distribution / core layers** — the campus hierarchy. 🔗 [Cisco design](https://www.cisco.com) · `./terms/campus-layers.md`
- **Three‑tier architecture** — access + distribution + core. 🔗 [Cisco](https://www.cisco.com) · `./terms/three-tier.md`
- **Two‑tier / collapsed core** — distribution + core merged. 🔗 [ref](https://codilime.com/blog/spine-leaf-vs-traditional-dcs/) · `./terms/collapsed-core.md`
- **Spine‑leaf / Clos fabric** — full‑mesh two‑tier DC design. 🔗 [Cisco ACI Clos WP](https://www.cisco.com/c/en/us/solutions/collateral/data-center-virtualization/application-centric-infrastructure/white-paper-c11-742214.html) · `./terms/spine-leaf.md`
- **North‑south / east‑west traffic** — client↔server / server↔server. 🔗 [ref](https://www.networkacademy.io/ccna/network-fundamentals/leaf-spine-architecture) · `./terms/traffic-directions.md`
- **ECMP** — equal‑cost multipath load balancing. 🔗 [ref](https://en.wikipedia.org/wiki/Equal-cost_multi-path_routing) · `./terms/ecmp.md`
- **VXLAN / EVPN** 🔵 — L2 overlay on an L3 fabric. 🔗 [Cisco](https://www.cisco.com) · `./terms/vxlan-evpn.md`
- **Oversubscription** — uplink vs downlink bandwidth ratio. 🔗 [Cisco](https://www.cisco.com) · `./terms/oversubscription.md`
- **WAN** — network across geographic distance. 🔗 [Cisco](https://www.cisco.com) · `./terms/wan.md`
- **Hub‑and‑spoke / mesh** — WAN layouts. 🔗 [Cisco](https://www.cisco.com) · `./terms/wan-topologies.md`
- **SD‑WAN** 🔵 — overlay over multiple transports. 🔗 [Cisco](https://www.cisco.com) · `./terms/sd-wan.md`
- **SOHO** — small office/home office. 🔗 [Cisco](https://www.cisco.com) · `./terms/soho.md`
- **On‑premises / cloud** — owned vs rented infrastructure. 🔗 [Cisco](https://www.cisco.com) · `./terms/on-prem-cloud.md`
- **IaaS / PaaS / SaaS** — cloud service models. 🔗 [NIST cloud def](https://csrc.nist.gov/pubs/sp/800/145/final) · `./terms/cloud-service-models.md`
- **Private / public / hybrid / multicloud** — deployment models. 🔗 [NIST](https://csrc.nist.gov/pubs/sp/800/145/final) · `./terms/cloud-deployment-models.md`
- **Failure domain** — the blast radius of a failure. 🔗 [Cisco](https://www.cisco.com) · `./terms/failure-domain.md`

---

## 13 · Practical Tasks 🧪

1. **Label the layers** 🟢 — in Packet Tracer, build access → distribution → core with a couple of VLANs; identify where inter‑VLAN routing happens (distribution/L3) and where endpoints connect (access).
2. **Collapse the core** 🟢 — take the three‑tier lab and redraw it two‑tier; explain what you saved and what you'd lose as it grows (§6.2).
3. **Spine‑leaf ECMP** 🔵 — (GNS3) build 2 spines + 2 leaves with an OSPF/BGP L3 underlay; `show ip route` on a leaf to see **ECMP** paths via both spines; trace a leaf→spine→leaf path (§6.3).
4. **Traffic‑direction call** 🔑 — for (a) a branch office of desktops using cloud apps and (b) a virtualization cluster with heavy VM‑to‑VM traffic, decide hierarchy vs spine‑leaf and justify via north‑south vs east‑west (§6.5 ①).
5. **WAN layout trade‑off** 🔵 — for 12 branches that mostly talk to HQ but occasionally to each other, compare hub‑and‑spoke vs partial mesh vs SD‑WAN on cost, redundancy, and spoke‑to‑spoke latency (§6.4, §6.5 ②).
6. **Oversubscription math** 🔵 — a leaf has 48×25G server ports and 6×100G spine uplinks; compute the oversubscription ratio and reason about when it bites (§6.5 ⑤).
7. **Cloud decision** 🔵 — for a latency‑sensitive on‑prem app vs a bursty seasonal workload, argue on‑prem vs cloud vs hybrid on capex/opex, elasticity, latency, and compliance (§6.5 ⑧).

---

## 14 · Sources 📚

- Cisco — **ACI Multi‑Tier / Clos Spine‑Leaf White Paper**: 2‑tier spine‑and‑leaf, Clos fabric, VXLAN overlay, consistent latency, rise of **east‑west** traffic from virtualization; three‑tier (core/aggregation/access) still valid where appropriate.
- Data‑center design references (NetworkAcademy.IO, IntelligentVisibility, network‑switch.com, TheNetworkDNA, CodiLime, ComputerNetworkingNotes): three‑tier vs collapsed‑core vs spine‑leaf, **north‑south vs east‑west**, **STP‑blocked links** vs **L3 + ECMP**, horizontal vs vertical scaling, leaf=ToR/spine=core, BGP/OSPF/IS‑IS underlay + VXLAN/EVPN overlay, leaf→spine→leaf predictable path.
- NIST **SP 800‑145** — cloud definitions (IaaS/PaaS/SaaS; private/public/hybrid/community).
- **Blueprint anchor:** CCNA 200‑301 v1.1 **1.2.a–f** (two‑tier, three‑tier, spine‑leaf, WAN, SOHO, on‑prem & cloud); devices from `1.1`; cabling in `1.3`; virtualization/cloud deep‑dive in Block 7; campus/DC/WAN design deepen in CCNP ENSLD/ENCOR/ENSDWI.

---

> **Remaining Block‑1 topics**, on your **go**, one at a time:
> - `1.3` **Cabling & media** — single‑mode/multimode fiber, copper (extends the optics family from `1.1.5`).
> - `1.4` **Interface & cable issues** — collisions, errors, duplex/speed mismatch.
>
> Tell me which, and I'll build it.
