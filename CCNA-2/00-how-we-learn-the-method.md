# 📘 00 · How We Learn — The Method

> The operating manual for this entire study system. **Every topic document obeys the rules below.**
> This file is the framework itself, so it *describes* the 14-section template rather than following it — a style guide isn't written in the style it prescribes.
> **Date:** 2026-08-01 · **Goal:** CCNA 200-301 (target ~31 Aug 2026) → CCNP, learned at real engineer depth.

---

## 🎯 Mission & Philosophy

- 🧠 **Learn it once — deeply and correctly.** No shallow pass we have to redo later. If we meet a topic, we meet all of it that matters.
- 🏗️ **Engineer / university level from first contact** — real PDU structures dissected bit by bit, real practice on real-ish devices (Packet Tracer 8 / GNS3).
- 🔬 The goal is **not the right answer** — it's understanding **what happens under the hood**, so you can make decisions *and justify them* on your own. That's the level above "passed the exam."
- 🗣️ **Mastery test:** you can explain the topic to another person, **point by point**, straight from the document's structure.
- ✅ **Every technical claim is verified** against trusted, professional sources before it lands — official Cisco/vendor docs, RFCs, IEEE, archive.org / libraries, reputable forums. **No baked-in errors.**
- ✋ **Never start writing a topic's content until the learner names the topic and gives the go-ahead.** Plan first, confirm, then build.

---

## 🧱 The Obligatory 14-Section Template

Every **topic** document has exactly these sections, in this order. Non-negotiable.

| # | Section | Purpose |
|---|---|---|
| 1 | **Base data** | Title, date, topic & exam-scope tag. What am I looking at and how much of it is on my exam? |
| 2 | **Visualization** 👁️ | The at-a-glance overview — diagram and/or overview table. The whole big picture with **zero scrolling** (visual-learner first). |
| 3 | **TL;DR** ⚡ | The essentials in short but *complete* form, for when the visual alone isn't enough. |
| 4 | **Importance** 🎯 | **State the underlying problem/task FIRST.** Once the problem is clear, the solution becomes obvious — then why it matters for exam + job. |
| 5 | **Description** 📝 | What it actually is, in bullets. |
| 6 | **How it works** ⚙️ | Step by step, under the hood — bit-level, packet-flow, the real mechanics. |
| 7 | **Pros & Cons** ⚖️ | Honest trade-offs. |
| 8 | **Variants** 🔀 | Common → specific. |
| 9 | **Devices / media / protocols** 🖧 | Who operates here — each linked to its own doc. |
| 10 | **Best practices** 🌟 | How engineers actually use / deploy it. |
| 11 | **No-Goes** ❌ | Common mistakes **+ troubleshooting** — what breaks and how to spot it. |
| 12 | **Terms** 📖 | Every term used: short definition + **external** authoritative link + **internal** `./terms/…` link (own doc, finished later). |
| 13 | **Practical Tasks** 🛠️ | Hands-on exercises that cement the topic (Wireshark, lab, CLI). Listed briefly here; each expanded in detail later, in context. |
| 14 | **Sources** 📚 | Where every claim came from. |

> **Why this order:** *see it* (2) → *grasp it fast* (3) → *understand why it exists* (4) → *learn it* (5–9) → *use it well* (10–11) → *own the vocabulary* (12) → *do it with your hands* (13) → *trust it* (14).

---

## 🎨 Style Rules

- ✍️ **Markdown**, with **emojis & symbols** for colour and memorability — this is built for a visual learner, so the page should be pleasant and sticky to read.
- 📐 **Simple ASCII diagrams** — up front, and anywhere a picture beats a paragraph.
- 🔬 **Bit-level header / PDU dissection** and **packet-flow walkthroughs** — always show the real structure, not a hand-wave.
- 🧬 **Evolution / "why it exists" framing** — every technology solves a prior pain; name the pain.
- 🕵️ A **real-world "dirty secrets" layer** — the messy truth practitioners actually deal with.
- 🏷️ **Tag any depth beyond current exam scope** (using the markers below) so the exam boundary is always visible while we still go deep.
- 📄 **One topic per document, self-contained, in its correct home.** No mixing topics, no duplicate/old/v2 files. Keep the *structure* whole but the *content* scoped — right depth, not sprawl.

---

## 🏷️ Cert Marker Legend (protocols)

Applied to **protocols** so scope is visible at a glance:

- 🟢 **CCNA** 200-301 — learn now, baseline
- 🔵 **CCNP ENCOR** 350-401 — core
- 🟣 **CCNP ENARSI** 300-410 — routing/services deep-dive
- 🟠 **CCNP ENSLD / ENSDWI** 300-420 / 300-415 — design / SD-WAN
- ⚪ **Context-only** — shown so the picture is whole, not a study target itself

**Double-dot** (e.g. 🟢🟣) = *introduced* at the first tier, *deepened* at the second; the protocol's own leaf doc carries the depth ladder. **Devices are linked but not marked** — they're universal.

---

## 🗂️ File & Link Conventions

- **Category folders, relative links, kebab-case:**
  - `./devices/router.md`
  - `./protocols/ospf.md`
  - `./terms/encapsulation.md`
- Every **device / protocol / term** named in a doc is a **link to its own future file**.
- **Hub-and-leaf model:** a topic doc's Visualization / Devices / Terms sections form the *hub*; each link is a *leaf* we build later. Opening the hub gives the map; the leaves give the depth.

---

## 🧗 Curriculum Sequence (zero → hero, an OSI climb)

Built strictly in this order so **nothing ever depends on an untaught topic**:

```
 Block 0  Models · OSI · PDU          ← the map (you are here)
 Block 1  L1 Physical · Devices
 Block 2  L2 Ethernet · Switching
 Block 3  L3 Addressing
 Block 4  L3 Routing
 Block 5  L4 Transport
 Block 6  IP Services
 Block 7  Virtualization · Cloud
 Block 8  Wireless
 Block 9  Security
 Block 10 Automation · Programmability
```

---

## ✅ Definition of Done (per topic doc)

- [ ] All **14 sections** present, in order.
- [ ] Every claim **sourced**; any **beyond-scope depth tagged** with a marker.
- [ ] Devices / protocols / terms **linked** with the folder + kebab convention.
- [ ] Lives in its **correct block**; no duplicate or older version left behind.
- [ ] Passes the **mastery test**: could I teach this, point by point, from the doc alone?

---

## 📚 References

- Cisco Learning Network — official exam blueprints (CCNA 200-301 v1.1; CCNP ENCOR 350-401, ENARSI 300-410, ENSLD 300-420, ENSDWI 300-415). Used for scope tagging and block sequencing.
