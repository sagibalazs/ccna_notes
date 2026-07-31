

# Interface and Cable Issues

# NE — CCNA Preparing — Interface & Cable Issues

Below is a compact “matrix” of Ethernet interface types vs. their most common issues (ranked from most to least common in day-to-day CCNA troubleshooting). After the table, we start with the first interface type and walk through symptoms, how to detect them on IOS, root causes, and fixes.

## Overview matrix — interface types × typical issues

|Interface / media|Most common issues (→ descending)|Why it happens|What you’ll see on the device|Primary fix|
|---|---|---|---|---|
|Copper Ethernet, point-to-point (switch↔host / switch↔switch)|Duplex mismatch → speed mismatch → CRC/input errors → runts/giants|Manual speed/duplex on one side vs auto on the other; bad/long/EMI-noisy cable; wrong MTU|Port shows up/up but “a-half / a-100” on one end; counters: late collisions, collisions, CRC, input errors, runts/giants|Make both ends `speed auto duplex auto` or explicitly match; replace/shorten cable; remove EMI; align MTU end-to-end.|
|Copper Ethernet via hub (shared media)|Collisions (normal), runts, CRC|Hubs repeat signals; CSMA/CD is active; simultaneous sends collide|Rising collisions counter (normal in half-duplex), occasional runts/CRC|Keep half-duplex where a hub exists; migrate to switches to eliminate shared media.|
|Multimode fiber (MMF)|CRC/input errors; “no/losing carrier” (light-level/dirty connector); speed/duplex mismatch on copper SFP peer|Dirty ends, tight bends, wrong transceiver/wavelength or mismatched speed on the copper side of a media converter|Input errors/CRC without collisions; link flaps|Clean connectors, verify transceiver & wavelength, respect bend radius; match speed/duplex across media converters/SFPs. (CRC/input-error meaning and mismatch detection are same as copper.)|
|Single-mode fiber (SMF)|As with MMF plus distance/optic mismatch (LX/SX)|Too-long runs for the optic; wrong optic type|Intermittent up/down; input errors without collisions|Correct optic for distance; add attenuator if receiver is saturating; proper patch type. (Use same counter logic as below.)|
|Any Ethernet|Giants / runts / framing errors|MTU mismatch (giants); collisions or faulty NIC/cable (runts, frame errors)|Giants, runts, “frame” in `show interfaces`|Align MTU on path; fix duplex; replace bad NIC/cable.|

> Key counter meanings (IOS `show interfaces`): runts, giants, input errors, CRC, frame, collisions, late collisions; late collisions are a classic duplex-mismatch tell.
> 
> 1. CCNA 200-301 Official Cert G…
> 
> 2. CCNA 200-301 Official Cert G…

---

## Interface type 1: Copper Ethernet (point-to-point)

### 1) Duplex mismatch (the #1 real-world culprit)

**What happens.** One end is forced `100/full`, the other is left to autonegotiate. The auto side can sense speed but must fall back to **half-duplex**, creating a mismatch (full vs half). The link stays **up/up** and appears fine, but throughput is terrible and erratic because the half-duplex side runs CSMA/CD and “believes” collisions occur.

1.CCNA200-301OfficialCertGuide,…

1.CCNA200-301OfficialCertGuide,…

**How to discover (IOS).**

- `show interfaces <port> status` → look for `a-half` next to `a-100` or similar.
    
    1.CCNA200-301OfficialCertGuide,…
    
- `show interfaces <port>` repeatedly → **late collisions** rising on the half-duplex side; may also see collisions and CRC as secondary symptoms.
    
    1. CCNA 200-301 Official Cert G…
    
    2. CCNA 200-301 Official Cert G…
    

**Why these symptoms.** With half-duplex, collisions are part of CSMA/CD; when they occur **after byte 64** they are **late collisions**, strongly indicating a duplex mismatch, not normal shared-media behavior.

1. CCNA 200-301 Official Cert G…

2. CCNA 200-301 Official Cert G…

**Fix, step by step.**

1. On both ends set either `speed auto` `duplex auto` or explicitly match:
    
    `interface g0/1   speed 100   duplex full`
    
2. Re-check with `show interfaces status` (both sides should show `a-full a-100` or explicit match).
    
3. Watch `show interfaces` — late collisions should stop increasing; CRC/input-errors should stabilize.
    
    1.CCNA200-301OfficialCertGuide,…
    
    1. CCNA 200-301 Official Cert G…
    

### 2) Speed mismatch

**What happens.** Ends are set to different speeds (e.g., 10 vs 100). Result: interface typically goes **down/down** or **notconnect**; far easier to spot than duplex issues.

1.CCNA200-301OfficialCertGuide,…

**Discovery.**

- `show interfaces status` → one or both ends show `notconnect` or `err-disabled` scenarios.
    
- Cabling OK? If the other side is hard-set, match it; otherwise use autonegotiation.
    

**Fix.** Align speed on both ends (or use auto on both). Then verify the neighbor view as well.

### 3) CRC / input errors without collisions

**What happens.** The receiver discards frames that fail FCS/CRC due to bit errors—often caused by **noisy/bad/too-long cable**, EMI (power cables), kinked patch leads, or dirty copper pairs. Interference triggers **CRC** and **input errors**, even when the link is up/up.

1. CCNA 200-301 Official Cert G…

2. CCNA 200-301 Official Cert G…

**Discovery.**

- `show interfaces <port>` → watch **CRC** and **input errors** grow while **collisions stay flat** (points to physical layer noise rather than duplex).
    
    1. CCNA 200-301 Official Cert G…
    
- Clear counters, generate traffic, re-check.
    

**Fix.**

- Replace the patch, shorten the run, re-terminate ends, route away from electrical noise, test with a certifier. If still rising, try another port/NIC. (CRC remediation guidance aligns with standard practice.)
    
    1. CCNA 200-301 Official Cert G…
    

### 4) Runts / giants / frame errors

**What happens.**

- **Runts** (<64 bytes) often from collisions or faulty NICs/buffer underruns.
    
- **Giants** (>1518 bytes classic Ethernet) from MTU/“jumbo” mismatches or bad NICs.
    
- **Frame** (illegal format/partial byte) usually accompanies collisions or bad hardware.
    
    1. CCNA 200-301 Official Cert G…
    

**Discovery.**

- `show interfaces` → counters: **runts**, **giants**, **frame**.
    
- If you see **giants** and you are using jumbo frames, verify MTU end-to-end. If you see **runts** with rising collisions, investigate duplex/shared-media; otherwise suspect a bad NIC/cable.
    
    Todd Lammle - CompTIA Network+ …
    
    Todd Lammle - CompTIA Network+ …
    

**Fix.**

- Align MTU across the entire path or disable jumbo at the source; fix duplex; replace NIC/cable as indicated.
    
    Todd Lammle - CompTIA Network+ …
    

---

## Interface type 2: Copper via hub (shared media)

**Context.** Hubs repeat electrical signals; multiple sends collide; half-duplex/CSMA-CD is mandatory. Collisions are expected; switches eliminate this by queuing.

1. CCNA 200-301 Official Cert G…

**Discovery & expectations.**

- `show interfaces` → **collisions** will increment; some **runts/CRC** may appear; this is not necessarily a fault if a hub truly exists.
    
    1. CCNA 200-301 Official Cert G…
    

**Fix.**

- Keep connected interfaces at **half-duplex** when a hub is present. Best practice is to migrate to switches (point-to-point, full-duplex) to eliminate shared bandwidth and collisions entirely.
    
    1. CCNA 200-301 Official Cert G…
    

---

## Interface type 3–4: Fiber (MMF/SMF)

**What carries over.** The same Layer-1 counters (**CRC, input errors, frame**) apply to fiber interfaces; rising CRC/input-errors without collisions point to **optical issues**: dirty connectors, bend radius violations, wrong optics/wavelength, over/under optical power. The investigation pattern is identical to copper for counters, while remediation is optic-specific (clean, re-seat, correct optic & patch, respect distance).

1. CCNA 200-301 Official Cert G…

---

## Fast “how-to” checklist (IOS)

1. **See if it’s a mismatch first**
    
    - `show interfaces status` → look for `a-half` or odd `a-10/a-100` on a gig link.
        
        1.CCNA200-301OfficialCertGuide,…
        
    - If mismatch suspected, set both ends to **auto** or explicitly match speed/duplex.
        
        1.CCNA200-301OfficialCertGuide,…
        
2. **Read the counters**
    
    - `show interfaces <port>` (twice, 30–60s apart) → late collisions (duplex issue), CRC/input errors (physical noise), runts/giants/frame (collisions/MTU/NIC).
        
        1. CCNA 200-301 Official Cert G…
        
3. **Eliminate cable/optic causes**
    
    - Swap patch, new port/NIC/SFP, clean fiber, verify MTU & optic type; check EMI/bend radius. CRC/input errors with **no collisions** scream cabling/optic interference rather than duplex.
        
        1. CCNA 200-301 Official Cert G…
        

If you want, next we can proceed interface-type by interface-type (MMF, SMF, then special cases like media converters and copper SFPs) with precise CLI sequences and lab mini-scenarios.


1. Cable Issues – Overview Matrix

| Cable type                    | Most common issues (descending)                             | Why it happens                                             | What you see (IOS / symptoms)                              | Primary fix                                              |
| ----------------------------- | ----------------------------------------------------------- | ---------------------------------------------------------- | ---------------------------------------------------------- | -------------------------------------------------------- |
| **UTP Copper (Cat5/5e/6/6a)** | Bad pair / open pair                                        | Broken conductor, bad crimp, cheap patch cable             | Link flaps, link down/down, “notconnect”, CRC/input errors | Replace cable; re-terminate; test with cable tester      |
|                               | Split pair                                                  | Pairs reversed; correct continuity but wrong twist pairing | High CRC, late collisions, terrible throughput             | Replace cable; fix termination; cable tester will detect |
|                               | EMI / crosstalk                                             | Cable routed next to power, motors, fluorescent lights     | CRC/input errors without collisions                        | Re-route cable; shield; shorten                          |
|                               | Wrong cable type (straight vs crossover when autoneg fails) | Old devices not supporting auto-MDIX                       | Link down or only 10 Mbps                                  | Use correct cable or enable auto-MDIX                    |
|                               | Overlength (>100 m)                                         | Attenuation too high                                       | CRC, drops, speed steps down                               | Shorten cable; add switch                                |
| **Fiber MMF**                 | Dirty connectors                                            | Dust, oil, fingerprints                                    | Link up/down flapping, input errors                        | Clean with fiber tool, re-seat                           |
|                               | Wrong wavelength (SX↔LX)                                    | Optics mismatch                                            | No link or heavy errors                                    | Correct transceiver pair                                 |
|                               | Bend radius exceeded                                        | Too tight bends reduce light level                         | CRC/input errors, link flaps                               | Re-route fiber, fix radius                               |
|                               | Wrong fiber type (OM1/2 vs OM3/4)                           | Modal dispersion                                           | Poor performance, link down                                | Match fiber to transceiver specs                         |
| **Fiber SMF**                 | Dirty endfaces                                              | Common cause #1                                            | Link up/down, CRC/input errors                             | Clean and re-seat                                        |
|                               | Distance mismatch                                           | Too long/too short (receiver saturation)                   | No link or heavy errors                                    | Correct optic / add attenuator                           |
|                               | Wrong connectors                                            | SC vs LC mismatch                                          | No physical link                                           | Match connectors                                         |
|                               | Damaged fiber                                               | Cracks, crush                                              | Link down                                                  | Replace cable                                            |
|                               |                                                             |                                                            |                                                            |                                                          |
|                               |                                                             |                                                            |                                                            |                                                          |
|                               |                                                             |                                                            |                                                            |                                                          |
|                               |                                                             |                                                            |                                                            |                                                          |

# 2. Copper Cable Issues — Detailed Troubleshooting

## Issue 1 — **Open Pair / Bad Pair (Most common)**

### What happens

One of the four twisted pairs is broken or poorly terminated. Ethernet needs specific pairs depending on speed (pair map is strict).

### Why

Bad crimp, cheap/old cable, mechanical stress, pulled cable.

### Symptoms

- Link **down/down** or **flapping**.
    
- Or link up, but **CRC/input errors** accumulate quickly.
    
- Low or unstable throughput.
    
- Sometimes interfaces fallback to **10 Mbps**.
    

### IOS Discovery

- `show interfaces <int>` → CRC, input errors grow.
    
- `show interfaces status` → `notconnect` or speed drop.
    
- Cable test (if supported):

```less
test cable-diagnostics tdr interface g0/1
show cable-diagnostics tdr interface g0/1
```

This identifies **open**, **short**, or **distance to fault**.

### Fix

- Replace cable.
    
- Re-crimp connectors.
    
- Avoid tight pull or pressure on the cable.

## Issue 2 — **Split Pair**

### What happens

Cable has continuity (so it “tests OK” with a basic tester), but the twisted pairs were split—e.g., pin 1 matches pin 2 electrically, but the twists belong to different pairs.

### Why

Incorrect termination by hand, cheap cables, wrong color pairing.

### Symptoms

- Link comes up, but throughput is extremely low.
    
- **CRC** and **late collisions** grow fast.
    
- Looks like duplex mismatch but persists even when duplex is correct.
    

### IOS Discovery

- CRC and input errors grow, but **no collisions** if full-duplex.
    
- Only a **TDR/certification tester** can reveal split pairs (simple continuity testers cannot).
    
- Interface may flap or autoneg may settle on 10 Mbps.
    

### Fix

- Replace cable with a proper factory-made one.
    
- Correctly terminate T568B or T568A on both ends.
    

---

## Issue 3 — **EMI / Crosstalk (Noise)**

### What happens

Ethernet frame bits get corrupted due to electromagnetic interference. The receiver fails CRC validation and drops frames.

### Why

- Cable routed next to power lines
    
- Electric motors, HVAC, elevator motors
    
- Fluorescent lighting
    
- Badly shielded cable bundles
    

### Symptoms

- **CRC** and **input errors** rise rapidly
    
- Throughput instability
    
- No collisions (full-duplex)
    
- Link remains **up/up**
    

### IOS Discovery

- `show interfaces <int>` → increasing CRC without collisions
    
- Clear counters, test again, errors reappear under load
    

### Fix

- Re-route UTP away from power lines
    
- Use shielded cables if needed
    
- Shorten cable runs
    
- Avoid tight cable bundles
    

---

## Issue 4 — **Wrong Cable Type (Straight vs Crossover when Auto-MDIX Fails)**

### What happens

Old devices or disabled auto-MDIX require the correct cable type.

### Why

Auto-MDIX is standard on modern switches, but not all old routers/switches/NICs.

### Symptoms

- Link **down/down**
    
- Or device negotiates **10 Mbps half-duplex**
    

### Discovery

- `show interface status` → “notconnect” even though the port is enabled
    
- Try manual speed/duplex; link still fails
    

### Fix

- Use crossover cable for switch↔switch, switch↔router if auto-MDIX unsupported
    
- Enable auto-MDIX if possible
    
- Replace very old NICs
    

---

## Issue 5 — **Exceeding Maximum Cable Length (>100 m)**

### What happens

Signal attenuation increases beyond what the receiver can decode.

### Why

Ethernet standard: 100 m (90 m horizontal + 2×5 m patch cables).

### Symptoms

- CRC/input errors
    
- Speed drop (e.g., 1G → 100M)
    
- Link flapping
    

### IOS Discovery

- Same as EMI: CRC keeps rising
    
- Only improves when cable is shortened
    

### Fix

- Keep total length ≤100 m
    
- Add a switch for regeneration
    
- Use fiber for long runs
    

---

# 3. Fiber Cable Issues — Detailed Troubleshooting

## Issue 1 — **Dirty / Contaminated Connectors (Most common)**

### What happens

Dust, oil, fingerprints on LC/SC connectors block or scatter light.

### Symptoms

- Link **up/down** flapping
    
- **Input errors**, CRC
    
- Sometimes no link at all
    

### IOS Discovery

- `show interfaces <int>` → input errors with no collisions
    
- Light levels on SFP (`show hw-module...` depending on platform)
    

### Fix

- Clean with proper fiber-cleaning tools
    
- Re-seat gently
    
- Never touch fiber endfaces with fingers

## Issue 2 — **Wrong Transceiver / Wavelength (SX ↔ LX, SMF ↔ MMF)**

### What happens

Optics designed for 850 nm (MMF) or 1310/1550 nm (SMF) mismatch.

### Symptoms

- Link never comes up
    
- Or extremely high errors
    

### Discovery

- Check SFP labels
    
- `show inventory` on Cisco
    
- Confirm patch type (OM3 vs OS2, etc.)
    

### Fix

- Match wavelength, connector type, fiber type
    
- Replace wrong optic
    

---

## Issue 3 — **Exceeding Bend Radius / Physical Damage**

### What happens

Bending fiber too tightly causes signal loss; crushing breaks the core.

### Symptoms

- CRC/input errors without collisions
    
- Link flaps or goes down entirely
    

### Fix

- Re-route fiber to meet bend-radius specs
    
- Replace physically damaged runs
    

---

## Issue 4 — **Distance or Power Mismatch (Receiver Saturation)**

### What happens

Short SMF runs with powerful optics cause overload; long MMF/SMF cause attenuation.

### Symptoms

- Random flapping
    
- Input errors
    
- Very unstable link
    

### Fix

- Add **attenuator** for short SMF runs
    
- Use proper optical budget for long runs
    
- Use correct transceiver (LR, SR, ER, ZX)
    

---

# 4. How to Diagnose Cable Issues Quickly (CCNA Cheat Sheet)



```less
# 1. Check link status
show interfaces status
# If "notconnect" → cable/optic/port.

# 2. Check error counters
show interfaces <int>
# - CRC rising → noise, bad pair, dirty fiber
# - Late collisions → duplex mismatch or split pair
# - Runts → collisions or bad cable
# - Giants → MTU mismatch or damaged cable

# 3. Use TDR on copper if possible
test cable-diagnostics tdr interface g0/1
show cable-diagnostics tdr interface g0/1

# 4. Verify speed/duplex negotiation
show interfaces <int> status
# Wrong speed? cable or device mismatch.

# 5. Replace cable → observe counters again
# If counters stop: confirmed cable fault.
```

