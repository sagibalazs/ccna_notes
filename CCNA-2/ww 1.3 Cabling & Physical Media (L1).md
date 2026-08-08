# 1.3 · Cabling & Physical Media 🧵🔦

> **Block 1 — Physical Layer & Network Components (L1)** · CCNA 200‑301 **1.3.a–b**
> **A media/physics topic, not a device** — standard **14‑section study template**. This is the **cable side** of the physical layer: it extends the **transceiver/optics family** from `1.1.5` (the *port*) into the **medium itself** (the *cable* — copper, MMF, SMF) that the transceiver drives.

---

## 1 · Base Data 📇

| Field | Value |
|---|---|
| **Title** | Cabling & Physical Media |
| **Doc ID** | `1.3` |
| **Date / Era** | 2 Aug 2026 · Cat6A/Cat8 copper, OM4/OM5 & OS2 fiber, mGig, 400G optics |
| **Topic** | The **physical medium** signals travel on — **copper (twisted pair)** and **fiber (single‑mode / multimode)** — and how devices **connect** (shared vs point‑to‑point) |
| **Scope** | 🟢 CCNA 1.3.a (SMF, MMF, copper) + 1.3.b (Ethernet shared media & point‑to‑point) · 🔵 engineer: modal dispersion, attenuation, EMI/crosstalk, connector/grade selection |
| **Key physics** | Copper carries **electrical** signals (EMI‑prone, ~100 m); fiber carries **light** (immune, km) — and in fiber, **core size decides distance** via modal dispersion |
| **Extends** | `./devices/switch-l2.md` (`1.1.5`) — the **transceiver cages & SFP/QSFP family** that terminate these media · powers/PoE on copper from `1.1.h` |
| **Feeds** | `1.4` (interface & cable issues — duplex/speed/errors) builds directly on this |

---

## 2 · Visualization — the three media, cross‑section 🗺️

```
 COPPER (twisted pair)          MULTIMODE FIBER (MMF)          SINGLE-MODE FIBER (SMF)
 ┌─────────────────────┐        core 50/62.5 µm                core ~9 µm
 │ 4 twisted pairs      │        ┌───────────────┐             ┌───────────────┐
 │  ((twists cancel     │        │  ╲ ╱ ╲ ╱ ╲ ╱  │  many paths  │  ───────────  │  one path
 │    EMI/crosstalk))   │        │   ╳   ╳   ╳    │  (modes)     │               │  (straight)
 │ RJ-45 · ≤100 m       │        │  ╱ ╲ ╱ ╲ ╱ ╲  │  → modal     │  ───────────  │  → NO modal
 │ electrical signal    │        └───────────────┘  dispersion  └───────────────┘  dispersion
 │ carries PoE ⚡        │        cladding 125 µm · 850 nm        cladding 125 µm · 1310/1550 nm
 └─────────────────────┘        LED/VCSEL · aqua · ≤~400 m       laser · yellow · 10–80+ km

 ┌──────────────────────────── MEDIA AT A GLANCE ────────────────────────────┐
 │ Medium   Core    Source   λ (nm)     Reach        EMI     PoE   Cost        │
 │ Copper   —       electric —          ≤100 m       prone   YES   $           │
 │ MMF      50/62.5 LED/VCSEL 850       ≤~400 m      immune  no    $$          │
 │ SMF      ~9      laser     1310/1550 10–80+ km    immune  no    $$$         │
 └────────────────────────────────────────────────────────────────────────────┘

 CONNECTIONS (1.3.b):  shared media → one collision domain, half-duplex (hub, legacy)
                       point-to-point → full-duplex, no collisions (switch port ↔ device)
```

---

## 3 · TL;DR ⚡

- Three media dominate: **copper twisted pair**, **multimode fiber (MMF)**, and **single‑mode fiber (SMF)**. The choice is driven by **distance, bandwidth, environment (EMI), security, PoE, and cost**.
- **Copper** carries an **electrical** signal over **twisted pairs** (the twisting cancels interference), reaches **~100 m**, is **cheap**, and **carries PoE** — but is **EMI‑prone** and distance‑limited.
- **Fiber** carries **light** through a glass **core** (surrounded by **cladding**, held in by **total internal reflection**). It's **immune to EMI**, **secure**, and has **enormous bandwidth** — but **can't carry power (no PoE)** and costs more.
- 🔑 **The star relationship — core size decides fiber distance.** **MMF** has a **big core (50/62.5 µm)** → light takes **many paths (modes)** → the paths arrive at slightly different times → the pulse **spreads (modal dispersion)** → **short reach (~hundreds of m)**. **SMF** has a **tiny core (~9 µm)** → light takes **one straight path** → **no modal dispersion** → **kilometres**. *The whole SMF‑vs‑MMF distance gap is modal dispersion.*
- **Copper grades:** Cat5e (1G/100 m) → Cat6 (10G/55 m) → **Cat6A (10G/100 m)** → Cat8 (25/40G/30 m, DC). **Fiber grades:** MMF **OM3/OM4/OM5** (OM1/2 are legacy); SMF **OS2** (backbone/outdoor).
- **Connectors:** **LC** (small, modern SFP) · **SC** (square push‑pull) · **ST** (bayonet, legacy) · **MPO/MTP** (multi‑fiber, 40/100G parallel).
- 🔑 **Connections (1.3.b):** **shared media** (a hub/bus) = one **collision domain**, **half‑duplex**, CSMA/CD; **point‑to‑point** (a switch port to one device) = **full‑duplex**, no collisions. Modern LANs are point‑to‑point (from `1.1.5`) — which sets up the **duplex/speed** issues in `1.4`.

---

## 4 · Importance — the problem first 🎯

**The problem:** a signal — an electrical pulse or a flash of light — has to travel a **physical distance** from one device to another and arrive **intact enough to be read**. But the physical world fights back the whole way:

1. **Signals fade with distance (attenuation).** Every metre of cable saps some signal. Go too far and the receiver can't tell a 1 from a 0. → the medium's **maximum reach** is a hard limit, and different media fade at very different rates.
2. **The world is full of electrical noise (EMI).** Motors, fluorescent lights, power cables, and even the wire's own neighbours induce interference into copper. → copper must **twist its pairs** (and sometimes **shield** them) to cancel noise, and where noise is severe, you need a medium that **doesn't care about it at all** → fiber.
3. **Bandwidth has a ceiling per medium.** Copper's electrical signalling tops out in the low GHz; light runs at **hundreds of THz**. → high‑speed/long‑haul links need **fiber**, not copper.
4. **Distance and cost pull against each other.** Copper and MMF are cheap but short; SMF reaches for kilometres but its **laser transceivers cost more**. → you pick the **cheapest medium that reliably covers the distance**.
5. **Some links must carry power, not just data.** APs and phones need **PoE**, and only **copper** carries it. → the medium choice can be forced by power, not just data.

The cable is not a passive afterthought — it's **part of the circuit**. Choose the wrong medium and you get a link that won't come up, runs at the wrong speed, drops packets under electrical noise, can't reach the far end, or can't power the device. Cabling is where all the fast silicon of `1.1` either connects to the world reliably… or doesn't.

🔵 **Why the depth?** Because the single most common physical‑layer question — *"why won't this link come up / why is it slow / why does it error"* — usually traces to a **media mismatch**: wrong fiber grade, wrong transceiver for the fiber, too‑long copper, EMI on an unshielded run, or MMF where SMF was needed. Understanding the **physics of the medium** is what lets you diagnose it in seconds instead of swapping cables blindly.

---

## 5 · Description 📋

**Copper (twisted pair)** 🟢
- Four **twisted pairs** in a jacket; twisting cancels **EMI and crosstalk**.
- **UTP** (unshielded) vs **STP/FTP** (shielded — for noisy environments).
- **RJ‑45** connector; **~100 m** max; carries **PoE** (`1.1.h`).
- **Categories** (bandwidth/speed climb): Cat5e → Cat6 → **Cat6A** → Cat8.

**Fiber (glass core + cladding)** 🟢
- Light travels in the **core**, confined by the **cladding** (lower refractive index) via **total internal reflection**. Notation **core/cladding** (e.g. **9/125, 50/125, 62.5/125** µm — cladding is always 125 µm).
- **Single‑mode (SMF)** — **~9 µm** core, **laser**, **1310/1550 nm**, grades **OS1** (indoor) / **OS2** (outdoor/backbone), **yellow** jacket, **10–80+ km**.
- **Multimode (MMF)** — **50 µm** (OM3/4/5) or **62.5 µm** (OM1/2) core, **LED/VCSEL**, **850 nm**, grades **OM1–OM5**, **≤~400 m**.

**Connectors** 🟢🔵 — **LC** (small, duplex, modern SFP) · **SC** (square push‑pull) · **ST** (bayonet, legacy) · **MPO/MTP** (8/12/24‑fiber, parallel 40/100G). Polish: **UPC** (flat) vs **APC** (8° angled, green, lower reflection).

**Connections (1.3.b)** 🟢 — **shared media** (many devices share one medium → collisions) vs **point‑to‑point** (a dedicated link between two devices → full‑duplex).

---

## 6 · How It Works — the physics & the relationships ⚙️

### 6.1 How light travels in fiber — and why core size sets distance 🔑
```
CLADDING (index n2, lower)
════════════════════════════
CORE (index n1, higher)   ← light bounces along, trapped by TOTAL INTERNAL REFLECTION
════════════════════════════
CLADDING

MULTIMODE (big core, 50/62.5 µm):        SINGLE-MODE (tiny core, ~9 µm):
 many angles fit → MANY PATHS (modes)     only one angle fits → ONE PATH
 each mode travels a different length      all light travels the same length
 → they arrive at different times          → no spreading
 → the pulse SPREADS = MODAL DISPERSION    → NO modal dispersion
 → limits distance/speed (~hundreds of m)  → kilometres, huge bandwidth
```
🔑 **This is the whole SMF‑vs‑MMF story.** A **bigger core** lets light in at **many angles**; each angle (**mode**) zig‑zags a **different total distance**, so a single pulse **smears out** over the run (**modal dispersion**) until pulses overlap and become unreadable. A **tiny core** admits **one mode** → the pulse stays sharp → it can run **far**. *Core size → number of modes → modal dispersion → maximum distance.*

### 6.2 How copper carries a signal — and why it's twisted 🔵
Copper sends a **differential electrical** signal on a pair (voltage difference between the two wires). External noise (EMI) hits **both** wires roughly equally, so the *difference* is preserved — and **twisting** the pair ensures each wire spends equal time in every position, making the induced noise **cancel**. Tighter twists and higher **category** = better cancellation = higher usable bandwidth. **Shielding** (STP/FTP) adds a foil/braid for hostile environments. *Twisting is noise‑cancellation built into the geometry.*

### 6.3 Attenuation & the distance limit 🔵
Every medium **attenuates** (weakens) the signal per unit length — copper strongly, MMF moderately (~3 dB/km at 850 nm), SMF very little (~0.2 dB/km at 1550 nm). When the signal drops below what the receiver can distinguish from noise, the link fails. *Attenuation per km × distance = why each medium has a max reach* (and why SMF, with the lowest attenuation, goes farthest).

### 6.4 Shared media vs point‑to‑point (1.3.b) 🟢
```
SHARED MEDIA (hub / old coax bus):        POINT-TO-POINT (switch port ↔ device):
  many devices on ONE medium                two endpoints, a DEDICATED link
  → only one can talk at a time             → both can talk at once
  → COLLISIONS → CSMA/CD → HALF-DUPLEX      → NO collisions → FULL-DUPLEX
  → one collision domain                    → its own collision domain (of one)
```
Modern switched LANs are **point‑to‑point, full‑duplex** (each switch port is its own collision domain, from `1.1.5`). Shared/half‑duplex survives only in legacy or specific media. *This distinction is exactly what the duplex/speed‑mismatch troubleshooting in `1.4` is about.*

### 6.5 ⚡ Media relationships (the "why" behind the choices) 🔑
> The cable is part of the circuit; these relationships decide which cable.

**① Core size ↔ modes ↔ modal dispersion ↔ distance.** (§6.1) The master fiber relationship — MMF's big core trades distance for cheap optics; SMF's tiny core buys kilometres. *Distance is bought with a smaller core (and a pricier laser).*

**② Light source ↔ media ↔ cost.** MMF uses cheap **LEDs/VCSELs**; SMF needs a precise **laser** aimed into a 9 µm core → **SMF transceivers cost more**. This — not the glass — is the real SMF vs MMF cost driver. *The transceiver, not the fibre, is where SMF costs you.*

**③ Wavelength ↔ attenuation ↔ reach.** Longer wavelengths (**1550 nm**) attenuate **less** than short (**850 nm**) → SMF's long wavelengths enable long haul; MMF's 850 nm suits short runs. *The colour of the light is a reach decision.*

**④ EMI ↔ copper vs fiber.** Copper is **electrical** → susceptible to interference (mitigated by twisting/shielding, but never immune). Fiber is **light** → **totally immune** to EMI and emits nothing to tap. *In electrically noisy or high‑security environments, fiber isn't better — it's the only honest choice.*

**⑤ Twisting/category ↔ crosstalk & bandwidth.** More/tighter twists and higher **Cat** = better crosstalk cancellation = higher usable frequency = more speed over the same 100 m. *Copper speed is a manufacturing‑quality relationship.*

**⑥ Bandwidth ceiling ↔ medium physics.** Copper's electrical signalling caps in the low GHz; light runs at **hundreds of THz**. *Beyond a point, no copper category can match fiber — it's a physics wall, not a quality gap.*

**⑦ Distance ↔ cost (the real design call).** Pick the **cheapest medium that reliably covers the distance**: copper for ≤100 m access, MMF for intra‑building/DC ≤~400 m, **SMF (OS2)** for backbone/inter‑building/outdoor where you can't easily re‑cable. *For hard‑to‑replace backbone, SMF is the conservative long‑term bet even if today's link is short.*

**⑧ PoE ↔ copper only.** Only copper carries **power** (`1.1.h`). A device that needs PoE **must** be on copper — fiber can carry its data but never its watts. *Power can force the medium regardless of distance/bandwidth.*

**⑨ Transceiver ↔ fiber match.** An **SMF transceiver won't link over MMF** (and vice‑versa) — the optics are built for one core. The **cage/form factor** (SFP/QSFP, from `1.1.5`) *and* the **fiber grade** must both match the reach/speed. *A link needs the transceiver, the fibre grade, and the reach all agreeing.*

---

## 7 · Pros & Cons ⚖️

| Aspect | **Copper (UTP)** | **Multimode fiber** | **Single‑mode fiber** |
|---|---|---|---|
| **Reach** | ≤100 m | ≤~400 m | 10–80+ km |
| **Bandwidth** | GHz‑limited | high | highest (≈unlimited) |
| **EMI immunity** | ❌ (twist/shield) | ✅ | ✅ |
| **Security (tap)** | easier to tap | hard | hard |
| **PoE** | ✅ carries power | ❌ | ❌ |
| **Transceiver cost** | cheapest | moderate | highest (laser) |
| **Best for** | access/endpoints | intra‑building / DC | backbone / campus / outdoor |
| **Fragility** | robust | bend‑sensitive | bend‑sensitive |

---

## 8 · Variants — grades, connectors, connections 🌿

**Copper categories** (bandwidth/speed climb):

| Cat | Speed @ distance | Bandwidth | Notes |
|---|---|---|---|
| **Cat5e** | 1G @ 100 m | 100 MHz | baseline; still common |
| **Cat6** | 1G @ 100 m · **10G @ 55 m** | 250 MHz | 10G only on short runs |
| **Cat6A** | **10G @ 100 m** | 500 MHz | 10G standard for new work |
| **Cat7** | (10G, shielded) | 600 MHz | non‑RJ45 variants; largely marketing/niche |
| **Cat8** | **25/40G @ 30 m** | 2000 MHz | data‑center short runs only |

**Fiber grades:**

| Grade | Type | Core | Jacket | Typical use |
|---|---|---|---|---|
| **OM1/OM2** | MMF | 62.5/50 µm | orange | legacy (≤1G) — avoid for new |
| **OM3** | MMF | 50 µm | aqua | 10G@300 m, 40/100G short |
| **OM4** | MMF | 50 µm | aqua/violet | 10G@400 m, 40/100G@150 m |
| **OM5** | MMF | 50 µm | lime green | SWDM wideband, 400G |
| **OS1** | SMF | ~9 µm | yellow | indoor single‑mode |
| **OS2** | SMF | ~9 µm | yellow | outdoor/backbone, long haul |

**Connectors:** **LC** (small duplex, modern SFP) · **SC** (square) · **ST** (bayonet, legacy) · **MPO/MTP** (8/12/24‑fiber, 40/100G parallel). Polish: **UPC** (flat, blue) vs **APC** (8° angled, green).

**Copper pinouts:** **straight‑through** (T568A↔A or B↔B, unlike devices) vs **crossover** (A↔B, like devices) — largely automated today by **Auto‑MDIX**.

**Connections (1.3.b):** **shared media** (half‑duplex, collision domain) vs **point‑to‑point** (full‑duplex).

---

## 9 · Devices / Media / Protocols 🔌

| Item | Type | Role |
|---|---|---|
| **UTP/STP twisted pair** | Media | Copper, ≤100 m, PoE‑capable |
| **MMF / SMF** | Media | Fiber, ≤~400 m / km‑scale |
| **RJ‑45** | Connector | Copper termination |
| **LC / SC / ST / MPO** | Connectors | Fiber termination |
| **Transceiver (SFP/SFP+/QSFP)** | Device | Drives the medium (home: `1.1.5`) |
| **1000BASE‑T / 10GBASE‑T** | Standard | Gigabit / 10G over copper |
| **1000BASE‑SX / 10GBASE‑SR** | Standard | Over MMF (850 nm) |
| **1000BASE‑LX / 10GBASE‑LR** | Standard | Over SMF (1310 nm) |
| **10GBASE‑ER/ZR** | Standard | Long‑haul SMF (1550 nm) |
| **Auto‑MDIX** | Feature | Auto straight/crossover |
| **DAC / AOC** | Media | Short direct‑attach (rack/stack, from `1.1.5`) |

---

## 10 · Best Practices ✅

- **Pick the cheapest medium that reliably covers the distance** — copper ≤100 m access, MMF for intra‑building/DC, **OS2 SMF for backbone/outdoor** (§6.5 ⑦).
- **Install Cat6A for new copper** so you have a 10G path; reserve Cat8 for short DC runs (§8).
- **Use OM4 (or OM5) for new MMF**; never install OM1/OM2 — they can't do 10G at useful distances (§8).
- **Choose SMF (OS2) for anything hard to re‑cable** (risers, inter‑building, outdoor) even if today's speed is modest — it's the conservative long‑term bet (§6.5 ⑦).
- **Match transceiver ↔ fiber ↔ reach exactly** (SMF optic on SMF, SR on MMF, LR on SMF) and the form factor to the port cage (§6.5 ⑨; `1.1.5`).
- **Use fiber where EMI is severe, distances are long, or security matters** (§6.5 ④).
- **Keep copper on copper for PoE** — a device needing power must be on twisted pair (§6.5 ⑧).
- **Respect bend radius and label jackets by colour** (yellow=SMF, aqua=OM3/4, lime=OM5) to prevent mismatches.

---

## 11 · No‑Goes 🚫

- ❌ **Plugging an SMF transceiver onto MMF (or vice‑versa).** The optics are built for one core — the link won't come up or errors badly (§6.5 ⑨).
- ❌ **Expecting 10G over Cat6 at 100 m.** Cat6 does 10G only to ~55 m; use **Cat6A** for 10G@100 m (§8).
- ❌ **Installing OM1/OM2 for new work.** Legacy; can't carry modern speeds at distance (§8).
- ❌ **Running copper past 100 m.** Attenuation kills it — use fiber or a repeater/switch in between (§6.3).
- ❌ **Unshielded copper through heavy EMI** (next to motors/power) — errors and instability; shield or go fiber (§6.5 ④).
- ❌ **Assuming fiber carries PoE.** It carries data only — powered devices need copper (§6.5 ⑧).
- ❌ **Mismatching connector/polish** (UPC vs APC, or wrong connector) — high loss/reflection.
- ❌ **Ignoring the shared‑vs‑point‑to‑point distinction** — it's the root of the duplex/speed issues you'll troubleshoot in `1.4`.

---

## 12 · Terms 📖

- **Twisted pair (UTP/STP)** — copper LAN cable; twisting cancels noise. 🔗 [ref](https://www.netstuts.com/fiber-vs-copper) · `./terms/twisted-pair.md`
- **Cable categories (Cat5e–Cat8)** — copper bandwidth/speed grades. 🔗 [ref](https://universalnetworkcable.com/) · `./terms/copper-categories.md`
- **Single‑mode fiber (SMF)** — ~9 µm core, laser, long haul. 🔗 [ref](https://us.elfcams.com/article/type-fibre) · `./terms/smf.md`
- **Multimode fiber (MMF)** — 50/62.5 µm core, LED/VCSEL, short reach. 🔗 [ref](https://dc.mynetworkinsights.com/single-mode-fiber-and-multi-mode-fiber/) · `./terms/mmf.md`
- **Core / cladding / total internal reflection** — how fiber traps light. 🔗 [ref](https://us.elfcams.com/article/type-fibre) · `./terms/fiber-core-cladding.md`
- **Modal dispersion** — pulse spreading from multiple modes (MMF). 🔗 [ref](https://www.netstuts.com/fiber-vs-copper) · `./terms/modal-dispersion.md`
- **Attenuation** — signal loss per distance. 🔗 [ref](https://seetronic.com/blog/optical-fiber-types-single%E2%80%91mode-vs-multimode/) · `./terms/attenuation.md`
- **OM1–OM5 grades** — multimode fiber grades. 🔗 [ref](https://speedtesthq.com/guides/cables/fiber-os1-os2-om3-om4-om5) · `./terms/om-grades.md`
- **OS1 / OS2 grades** — single‑mode fiber grades. 🔗 [ref](https://speedtesthq.com/guides/cables/fiber-os1-os2-om3-om4-om5) · `./terms/os-grades.md`
- **Wavelength (850/1310/1550 nm)** — light "colour" ↔ reach. 🔗 [ref](https://seetronic.com/blog/optical-fiber-types-single%E2%80%91mode-vs-multimode/) · `./terms/wavelength.md`
- **EMI / crosstalk** — interference copper must fight. 🔗 [ref](https://www.netstuts.com/fiber-vs-copper) · `./terms/emi-crosstalk.md`
- **Connectors (LC/SC/ST/MPO)** — fiber terminations. 🔗 [Cisco optics](https://www.cisco.com/c/en/us/products/interfaces-modules/transceiver-modules/index.html) · `./terms/fiber-connectors.md`
- **UPC / APC polish** — flat vs angled ferrule. 🔗 [ref](https://us.elfcams.com/article/type-fibre) · `./terms/upc-apc.md`
- **Auto‑MDIX / straight‑through / crossover** — copper pinout. 🔗 [Cisco](https://www.cisco.com) · `./terms/mdix-crossover.md`
- **Shared media vs point‑to‑point** — collision domain vs dedicated link. 🔗 [Cisco](https://www.cisco.com) · `./terms/shared-vs-p2p.md`
- **BASE‑T / SX / LX / SR / LR / ER** — Ethernet media standards. 🔗 [IEEE 802.3](https://www.ieee802.org/3/) · `./terms/ethernet-media-standards.md`

---

## 13 · Practical Tasks 🧪

1. **Media selection drill** 🔑 — for (a) a desk PC 40 m away, (b) two DC racks 30 m apart at 100G, (c) two buildings 3 km apart, pick copper/MMF/SMF + grade + connector, and justify via distance/EMI/cost (§6.5).
2. **Read the jacket** 🟢 — identify fiber grade by jacket colour (yellow=SMF, aqua=OM3/4, lime=OM5, orange=OM1/2) and copper by category print; note core/cladding notation (9/125 vs 50/125).
3. **Modal‑dispersion reasoning** 🔵 — explain in your own words why a 50 µm core caps distance and a 9 µm core doesn't; relate it to why 40/100G MMF reaches only ~100–150 m (§6.1).
4. **Transceiver ↔ fiber match** 🔵 — given an SR (MMF) and an LR (SMF) SFP+ and two fiber runs, pair them correctly and predict what happens if swapped (§6.5 ⑨; `1.1.5`).
5. **Copper limit test** 🟢 — reason about a 10G link over Cat6 at 80 m vs Cat6A at 80 m; which links, which errors, and why (§8).
6. **Shared vs point‑to‑point** 🟢 — in Packet Tracer, connect PCs via a **hub** (observe shared/half‑duplex/collisions) vs a **switch** (full‑duplex, no collisions); sets up `1.4` (§6.4).
7. **PoE forces the medium** 🔵 — a ceiling AP 60 m away needs power and data: which medium must you use and why can't fiber do it alone? (§6.5 ⑧; `1.1.h`).

---

## 14 · Sources 📚

- Fiber references (Seetronic, SpeedTestHQ, FiberCablesDirect, NetsTuts, Elfcam, MyNetworkInsights, UniversalNetworkCable): **SMF ~9 µm/laser/1310–1550 nm/OS1–OS2** (~0.2 dB/km @1550), **MMF 50–62.5 µm/LED‑VCSEL/850 nm/OM1–OM5** (~3 dB/km), **modal dispersion** from multiple modes, OM3 10G@300 m / OM4 @400 m / OM5 SWDM, cladding 125 µm, jacket colours, **total internal reflection**, connectors **LC/SC/ST/MPO**, UPC/APC.
- Copper references (UniversalNetworkCable, SpeedTestHQ): **Cat5e/Cat6/Cat6A/Cat8** speeds & distances, twisting/crosstalk, shielding, ~100 m limit; "Cat7 is largely marketing."
- IEEE **802.3** media standards (1000BASE‑T/SX/LX, 10GBASE‑T/SR/LR/ER); transceiver/optics family cross‑ref `1.1.5`.
- **Blueprint anchor:** CCNA 200‑301 v1.1 **1.3.a** (SMF/MMF/copper) & **1.3.b** (shared media & point‑to‑point); transceivers/cages in `1.1.5`; PoE (copper power) in `1.1.h`; duplex/speed/error troubleshooting in `1.4`.

---

> **Last Block‑1 topic**, on your **go**:
> - `1.4` **Interface & cable issues** — collisions, (CRC/input) errors, and **duplex/speed mismatch** — the physical‑layer troubleshooting leaf that builds on this one (`1.3`) and the NIC/switch/duplex concepts from `1.1.1`/`1.1.5`.
>
> Tell me when, and I'll build it.
