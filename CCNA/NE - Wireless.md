
# NE  - Wireless

# **CHAPTER 1 — Wireless Networking Fundamentals (CCNA Level, Engineer Format)**

---

# **CHAPTER 13 — ONE-PAGE CCNA & CCNP WIRELESS SUMMARIES**

This chapter provides **compressed, exam-ready one-page cheat sheets** for CCNA and CCNP ENCOR.  
Each summary focuses purely on what you must know for the exams — nothing extra, no filler — but still maintains **engineer-level precision**.

Everything is delivered in **single, clean code blocks** per exam, exactly how you prefer it.

---

# **13.1 ONE-PAGE CCNA WIRELESS SUMMARY (200-301)**

```less
===================================================================================
CCNA WIRELESS – ONE PAGE SUMMARY (200-301)
===================================================================================

1. Wireless Basics
------------------
- Wi-Fi uses 2.4 GHz (1/6/11 only) and 5 GHz bands.
- Modulation: OFDM (11a/g/n/ac/ax), DSSS (11b).
- MIMO introduced in 802.11n → more throughput.
- Wi-Fi 6 (802.11ax) adds OFDMA + better efficiency.

2. RF Fundamentals
------------------
- RSSI target: -67 dBm for enterprise; SNR > 20 dB.
- 2.4 GHz = longer range, high interference.
- 5 GHz = shorter range, many channels, DFS.
- Attenuation: drywall ~3 dB, concrete ~10 dB, glass ~4 dB.

3. WLAN Components
------------------
- AP: lightweight (LWAP) or autonomous.
- WLC: manages RF, security, roaming, policies.
- Split-MAC: AP handles real-time MAC; WLC handles control/auth.

4. CAPWAP
---------
- Control: UDP/5246 (DTLS encrypted)
- Data: UDP/5247 (optional encrypted)
- Discovery: DHCP Opt 43, DNS, L2 broadcast.

5. Basic WLAN Configuration
---------------------------
- SSID = WLAN.
- WPA2-PSK or WPA2-Enterprise (802.1X).
- VLAN mapped to WLAN via policy/profile.
- Switchport for AP = trunk, native VLAN = management VLAN.

6. Security
-----------
- WPA2-PSK still common; WPA3-SAE preferred.
- WPA2-Enterprise = EAP/802.1X + RADIUS.
- PMF protects mgmt frames; required in WPA3.

7. Client Join Process
----------------------
SCAN → AUTH → ASSOC → 4-WAY HANDSHAKE → DHCP → DATA.

8. Roaming
----------
- L2 roam: same subnet, fast.
- L3 roam: requires mobility tunnel (ANCHOR/FOREIGN) in classic WLAN.

9. Troubleshooting
------------------
- Check RF first (RSSI, SNR, CCI).
- Check Auth/Assoc logs on WLC.
- DHCP failures = VLAN/trunk issues.
- AP join failures = DHCP/DNS/Option 43/DTLS issues.

===================================================================================
```

# **13.2 ONE-PAGE CCNP ENCOR WIRELESS SUMMARY**

(Advanced topics: QoS, RRM, high-density, 802.1X, SD-Access Wireless)

```less
===================================================================================
CCNP ENCOR – ADVANCED WIRELESS SUMMARY (350-401)
===================================================================================

1. Advanced RF
---------------
- Use 20 MHz channels in dense environments.
- High-density tuning: low Tx power, directional antennas.
- SNR > 25–30 dB for voice.
- Disable lowest data rates (6–12 Mbps mandatory recommended).

2. RRM (Radio Resource Management)
-----------------------------------
Functions:
- DCA: Dynamic Channel Assignment (avoid CCI/ACI/DFS).
- TPC: Transmit Power Control (balance cell sizes).
- CHD: Coverage Hole Detection.
- RF Groups: APs form RF neighborhood clusters.

Key commands:
- show wireless rrm dca summary
- show wireless rrm tsp summary

3. QoS (WMM/EDCA)
-----------------
Access Categories:
- AC_VO (Voice), AC_VI (Video), AC_BE, AC_BK.

EDCA Params (Engineer's view):
- AIFS (backoff spacing)
- CWmin/CWmax (contention)
- TXOP (airtime burst len)

Voice SSID:
- 11r FT enabled
- 20 MHz, mandatory data rates
- QoS marking preservation

4. Security (Advanced)
----------------------
- EAP-TLS = gold standard for 802.1X.
- PMK caching, OKC, Fast BSS Transition (802.11r).
- WPA3-SAE protects against offline cracking.
- WIPS/WIDS: rogue AP detection & containment.
- CleanAir for non-Wi-Fi interference.

5. Mobility
-----------
Classic:
- Anchor/Foreign controller architecture.
- L2 roam = fast; L3 roam = mobility tunnel.

SD-Access Wireless:
- VXLAN for data plane.
- LISP for mobility control.
- WLC handles only control/auth; fabric edges handle data.
- SGT (Scalable Group Tags) for segmentation.

6. High-Density Wireless
-------------------------
- Keep channel width 20 MHz.
- Increase AP count but reduce Tx power.
- Remove 2.4 GHz if possible.
- Use directional antennas and tuned RF profiles.
- Limit SSIDs to 3–4.
- Client Load Balancing + Band Steering.

7. Troubleshooting (Advanced)
-----------------------------
- AP join: (DTLS, DNS, DHCP Opt 43, Certs).
- 802.1X: debug dot1x, ISE logs, cert validation.
- WPA3 issues: PMF mismatch, driver bugs.
- RF: SNR, retries, airtime, MCS tracking.
- Capture 802.11 frames using AP Sniffer mode.

===================================================================================
```

# **13.3 ONE-PAGE "WIRELESS ENGINEER" SUMMARY**

(The most important concepts in one ultra-dense technical page)

```less
===================================================================================
WIRELESS ENGINEER – ONE PAGE SUMMARY
===================================================================================

ARCHITECTURE
------------
- Lightweight AP + Catalyst 9800 = modern standard.
- Split-MAC: AP = real-time MAC, WLC = control/auth/RRM.
- CAPWAP: Control 5246 (DTLS), Data 5247.
- Tags: Policy Tag = WLAN + Policy + RF + Site.

RF DESIGN
----------
- RSSI ≥ -67 dBm, SNR ≥ 25 dB (voice).
- 5 GHz primary band; 2.4 GHz for IoT only.
- Use 20 MHz channels in enterprise deployments.
- Minimize CCI; disable legacy data rates.
- AP height: 2.4–3 m ceiling mount.

SECURITY
---------
- WPA3-SAE, WPA3-Enterprise preferred.
- 802.1X + EAP-TLS mandatory for enterprise.
- PMF-enabled always.
- Rogue AP detection + containment.
- CleanAir for interference analysis.

ROAMING
--------
- 802.11r FT for voice.
- PMK caching and OKC accelerate roaming.
- SD-Access: LISP mobility = instant L3 roam.

QoS
----
- WMM must be enabled.
- EDCA tunes contention windows per AC.
- Voice: AC_VO; Video: AC_VI.
- Maintain DSCP markings end-to-end.

SD-ACCESS WIRELESS
-------------------
- WLC does control only; fabric edges handle data (VXLAN).
- SGT tags travel with packets.
- VNs replace VLANs.
- DNA Center automates RF + policy + roaming + segmentation.

TROUBLESHOOTING
----------------
1. RF: RSSI, SNR, CCI, ACI, MCS rates.
2. MAC: auth/assoc frame exchange.
3. Security: 4-way handshake, EAP flow.
4. Network: VLAN, DHCP, routing.
5. Control: CAPWAP join/DTLS.
6. Data: FlexConnect local switching vs central.

===================================================================================
```


## 14.1.2 Debug Commands (Careful!)

```less
debug wireless mac <MAC>
debug wireless events enable
debug capwap events enable
debug capwap errors enable
debug dtls events enable
debug dot1x all enable
debug pmkid all enable

# Stop debugging:

no debug all
```


# **14.2 AireOS WLC Command Reference**

Even though AireOS is legacy, still widely encountered.

## **14.2.1 Show Commands**

```less
show ap summary
show ap config general <AP>
show client summary
show client detail <MAC>
show interface detailed
show wlan summary
show auth statistics
```

## 14.2.2 Debug Commands

```less
debug capwap events enable
debug pmkid enable
debug dot1x events enable
debug aaa events enable
debug dhcp packet enable
```

## 14.3 AP Console Commands

```less
show capwap client
show capwap ip config
show version
show crypto pki trustpoints
debug capwap client no-reload
debug dtls error
debug dot11 state
debug dot11 events

# AP reload:

reload
```


# **14.4 Switch Command Reference (Catalyst 2960-X / 9300)**

## **Interface Configuration for AP**

```less
interface Gi1/0/24
 description Cisco AP
 switchport mode trunk
 switchport trunk native vlan 10
 switchport trunk allowed vlan 10,20,30,40
 spanning-tree portfast trunk
 power inline auto
```

## Troubleshooting

```less
show interface status
show interface trunk
show mac address-table interface Gi1/0/24
show cdp neighbors
```


# **14.5 IEEE 802.11 Amendment Table (Engineer-Focused)**

|Amendment|Purpose|
|---|---|
|**802.11b**|2.4 GHz, DSSS, 11 Mbps|
|**802.11a**|5 GHz, OFDM, 54 Mbps|
|**802.11g**|2.4 GHz, OFDM, 54 Mbps|
|**802.11n**|MIMO, HT PHY, 600 Mbps|
|**802.11ac**|VHT PHY, 5 GHz, up to 7 Gbps|
|**802.11ax (Wi-Fi 6)**|OFDMA, MU-MIMO, BSS coloring, 1–10 Gbps|
|**802.11be (Wi-Fi 7)**|Multi-Link Operation, 320 MHz channels|
|**802.11e**|QoS/WMM/EDCA|
|**802.11i**|WPA2 security|
|**802.11r**|Fast roaming|
|**802.11k**|Neighbor reports|
|**802.11v**|Client steering|
|**802.11w**|Protected Management Frames|
|**802.11s**|Mesh networking|

---

# **14.6 Wireless Glossary (Critical Definitions)**

|Term|Definition|
|---|---|
|**RSSI**|Received Signal Strength Indicator|
|**SNR**|Signal-to-Noise Ratio|
|**MCS**|Modulation & Coding Scheme|
|**CCA**|Clear Channel Assessment|
|**DCA**|Dynamic Channel Assignment|
|**TPC**|Transmit Power Control|
|**CHD**|Coverage Hole Detection|
|**PMF**|Protected Management Frames|
|**OFDMA**|Orthogonal Frequency Division Multiple Access|
|**BSS**|Basic Service Set|
|**ESS**|Extended Service Set|
|**RRM**|Radio Resource Management|
|**DTLS**|Datagram Transport Layer Security (CAPWAP)|
|**SGT**|Scalable Group Tag (TrustSec)|
|**VNID**|Virtual Network ID (SD-Access)|
|**EAPOL**|Extensible Authentication Protocol over LAN|
|**GTK**|Group Temporal Key|
|**PTK**|Pairwise Transient Key|
|**LISP**|Locator/ID Separation Protocol|

---

# **14.7 Data Rate Reference Table**

|Rate|Band|Recommended Usage|
|---|---|---|
|1, 2, 5.5, 11 Mbps|2.4 GHz|Disable for enterprise|
|6 Mbps|5 GHz|Often mandatory|
|12/24 Mbps|5 GHz|Good mandatory rates|
|54 Mbps|5 GHz|Legacy highest|

Higher mandatory rates → smaller cells → better roaming.

---

# **14.8 Wireless Formulas (Engineer Essentials)**

### **Free Space Path Loss (FSPL)**

```less
FSPL(dB) = 20 * log10(d) + 20 * log10(f) + 32.44
d = distance in km
f = frequency in MHz
```

## EIRP

```less
EIRP = Tx Power + Antenna Gain – Cable Loss
```

## SNR

```less
SNR = RSSI – Noise Floor
```

## Airtime Calculation

```less
Airtime = Frame Duration / Channel Capacity
```

## 14.9 CAPWAP Join Checklist (Engineer Troubleshooting Template)

```less
1. Does AP get an IP address? (DHCP)
2. Is the native VLAN correct on switchport?
3. Is Option 43 configured if needed?
4. Does DNS entry cisco-capwap-controller exist?
5. Is WLC reachable (ping)?
6. Does AP regulatory domain match WLC?
7. Any certificate issues? (DTLS failures)
8. Does AP have correct image?
9. Do policy tags, rf tags, site tags exist?
10. Is AP assigned the correct policy tag?
```

# **14.10 Authentication Failure Matrix**

|Failure Stage|Typical Cause|Fix|
|---|---|---|
|No probe response|AP down, SSID disabled|Check AP/WLAN status|
|Auth failure|Wrong PSK / 802.1X|Validate credentials or certs|
|Assoc failure|PMF mismatch, rate mismatch|Adjust PMF/rates|
|4-way handshake fail|Wrong PSK, PMK mismatch|Re-enter PSK / check RADIUS|
|DHCP fail|VLAN, trunk, ACL|Fix switch config|
|RADIUS reject|Incorrect ISE rules|Check policy sets|
|EAP failure|Certificate trust|Validate CA/CN|

---

# **14.11 FlexConnect Quick Reference**

**Use Case:** Remote sites, local data switching.

### Key Concepts:

- Central authentication (or local auth)    
- Local switching    
- WAN outage tolerant    
- VLAN mapping per WLAN    

### Commands:

```less
show ap name <AP> config flex
wireless profile flex <PROFILE>
wireless profile policy <POLICY> local-switching
```

# **14.12 Cisco RRM Default Thresholds (Useful for Tuning)**

|RRM Feature|Default Behavior|
|---|---|
|DCA Interval|10 min|
|TPC Range|-65 dBm target|
|CHD Trigger|Client RSSI < -80 dBm|
|Band Select|Enabled for 5 GHz preference|

---

# **14.13 Wireless Debug Workflow (Complete)**

```less
STEP 1: RF
  show ap dot11 5ghz statistics <AP>
  show wireless rrm dca summary

STEP 2: Client State
  show wireless client mac <MAC> detail

STEP 3: Logging
  show logging | include <AP or MAC>

STEP 4: Debug (if needed)
  debug wireless mac <MAC>
  debug dot1x all enable
  debug capwap events enable

STEP 5: Packet Capture
  AP Sniffer mode
  Wireshark 802.11 analysis
```
























# **14.1 Command Reference — Catalyst 9800 WLC**

## **14.1.1 Show Commands**


```less
show ap summary
show ap config general <AP>
show ap join stats detailed <AP>
show ap name <AP> capwap client config
show ap name <AP> image
show ap dot11 5ghz statistics <AP>
show ap dot11 2ghz statistics <AP>

show wireless client summary
show wireless client mac <MAC> detail

show wireless fabric client summary
show wireless mobility summary

show wireless rrm dca summary
show wireless rrm tsp summary
show wireless rrm channel stats <AP>

show wireless profile policy summary
show wireless tag policy
show wireless tag site
show wireless tag rf

show run | sec wireless
show logging | inc <AP-NAME>
```






# **1. Wireless Networking Fundamentals**

## **1.1 Evolution of Wireless LANs**

Wireless LANs started as a convenience technology and evolved into mission-critical enterprise infrastructure. Key milestones:

- **802.11 (1997):** First Wi-Fi standard, slow, legacy.    
- **802.11b (1999):** Popularization of Wi-Fi (11 Mbps, 2.4 GHz).    
- **802.11a (1999):** 54 Mbps, 5 GHz, less adoption due to cost.    
- **802.11g (2003):** 54 Mbps in 2.4 GHz, major adoption wave.    
- **802.11n (2009):** MIMO, bonded channels, major performance jump.    
- **802.11ac (2013):** 5 GHz only, multi-user enhancements.    
- **802.11ax (Wi-Fi 6, 2021):** OFDMA, high-density optimization.    
- **802.11be (Wi-Fi 7, ongoing):** Multi-link operation, > 40 Gbps theoretical.    

Enterprise Wi-Fi has shifted from:

- **Autonomous APs → Lightweight AP + WLC (Cisco split-MAC)**    
- **AireOS WLC → Catalyst 9800 IOS-XE Wireless Architecture**    
- **Local on-prem control → Cloud management (Meraki, DNA-C)**    

This evolution is important because **CCNA → CCNP expects understanding of modern controller-based architectures.**

| **Amendment**   | **Wi-Fi Gen** | **Frequency Band(s)** | **Max Data Rate**       | **Modulation / Coding** | **Channel Widths**  | **Key Technologies**                                                         |
| --------------- | ------------- | --------------------- | ----------------------- | ----------------------- | ------------------- | ---------------------------------------------------------------------------- |
| **802.11-1997** | N/A           | 2.4 GHz               | 2 Mbps                  | DSSS or FHSS            | 22 MHz              | The original standard ratified in 1997.                                      |
| **802.11b**     | N/A           | 2.4 GHz               | 11 Mbps                 | HR-DSSS (CCK)           | 22 MHz              | Introduced in 1999; backward compatible with original DSSS.                  |
| **802.11a**     | N/A           | 5 GHz                 | 54 Mbps                 | OFDM                    | 20 MHz              | Introduced in 1999; uses 52 subcarriers (48 data, 4 pilot).                  |
| **802.11g**     | N/A           | 2.4 GHz               | 54 Mbps                 | OFDM                    | 20 MHz              | Introduced in 2003; brought OFDM to the 2.4 GHz band.                        |
| **802.11n**     | Wi-Fi 4       | 2.4 & 5 GHz           | 600 Mbps                | OFDM, up to 64-QAM      | 20, 40 MHz          | **HT (High Throughput)**; introduced MIMO and packet aggregation.            |
| **802.11ac**    | Wi-Fi 5       | 5 GHz                 | 6.93 Gbps               | OFDM, up to 256-QAM     | 20, 40, 80, 160 MHz | **VHT (Very High Throughput)**; introduced MU-MIMO (Downlink only).          |
| **802.11ax**    | Wi-Fi 6 / 6E  | 2.4, 5, & 6 GHz       | ~9.6 Gbps (4x 802.11ac) | OFDMA, up to 1024-QAM   | 20, 40, 80, 160 MHz | **HE (High Efficiency)**; introduced OFDMA, UL/DL MU-MIMO, and BSS Coloring. |

---

## **1.2 Wireless Terminology**

## **Service Sets**

| Term      | Meaning                                                         |
| --------- | --------------------------------------------------------------- |
| **SSID**  | Network name (human-readable)                                   |
| **BSSID** | MAC address of a radio interface in an AP                       |
| **BSS**   | Basic Service Set: AP + its associated clients                  |
| **ESS**   | Extended Service Set: Multiple APs sharing SSID, VLAN, security |

### **Channels & Bands**

- **2.4 GHz:** 3 usable non-overlapping channels (1, 6, 11)    
- **5 GHz:** Many channels, includes DFS (radar-protected) ranges    
- **6 GHz:** Wi-Fi 6E, high performance, low interference    

### **MAC Layer Terms**

- **CSMA/CA:** Collision avoidance, not detection    
- **AIFS, SIFS:** Inter-frame spaces    
- **RTS/CTS:** Optional mechanism to reduce hidden-node collisions
    

### **Client Behavior**

- Scanning → Authentication → Association
    
- Clients choose APs based on:    
    - RSSI        
    - SNR        
    - AP load        
    - Vendor-specific roaming algorithms        

---

## **1.3 Wireless Standards Overview (PHY/MAC)**

### **Physical Layer**

Major technologies:

- OFDM
    
- MIMO (2x2, 3x3, 4x4, 8x8)
    
- MU-MIMO
    
- OFDMA (Wi-Fi 6)
    
- Beamforming
    

### **MAC Layer**

- Management frames (beacons, probe requests, authentication, association)
    
- Control frames (RTS/CTS, ACK)
    
- Data frames (payload)
    

---

# **1.4 Multi-Layer Diagram: WLAN at OSI Perspective**

```less
+-----------------------------------------------------------------------+
|                        OSI MODEL (WLAN VIEW)                          |
+------------+---------------+-----------------------+------------------+
| Layer 7    | Application   | Apps using Wi-Fi       | Web, VoIP, etc. |
+------------+---------------+-----------------------+------------------+
| Layer 4    | Transport     | TCP/UDP over WLAN      | Ports, flows    |
+------------+---------------+-----------------------+------------------+
| Layer 3    | Network       | IP addressing, routing | IPv4/IPv6       |
+------------+---------------+-----------------------+------------------+
| Layer 2.5  | 802.1X/Auth   | EAP, RADIUS, AAA       | WPA2/3-Enterprise|
+------------+---------------+-----------------------+------------------+
| Layer 2    | 802.11 MAC    | CSMA/CA, frames        | Assoc, roam     |
+------------+---------------+-----------------------+------------------+
| Layer 1    | 802.11 PHY    | RF, modulation, MIMO   | 2.4/5/6 GHz     |
+------------+----------------------------------------------------------+
```

This diagram will be reused cross-chapter to include controllers, AP modes, security workflows, etc.

---

# **1.5 Wireless Communication Flow (Physical to Application)**

**Complex multi-layer diagram as required.**

```less
+=========================================================================+
|                     END-TO-END WLAN COMMUNICATION                       |
+=========================================================================+

CLIENT STA                              AIR                         ACCESS POINT
(Windows/Linux/macOS)                                              (Cisco LAP)

Layer 7: App ----------------------- Data ------------------------------> App Host
       (HTTP/VoIP/DB)                                                  (Server)

Layer 4: TCP/UDP -------------- Segments -------------------------------> TCP/UDP

Layer 3: IP --------------------- IP Packets ---------------------------> IP

Layer 2.5: 802.1X/Auth ------- EAPOL Frames ----------------------------> WLC
                               (if Enterprise)

Layer 2: 802.11 MAC ---- Management + Data Frames -----> 802.11 MAC on AP
                        (beacons, probe, auth, assoc)

Layer 1: RF -------- Physical Wave Transmission -------- RF ------------> Antenna

AP → CAPWAP Tunnel → WLC
+--------------------------------------------------------------+
| CAPWAP Control Plane (DTLS encrypted)                        |
| CAPWAP Data Plane (encapsulated 802.11 frames)               |
+--------------------------------------------------------------+

WLC → Switch → Router → LAN/WAN → Destination
```

# **1.6 CSMA/CA – Detailed MAC Operation**

**Complete engineering-level sequence:**

```less
Client wants to transmit frame:

1. Listen to channel → is it free?
2. Wait DIFS (Distributed Inter Frame Space)
3. Calculate random backoff
4. Count down backoff while medium idle
5. Transmit DATA frame
6. AP responds with ACK

If collision:
7. Client doubles contention window (CWmin → CWmax)
8. Retries transmission
```

Hidden node mitigation:

```less
CLIENT A ---\
              \
               ---> AP  (A and C cannot hear each other)
              /
CLIENT C ---/

AP uses RTS/CTS:
A → RTS
AP → CTS (heard by C)
→ C stays silent
```

# **1.7 Summary Table – Wireless Fundamentals (Exam Focus)**

| Topic              | CCNA Focus | CCNP Focus              |
| ------------------ | ---------- | ----------------------- |
| RF basics          | Required   | Deep (math + design)    |
| 802.11 frame types | Required   | Very deep               |
| CSMA/CA            | Required   | Timers & efficiency     |
| Standards          | Basic      | Engineering detail      |
| Channels           | CCNA-level | DFS, regulatory domains |
| AP modes           | Mentioned  | Critical                |
| WLC function       | Basic      | Advanced control plane  |


# **CHAPTER 2 — RF BASICS AND WIRELESS PHYSICS**

_Cisco-centric, Engineer Level, CCNA → CCNP depth, complex diagrams included_

---

# **2. RF BASICS AND WIRELESS PHYSICS**

Wireless LAN performance is determined not primarily by software, but by **RF behavior, environmental physics, antenna design, and regulatory restrictions**.  
Understanding these fundamentals is mandatory for designing, deploying, and troubleshooting Cisco WLANs (Catalyst 9800, AireOS, CAPWAP APs, etc.).

---

# **2.1 Electromagnetic Waves – Core Concepts**

Wi-Fi uses **radio waves**, a form of electromagnetic radiation.  
Key parameters:

- **Frequency (Hz):** cycles per second    
- **Wavelength (λ):** physical size of the wave    
- **Amplitude:** signal strength    
- **Phase:** wave alignment    

![[Pasted image 20251218213932.png]]

![[Pasted image 20251218214026.png]]



![[Pasted image 20251218213954.png]]

![[Pasted image 20251218214006.png]]

### Wifi Bands and Channel Frequencies


![[Pasted image 20251218225446.png]]

| **Band**    | **Channel** | **Frequency Range (MHz)** | **Center Frequency** | **Notes**                     |
| ----------- | ----------- | ------------------------- | -------------------- | ----------------------------- |
| **2.4 GHz** | 1           | 2401 – 2423               | 2412 MHz             | Standard worldwide            |
|             | 6           | 2426 – 2448               | 2437 MHz             | Standard worldwide            |
|             | 11          | 2451 – 2473               | 2462 MHz             | Standard worldwide            |
|             | 13          | 2461 – 2483               | 2472 MHz             | Common in EU/Asia             |
|             | 14          | 2473 – 2495               | 2484 MHz             | Japan only (802.11b)          |
| **5 GHz**   | 36          | 5170 – 5190               | 5180 MHz             | U-NII-1 (Indoor)              |
|             | 48          | 5230 – 5250               | 5240 MHz             | U-NII-1 (Indoor)              |
|             | 52 – 64     | 5250 – 5330               | 5260 – 5320 MHz      | U-NII-2A (DFS Required)       |
|             | 100 – 144   | 5490 – 5730               | 5500 – 5720 MHz      | U-NII-2C (DFS Required)       |
|             | 149 – 165   | 5735 – 5835               | 5745 – 5825 MHz      | U-NII-3 (Consumer high-power) |
| **6 GHz**   | 1           | 5945 – 5965               | 5955 MHz             | Wi-Fi 6E/7 only               |
|             | 5           | 5965 – 5985               | 5975 MHz             | Wi-Fi 6E/7 only               |
|             | ...         | ...                       | ...                  | Incrementing by 4 per channel |
|             | 233         | 7105 – 7125               | 7115 MHz             | Highest channel in 6 GHz      |

### Relationship between frequency and wavelength:

```less
λ = c / f
λ = wavelength
c = speed of light (~3 × 10^8 m/s)
f = frequency in Hz
```

Example wavelengths:

- 2.4 GHz → ~12.5 cm    
- 5 GHz → ~6 cm    
- 6 GHz → ~5 cm    

**Important:**  
Higher frequency = **shorter wavelength**, less ability to penetrate walls, more attenuation.

---

# **2.2 Attenuation, Path Loss & Environmental Impact**

Signal strength decreases with distance and obstacles.

## **2.2.1 Free-Space Path Loss (FSPL)**

Formula (in dB):

```less
FSPL = 20log10(d) + 20log10(f) + 32.44
d = distance (km)
f = frequency (MHz)
```

### **Key Takeaway**

For equal distance, **5 GHz loses ~6 dB more** than 2.4 GHz → **drastically shorter effective range**.

---

## **2.2.2 Indoor Material Attenuation**

|Material|Typical Loss|
|---|---|
|Drywall|3 dB|
|Brick|6–9 dB|
|Concrete|10–15 dB|
|Double brick + insulation|20–25 dB|
|Elevator shafts, metal doors|20+ dB|
|Human body|3 dB per person (varies)|

_Enterprise deployment must consider these values in AP placement._

---

# **2.3 RF Behaviors: Reflection, Absorption, Scattering, Diffraction, Multipath**

Wi-Fi signals interact with the environment:

### **Reflection**

Occurs on metal, glass → can create echo signals.

### **Absorption**

Walls, humans, furniture → lower SNR.

### **Scattering**

Small particles, rough surfaces → dispersed signal.

### **Diffraction**

Bending around obstacles → reduced but usable signal.

### **Multipath**

Multiple signal copies reach AP or client.

**Good (802.11n/ac/ax use MIMO to exploit it)**  
**Bad (causes inter-symbol interference if unmanaged)**

---

# **2.4 Antennas – Types, Gain, Patterns**

## **2.4.1 Antenna Types**

|Antenna Type|Description|Usage|
|---|---|---|
|**Omni**|360° horizontal coverage|Offices, hallways|
|**Patch / Panel**|Focused beam|Walls, warehouses|
|**Yagi**|Long-range directional|Outdoor bridging|
|**Sector**|Wide-angle directional|Stadiums, outdoors|
|**Dish**|Highly directional|Point-to-point links|

---

## **2.4.2 Antenna Gain & EIRP**

### Gain measured in:

- **dBi** – relative to isotropic radiator
    
- **dBd** – relative to dipole
    

Higher gain = **more focused energy**, NOT more power.

### **EIRP**

```less
EIRP = Tx Power (dBm) + Antenna Gain (dBi) – Cable Loss (dB)
```

Regulations limit EIRP.

Cisco APs auto-adjust power via **RRM/TPC (Transmit Power Control)**.

---

# **2.5 Complex Multi-Layer Diagram: RF Propagation in an Enterprise Floor**

```less
+---------------------------------------------------------------------------------------------------+
|                                   ENTERPRISE OFFICE RF MODEL                                      |
+---------------------------------------------------------------------------------------------------+
|                                                                                                   |
|  AP1 (5 GHz, CH 36)                          AP2 (5 GHz, CH 44)                                  |
|  Tx Power: 12 dBm                             Tx Power: 8 dBm                                    |
|  Antenna: Internal Omni                     Antenna: Internal Omni                               |
|                                                                                                   |
|   SIGNAL PATHS:                                                                                  |
|                                                                                                   |
|      (Reflection)            ┌─────────────── Glass Wall ───────────────┐                         |
|   Client A  -->   *\         |   * reflection paths                      |                         |
|                    \*        |                                            |                         |
|                     \ *      |       (Absorption)                        |                         |
|                      \  ---> |   Drywall (-3 dB)                          |                        |
|                                                                                                   |
|       (Multipath)                                                                                    |
|   Client B --->   *----*---*-------> AP1  (MIMO helps combine paths)                            |
|                                                                                                   |
|       (Interference region)                                                                        |
|   Overlap Zone: AP1 CH36  ~20m radius, AP2 CH44 ~15m                                               |
|   → Co-channel interference avoided due to different channels                                      |
|                                                                                                   |
|       (Human absorption)                                                                           |
|   25 people in meeting room → ~75 dB total dynamic absorption                                     |
|                                                                                                   |
+---------------------------------------------------------------------------------------------------+
```

This multi-layer RF diagram shows how signals interact with real-world objects.

---

# **2.6 2.4 GHz vs 5 GHz vs 6 GHz – Engineering Comparison**

|Feature|2.4 GHz|5 GHz|6 GHz (Wi-Fi 6E)|
|---|---|---|---|
|Range|Long|Medium|Short|
|Penetration|Best|Good|Weak|
|Channels|3|Many|Many (very wide)|
|Interference|High|Medium|Low|
|DFS Required|No|Some channels|Yes (varies)|
|Best Use|IoT, legacy devices|Enterprise clients|High-density, modern devices|

---

# **2.7 SNR, RSSI, Noise Floor – Performance Metrics**

## **RSSI (Received Signal Strength)**

- Measured in dBm
    
- Target: **-65 dBm or better** for enterprise
    

## **Noise Floor**

- Background RF energy
    
- Typical indoor values: **-90 dBm to -100 dBm**
    

## **SNR (Signal-to-Noise Ratio)**

```less
SNR = RSSI – Noise Floor
```

SNR thresholds:

|Application|Required SNR|
|---|---|
|Web|≥ 15 dB|
|Video|≥ 25 dB|
|Voice (Cisco Jabber/Wi-Fi VoIP)|≥ 30 dB|

---

# **2.8 MIMO, MU-MIMO, OFDMA – Modern PHY Enhancements**

## **MIMO (802.11n)**

Multiple antennas to increase throughput using multipath.

## **MU-MIMO (802.11ac Wave 2)**

AP can send to multiple clients simultaneously.

## **OFDMA (802.11ax / Wi-Fi 6)**

Physical layer resource unit allocation (like mini-sub-channels).

**Benefit:**  
Reduces latency in high-density networks.

---

# **2.9 Complex Diagram – MIMO Beamforming Mechanism**

```less
CLIENT STA                               ACCESS POINT (4x4:4 MIMO)
(2 antennas)                             (4 antennas)

    Rx1  Rx2                           Tx1 Tx2 Tx3 Tx4
     |    |                              |   |   |   |
     |    └──────────────────────────────┼───┼───┘   |
     |                                   |   |       |
     └────────────────────────────────────┼──┘       |
                                         |
+----------------------------------------------------------------------------+
|               BEAMFORMING CALCULATION (AP adjusts phase & amplitude)       |
|----------------------------------------------------------------------------|
| AP uses client feedback (802.11ac/ax sounding) to compute steering matrix. |
| AP transmits combined beams → signals arrive in-phase at client antennas.  |
| Result: Higher RSSI, higher SNR, stronger MCS rate.                         |
+----------------------------------------------------------------------------+
```

# **2.10 Regulatory Domain Constraints (Cisco Requirement)**

Cisco APs must match:

- Country code
    
- Power limits
    
- Channel availability
    
- DFS rules
    

Regulated by:

- ETSI (Europe)
    
- FCC (US)
    
- MIC (Japan)
    

Wrong domain prevents AP from joining WLC.

---

# **2.11 Summary Table (Exam & Engineering Focus)**

| Topic        | CCNA       | CCNP (ENCOR)               |
| ------------ | ---------- | -------------------------- |
| RF basics    | Required   | Deep-level physics         |
| FSPL         | Mentioned  | Calculations               |
| Antennas     | Basic      | Full pattern analysis      |
| Interference | Basic      | Spectrum-based mitigation  |
| MIMO/OFDMA   | High-level | Scheduling + RU allocation |
| SNR/RSSI     | Required   | RF optimization metrics    |



# **CHAPTER 3 — WIRELESS ARCHITECTURES & COMPONENTS**

_Cisco-centric. CCNA → CCNP depth. Complex multi-layer diagrams included._

---

# **3. Wireless Architectures & Components**

Modern enterprise Wi-Fi—especially in Cisco environments—is built around **centralized management, policy enforcement, automation, and RF optimization**.  
This chapter explains the core architectural building blocks:

- AP types (autonomous vs lightweight)
    
- Controller architectures (AireOS → Catalyst 9800)
    
- AP operational modes
    
- CAPWAP tunnels
    
- WLC management and data plane behavior
    
- Deployment models (centralized, distributed, FlexConnect, mesh)
    

---

# **3.1 Autonomous vs. Lightweight Architectures**

## **3.1.1 Autonomous APs (Legacy, Standalone)**

AP handles everything locally:

- RF management
    
- Security
    
- VLANs
    
- SSID configuration
    
- QoS
    

**Problems:**

- No central control
    
- Hard to scale
    
- Manual configuration
    
- Inefficient RF tuning
    

These still appear in older 1100/1200/1300 Cisco APs.

---

## **3.1.2 Lightweight APs (LWAP)**

Modern standard for enterprises.

AP delegates major tasks to WLC (controller).  
Runs Cisco **Split-MAC Architecture**.

|Function|Who Handles It|
|---|---|
|Authentication|WLC|
|Association|WLC (via AP)|
|Key management|WLC|
|RF management|WLC (RRM)|
|Frame forwarding|AP (wireless → CAPWAP tunnel)|
|QoS|Roaming decisions|

Lightweight APs communicate using:

- **CAPWAP Control Tunnel (UDP/5246)**
    
- **CAPWAP Data Tunnel (UDP/5247)**
    

---

# **3.2 Cisco Split-MAC Architecture (Critical Exam Topic)**

The most important concept in Cisco WLAN engineering.

## **3.2.1 Which MAC functions run where?**

### **AP Handles (local):**

- ACK frames
    
- RTS/CTS
    
- Encryption/decryption (AES)
    
- QoS marking
    
- RF functions (signal measurement)
    

### **WLC Handles (centralized):**

- Authentication
    
- Association decisions
    
- Mobility management
    
- Key exchange
    
- VLAN assignment
    
- ACL enforcement
    
- RRM (channel/power decisions)
    

---

# **Complex Multi-Layer Diagram: Cisco Split-MAC Architecture**

```less
                     +---------------------------------------------------+
                     |               CATALYST 9800 WLC                    |
                     |---------------------------------------------------|
                     |  - Central Auth (802.1X/EAP/RADIUS)               |
                     |  - Association State Machine                       |
                     |  - WLANs, Policies, VLAN Mapping                  |
                     |  - RRM (DCA, TPC, CHD)                             |
                     |  - Mobility Control (L2/L3 roaming)                |
                     +-------------------------^-------------------------+
                                               | CAPWAP Control (5246)
                         CAPWAP Data (5247)    |
+---------------------------+                   |
|   CISCO LIGHTWEIGHT AP   |                   |
|---------------------------|<------------------+
| Local MAC Functions:     |
| - 802.11 encryption/dec  |
| - ACK, RTS/CTS           |
| - QoS (WMM)              |
| - RF measurement          |
|---------------------------|
| Radios: 2.4/5/6 GHz      |
+---------------------------+
          ^
          |
          | 802.11 Frames (Mgmt + Data)
          |
   +-------------+
   | CLIENT STA  |
   +-------------+

```


# **3.3 Wireless Deployment Models**

Cisco supports several deployment models.  
Here are the main ones for CCNA → CCNP.

---

## **3.3.1 Centralized (Enterprise Standard)**

All APs connect over Layer 2/3 to a central WLC.

**Advantages:**

- Simplified management
    
- Consistent security policies
    
- RRM for channel/power optimization
    
- Fast/secure roaming
    
- High density support
    

**Disadvantages:**

- Controller dependency
    
- WAN latency (if remote sites)
    

---

## **3.3.2 FlexConnect (Remote Sites)**

AP handles local switching, even without WLC, but still uses central control when available.

### **FlexConnect Local Switching:**

- Data forwarded to local switch/VLAN at remote site
    
- Control stays with WLC
    

### **FlexConnect Local Authentication:**

Allows AP to authenticate clients without WLC connectivity.

Cisco use case: **Retail stores, branches, remote offices**

---

## **3.3.3 Distributed / Cloud (Meraki, DNA-C)**

Controller is hosted in cloud or in SD-Access fabric.

WLC functions are abstracted away.

---

## **3.3.4 Mesh / Bridge Mode**

Used for outdoor scenarios where APs wirelessly backhaul traffic.

- Root AP (RAP)
    
- Mesh AP (MAP)
    
- Bridge AP for point-to-point or point-to-multipoint
    

---

# **3.4 AP Operational Modes (Cisco-specific)**

### **Local Mode (default)**

- All traffic tunneled to WLC
    
- Best for campus networks
    
- Provides full RRM scanning
    

### **Monitor Mode**

- Scans channels
    
- Detects rogue APs/clients
    
- No serving of clients
    

### **Sniffer Mode**

- Captures 802.11 frames
    
- Exports to Wireshark or analyzer
    

### **FlexConnect Mode**

- For remote sites
    
- Local switching
    
- Optional local authentication
    

### **Bridge / Mesh Mode**

- Point-to-point or point-to-multipoint bridging
    

### **Rogue Detector**

- Listens for rogue devices by MAC
    
- Does not serve clients
    

---

# **3.5 CAPWAP Protocol (Control and Provisioning of Wireless Access Points)**

The lifeline between AP and WLC.

## **3.5.1 CAPWAP Control Plane**

- Port 5246
    
- DTLS encrypted
    
- Authentication, configuration, keepalive
    

## **3.5.2 CAPWAP Data Plane**

- Port 5247
    
- May be encrypted or unencrypted
    
- Tunnels client data frames to WLC
    

---

# **Complex Diagram: CAPWAP Establishment Flow**

```less
CLIENT STA                             AP                          WLC (9800)
   |                                   |                               |
   |---802.11 Probe/Auth/Assoc-------> |                               |
   |                                   |                               |
   |                                   |---Discovery------------------>|
   |                                   | (L2 broadcast, DHCP opt 43,    |
   |                                   |  DNS: CISCO-CAPWAP-CONTROLLER)|
   |                                   |                               |
   |                                   |<------Join Response-----------|
   |                                   |                               |
   |                                   |----DTLS Setup (Control)------>|
   |                                   |                               |
   |                                   |---CAPWAP Data Tunnel----------|
   |                                   |                               |
   |<------802.11 Data via CAPWAP----->|==========DATA===========>     |
```

# **3.6 Catalyst 9800 Architecture vs. AireOS**

## **Catalyst 9800 (Modern)**

- IOS-XE OS (modular, programmable, API-driven)
    
- Model-driven telemetry
    
- Always-on, hitless upgrade options
    
- Native integration with DNA-C
    
- Excellent for SD-Access Wireless
    

### Internal Architecture:

- **Control Plane Node (CPN)**
    
- **Data Plane Node (DPN)**
    
- **RF Tags, Policy Tags, Site Tags**
    
- **Profiles for WLAN, RLAN, Policy, Flex, RF**
    

---

## **AireOS (Legacy)**

- Monolithic OS
    
- Mobility groups, AP groups
    
- Limited automation
    
- GUI simpler but outdated
    
- Still used in CCNA books, but EoL approaching
    

**Both will be documented when necessary.**

---

# **3.7 Mobility Architecture (Roaming)**

Cisco supports:

### **Layer 2 Roaming**

- Same subnet
    
- No IP change
    
- Very fast
    

### **Layer 3 Roaming**

- Different subnets
    
- Anchor/foreign WLC
    
- Mobility tunnel required
    

---

# **Complex Multi-Layer Diagram: Cisco Centralized Wireless Network**


```less
+==========================================================================================+
|                               ENTERPRISE WLAN ARCHITECTURE                               |
+==========================================================================================+

                            +---------------- CATALYST 9800 WLC ----------------+
                            |  - AAA / 802.1X                                  |
                            |  - WLAN Policies                                 |
                            |  - RF Management (RRM)                           |
                            |  - Mobility + Roaming                            |
                            |  - CAPWAP Control                                |
                            +----------------------^---------------------------+
                                                   | CAPWAP Control + Data
                                                   |
       +---------------------------+               |                  +----------------------------+
       | Cisco Lightweight AP #1   |<--------------+----------------->| Cisco Lightweight AP #2    |
       | Mode: Local               |                                  | Mode: Local                |
       | Radios: 2.4 / 5 / 6 GHz   |                                  | Radios: 2.4 / 5 / 6 GHz    |
       +---------------------------+                                  +----------------------------+
                     ^                                                          ^
                     |                                                          |
                     | 802.11a/b/g/n/ac/ax Frames                               |
                     |                                                          |
              +---------------+                                          +---------------+
              | CLIENT STA A  |                                          | CLIENT STA B  |
              +---------------+                                          +---------------+

                            +----------------- SWITCH CORE -----------------+
                            | VLANs, SVI, Routing, QoS, Trust Boundaries    |
                            +------------------------------------------------+

```

# **3.8 Typical Enterprise Wireless Components (Cisco)**

### **AP Models**

- Catalyst 9105, 9115, 9120, 9130 (Wi-Fi 6)
    
- Cisco legacy: 2700, 2800, 3800, 4800
    

### **WLC Models**

- **Catalyst 9800 series**
    
- AireOS: 3504, 5520, 8540, 2504
    

### **Switches**

- Catalyst 2960-X (access)
    
- Catalyst 9300 (access/Core SD-Access-ready)
    
- Catalyst 9500 (distribution/core)
    

### **Authentication**

- Cisco ISE (AAA/RADIUS)

# **3.9 Summary Table (Exam & Practical Deployment)**

| Component   | CCNA               | CCNP                       | Engineers in Field        |
| ----------- | ------------------ | -------------------------- | ------------------------- |
| AP Types    | Autonomous vs LWAP | Modes, RF roles            | Deployment design         |
| WLC         | Basics             | Catalyst 9800 architecture | High availability, SSO    |
| CAPWAP      | Required           | DTLS, data-plane           | Troubleshooting AP join   |
| FlexConnect | Required           | Split tunneling            | Retail/branch deployments |
| Mobility    | Mentioned          | L2/L3 roaming              | Multi-WLC design          |
| Mesh        | Basic              | Root/MAP design            | Outdoor RF engineering    |


# **CHAPTER 4 — WLAN STANDARDS & OPERATIONS (MAC BEHAVIOR)**

Cisco-centric. Deep CCNA → CCNP → Engineer level.  
Includes detailed MAC-layer workflows, WPA/WPA2/WPA3 processes, roaming, and multi-layer diagrams.

---

# **4. WLAN Standards & Operations (MAC Layer)**

Wi-Fi functions are defined by the **IEEE 802.11 standard**, which specifies:

- Frame formats
    
- Management, control, data workflows
    
- Authentication & association
    
- Power-saving mechanisms
    
- Roaming behavior
    
- Security and encryption handshakes
    
- Channel access behavior (CSMA/CA)
    

This chapter is critical for CCNA and absolutely essential for CCNP ENCOR.

---

# **4.1 802.11 Frame Types (The Foundation of WLAN Operations)**

802.11 defines **three categories**:

## **4.1.1 Management Frames**

Used to establish and maintain wireless communication.

|Frame|Purpose|
|---|---|
|**Beacon**|Advertises SSID, capabilities, channel|
|**Probe Request/Response**|Discovery of networks|
|**Authentication**|First step in joining AP|
|**Association/Reassociation**|Joining the BSS|
|**Disassociation**|Graceful disconnect|
|**Deauthentication**|Session teardown|

Cisco WLC logs these events and uses them to trigger RRM, load balancing, and roaming decisions.

---

## **4.1.2 Control Frames**

Support MAC functionality.

|Frame|Function|
|---|---|
|**RTS/CTS**|Reduces hidden-node collisions|
|**ACK**|Acknowledges received frames|
|**PS-Poll**|Power-save mechanisms|

These frames always operate at **basic (lowest) rates**, so too many of them reduces airtime efficiency.

---

## **4.1.3 Data Frames**

Carry user payload + sometimes encryption headers.

Includes:

- QoS Data
    
- Null Data (power-save)
    
- A-MPDU / A-MSDU (aggregation in 802.11n/ac/ax)
    

---

# **4.2 Detailed MAC Frame Format (Engineer's View)**

```less
+---------------------------------------------------------------------------------------------+
|                                   802.11 MAC FRAME                                          |
+----------------------+----------------------+-----------------------------+------------------+
| Frame Control (2 B)  | Duration/ID (2 B)    | Address Fields (6 B x 3-4) | Seq Ctrl (2 B)   |
+----------------------+----------------------+-----------------------------+------------------+
| QoS Ctrl (2 B)*      | HT Ctrl (4 B)*       | Frame Body (0–2312 B)      | FCS (4 B)        |
+----------------------+----------------------+-----------------------------+------------------+
* present only in QoS and HT/VHT/HE frames
```

Address fields depend on whether the frame is going to/from the DS (Distribution System):

|Scenario|To DS|From DS|Addr1|Addr2|Addr3|Addr4|
|---|---|---|---|---|---|---|
|Client → AP|1|0|AP|Client|BSSID|—|
|AP → Client|0|1|Client|AP|BSSID|—|
|WDS/Mesh|1|1|AP1|AP2|Destination|Source|

---

# **4.3 802.11 Authentication & Association Workflow (Critical)**

Wi-Fi joining occurs in **multiple states**.

## **4.3.1 State Machine Overview**

```less
STATE 1: Unaunthenticated, Unassociated
   |
   |-- Authentication Request/Response
   v
STATE 2: Authenticated, Unassociated
   |
   |-- Association Request/Response
   v
STATE 3: Authenticated, Associated
```

### The WLC (Catalyst 9800) handles:

- Authentication/Association logic (centralized)
    
- Policy application
    
- VLAN assignment
    

The AP forwards mgmt frames over CAPWAP control.

---

# **Complex Multi-Layer Diagram: Authentication → Association**

```less
CLIENT STA                     AP (Local Mode)                     WLC (Catalyst 9800)
-----------                   ----------------                    ----------------------
| Scan Channels |             | Listen for Probe |                |
|-------------->| Probe Req  |----------------->| CAPWAP Tunnel  |
                |<------------| Probe Resp       |--------------->|
                
|------------ Authentication Req ------------->|
                     (Open System Auth)
                |<----------- Authentication Resp ------------|
                
|------------ Association Request ------------>|
                (SSID, Supported Rates, MCS, Capabilities)
                |<----------- Association Response -----------|

AP forwards both auth/assoc mgmt frames to WLC over CAPWAP Control (UDP/5246).
```

# **4.4 Security Handshakes (WPA2/WPA3) – The Real Join Process**

Once associated at Layer 2, the **security process** begins.

---

## **4.4.1 WPA2-PSK (Personal) – 4-Way Handshake**

### Purpose:

- Derive encryption keys
    
- Prove possession of PSK
    
- Install PTK (Pairwise Transient Key)
    

### **Detailed Diagram: 4-Way Handshake**

```less
CLIENT STA                                     AP / WLC
-------------                                  -------------
     |                                             |
1.   |<-------------- Message 1 -------------------|
     |         (ANonce, RSN Information)          |
     |                                             |
2.   |-------------- Message 2 ------------------->|
     |         (SNonce, MIC using PMK)             |
     |                                             |
3.   |<-------------- Message 3 -------------------|
     |    (GTK, Install PTK, Key Confirmation)     |
     |                                             |
4.   |-------------- Message 4 ------------------->|
     |        (Confirm GTK, final MIC)             |
     |                                             |
     |---------- ENCRYPTED COMMUNICATION --------->|
```

## **4.4.2 WPA3-SAE (Simultaneous Authentication of Equals)**

Replaces PSK; resistant to offline dictionary attacks.

### Key differences:

- Uses **Dragonfly handshake**
    
- AP and client exchange public values
    
- Password-based Diffie-Hellman
    

---

## **4.4.3 WPA2/WPA3-Enterprise (802.1X / RADIUS)**

### Involves:

- Client STA
    
- AP
    
- WLC
    
- Cisco ISE (RADIUS server)
    

### Authentication types:

- EAP-TLS (preferred)
    
- PEAP
    
- EAP-TTLS
    

### **Complex Multi-Layer Diagram: 802.1X Authentication**

```less
CLIENT STA         AP          WLC (9800)                   RADIUS (Cisco ISE)
-----------       -----      ----------------             -----------------------
| EAPOL-Start |-->|   |------>|  EAPOL-to-RADIUS Proxy |-->| Access-Request      |
|             |   |   |       | (WLC encapsulates EAP)  |   | (EAP payload)       |
|<---EAP-Req--|   |   |<------|  Access-Challenge       |<--|                    |
|---EAP-Resp->|   |   |------>|  Access-Request        |-->|                    |
|<---EAP-Req--|   |   |<------| Access-Challenge       |<--|                    |
|---EAP-Resp->|   |   |------>| Access-Request         |-->|                    |
|<----------- SUCCESS ----------|<----------------------| SUCCESS                |
```

After success → 4-way handshake occurs → data encryption begins.

---

# **4.5 Power Saving (PSM, U-APSD)**

Important for mobile and IoT devices.

|Mechanism|Description|
|---|---|
|**Legacy PSM**|Client sleeps; AP buffers frames|
|**U-APSD (WMM Power Save)**|Trigger-based delivery for WMM ACs|
|**Target Wake Time (TWT)** (Wi-Fi 6)|Negotiated sleep schedule for IoT/low-power|

---

# **4.6 Channel Access – CSMA/CA (Detailed)**

The algorithm:

```less

1. Sense channel
2. Channel idle? → Wait DIFS
3. Pick random backoff
4. Countdown while idle
5. Transmit
6. Receive ACK
7. If no ACK → exponential backoff
```

Cisco WLC can modify contention parameters using **EDCA profiles**.

---

# **4.7 Roaming Operations (Critical CCNP Topic)**

## **4.7.1 L2 Roaming (Same Subnet)**

- AP forwards roam event to WLC
    
- WLC updates client’s AP mapping
    
- No tunnel needed
    
- Very fast (<50 ms)
    

---

## **4.7.2 L3 Roaming (Different Subnet)**

Requires **Mobility Tunnel** between WLCs.

Roles:

- **Anchor WLC** (holds client IP)
    
- **Foreign WLC** (client physically connected)

```less
CLIENT             AP(Foreign)         WLC(Foreign)            WLC(Anchor)
-------            -----------         -------------            -----------
Connect ------->      |                     |                      |
                      | CAPWAP Join         |                      |
                      |-------------------->|                      |
                      |                      Mobility Tunnel       |
                      |<------------------------------------------>|
                      |                                             |
Data flows from client → foreign AP → foreign WLC → mobility tunnel → anchor WLC → LAN
```

Client keeps IP despite subnet change.

---

## **4.7.3 Fast Roaming (802.11r)**

Supports fast BSS transitions:

- Fast Transition over the air
    
- Fast Transition over DS
    

Key difference:  
**PMK-R1 keys pre-derived** for neighbor APs.

---

# **4.8 Complex Multi-Layer Diagram — End-to-End WLAN Operation**

```less
+====================================================================================================+
|                                  FULL WLAN OPERATION PIPELINE                                      |
+====================================================================================================+

1. SCANNING
   CLIENT ----Probe Req---> AP
   CLIENT <---Probe Resp--- AP

2. AUTHENTICATION (Open or SAE or 802.1X/EAP)
   CLIENT ----Auth Req----> AP ----CAPWAP----> WLC
   CLIENT <---Auth Resp--- AP <----CAPWAP---- WLC

3. ASSOCIATION
   CLIENT ----Assoc Req---> AP ----CAPWAP----> WLC
   CLIENT <---Assoc Resp-- AP <----CAPWAP----- WLC

4. SECURITY (Depends on mode)
   WPA2-PSK: 4-way handshake
   WPA3-SAE: Dragonfly exchange
   802.1X: RADIUS EAP

5. DATA ENCRYPTION
   Client + AP install PTK
   AP encapsulates encrypted 802.11 frames into CAPWAP Data

6. DATA FLOW
   Client → AP → CAPWAP → WLC → Switch → Router → Server

7. ROAMING (optional)
   AP1 → AP2 → WLC Mobility
```

# **4.9 Summary Table (Exam & Real World)**

|Concept|CCNA|CCNP|Engineer|
|---|---|---|---|
|Frame types|Required|Deep|Used in troubleshooting|
|Auth/Assoc|Required|Deep|Critical|
|WPA2/3|Required|Deep handshake|Security validation|
|802.1X/EAP|Basic|Full workflow|Real enterprise|
|Roaming|Basic|Detailed|Multi-WLC design|
|Aggregation (A-MPDU)|Mentioned|Required|Performance tuning|


# **CHAPTER 5 — WLAN PLANNING, DESIGN & BEST PRACTICES**

Cisco-centric. Real engineer level. CCNA → CCNP → Enterprise Architect depth.  
Includes advanced RF design logic and complex ASCII diagrams.

---

# **5. WLAN PLANNING, DESIGN & BEST PRACTICES**

WLAN design is not a “coverage-only” task.  
It is a **capacity, performance, security, and application-first engineering discipline**.

Cisco (via CVD – Cisco Validated Designs) defines four major WLAN design pillars:

1. **Coverage (RF reachability)**
    
2. **Capacity (client count, bandwidth demand)**
    
3. **Performance (MCS rates, SNR, airtime utilization)**
    
4. **Reliability (roaming, redundancy, QoS)**
    

We engineer WLANs based on:

- Physical environment
    
- RF materials and attenuation
    
- Application usage
    
- Client device capabilities
    
- Regulatory domain restrictions
    
- AP model and antenna type
    

This chapter builds a complete design foundation.

---

# **5.1 Requirements Gathering (The Most Important Step)**

You **never** start WLAN design by placing APs.  
You begin with **requirements**.

---

## **5.1.1 Application Requirements**

|Application Type|Minimum RSSI|SNR|MCS|Airtime Requirement|Notes|
|---|---|---|---|---|---|
|Web browsing|-70 dBm|15 dB|low|low|tolerant to retries|
|Video streaming|-67 dBm|20–25 dB|medium|medium|jitter-sensitive|
|Voice (Cisco VoIP/Jabber)|-65 dBm|25–30 dB|high|very low|extremely latency-sensitive|
|High-density (auditorium)|-60 dBm|30 dB|high|minimal|requires directional antennas|

---

## **5.1.2 Client Device Requirements**

Different devices = different capabilities.

|Device|2.4 GHz|5 GHz|Streams|Notes|
|---|---|---|---|---|
|Laptop (modern)|yes|yes|2×2 / 3×3|high throughput|
|Smartphone|yes|yes|1×1 / 2×2|weaker radios|
|IoT|yes|often no|1×1|low power|
|Industrial scanners|yes|often weak|1×1|require robust roaming|

Weak radios → require higher density of APs.

---

# **5.2 Site Surveys – Predictive, Passive, Active**

## **5.2.1 Predictive Survey (Ekahau/Airmagnet)**

Uses floor plans + material loss values.

Pros:

- Fast
    
- Cheap
    
- Ideal early-stage design
    

Cons:

- Not fully accurate
    
- Requires validation
    

---

## **5.2.2 Passive Survey**

Laptop walks environment and listens to AP beacons.

Used for:

- Verifying RSSI
    
- Detecting co-channel interference
    
- Validating AP placement
    

---

## **5.2.3 Active Survey**

Client exchanges data with AP during measurement.

Used for:

- Throughput testing
    
- Roaming validation
    
- VoIP Wi-Fi readiness
    
- MCS analysis
    

---

# **5.3 Channel Planning (Critical for Performance)**

Primary design rule:

**2.4 GHz: Use channels 1 / 6 / 11 only.**  
(They are the only non-overlapping channels.)

**5 GHz/6 GHz: Use as many non-overlapping channels as possible.**

---

## **5.3.1 Co-Channel vs Adjacent-Channel Interference**

### Co-Channel Interference (CCI)

Two APs using the **same channel** → they share airtime → lower throughput.

### Adjacent-Channel Interference (ACI)

Two APs using **overlapping channels** → corrupt each other’s frames → severe performance drop.

---

# **Complex RF Diagram — Channel Overlap (2.4 GHz)**

```less
Frequency (MHz)
|--------------------------------------------------------------------------------|
2400        2412        2437        2462        2484
       CH1 ---------
                    \_________
                             CH6 ---------
                                         \_________
                                                  CH11 ---------
```


Channels 2–5 and 7–10 **overlap and cause interference** → never use them.

---

# **5.4 Power Planning (Tx Power Strategy)**

A common mistake:

**Setting AP power too high.**

This causes:

- Large cell sizes
    
- Excessive roaming delay
    
- CCI increase
    
- Clients unable to transmit back at equal strength
    

Wi-Fi clients typically transmit between **8 and 15 dBm**, much weaker than APs.

Therefore:

**AP Tx power should be kept in the 8–14 dBm range.**

Cisco RRM (Radio Resource Management) handles this automatically if configured properly.

---

# **5.5 AP Placement – Engineering Rules**

### **Rule 1: Ceiling-mounted APs perform best**

- Use 2.7–3.0 m height
    
- Avoid above 4 m (signal spreads too widely)
    

### **Rule 2: Avoid mounting near:**

- Metal ducts
    
- Concrete support pillars
    
- Elevator shafts
    
- Microwave ovens
    
- Heating systems
    
- Thick reinforced concrete
    

### **Rule 3: Maintain AP spacing**

- ~15 m apart for indoor office
    
- Adjust based on attenuation maps
    

---

# **5.6 High-Density Design (Stadiums, Auditoriums, Classrooms)**

## Techniques:

- Use directional antennas (Cisco AIR-ANT2513P4M etc.)
    
- Reduce AP Tx power
    
- Increase AP count
    
- Reduce SSID count
    
- Use 20 MHz channels
    
- Enable Cisco **Load Balancing**
    
- Use **band steering** to push clients to 5 GHz
    

---

# **5.7 Complex Multi-Layer Diagram – Enterprise Building WLAN Design**

```less
+=================================================================================================+
|                                   ENTERPRISE WLAN DESIGN MODEL                                 |
+=================================================================================================+


                              5 GHz Design (Primary Band)
                    CH36         CH44         CH149         CH157
             +----------------+-----------+-----------+----------------+
             |     AP1        |    AP2    |    AP3    |      AP4       |
             | Tx 10 dBm      | Tx 12 dBm | Tx 8 dBm  | Tx 11 dBm      |
             | RSSI Targets: -65 dBm for Voice                                         |
             +----------------+-----------+-----------+----------------+

Floor Attenuation Map:
------------------------
Concrete wall: -10 dB
Glass: -4 dB
Drywall: -3 dB
Furniture: -1 to -3 dB


                 +------------ Drywall (-3 dB) ------------+
                 |                                          |
     AP1 (CH36)  |           Open Office                    |   AP2 (CH44)
   RSSI -62 dBm  |                                          |  RSSI -65 dBm
                 +------ Glass Wall (-4 dB) ----------------+
                          |
                          | Meeting Room: 20 people (~50 dB dynamic loss)
                          |
     AP3 (CH149)  <-------+
     Directional Panel Antenna for High-Density Environment


Legend:
* Thick lines = High attenuation
* Thin lines = Low attenuation
* Overlap zones evaluated for CCI
* Cisco RRM adjusts channels/power every 10 min (default)
```

# **5.8 SSID Planning & Airtime Efficiency**

Adding SSIDs increases beacon traffic exponentially.

### Beacon overhead:

- ~100 bytes
    
- Transmitted every 100 ms
    
- Sent at lowest mandatory rate (often 1 or 6 Mbps)
    

### Rule:

**Use no more than 3–4 SSIDs in enterprise deployments.**

More SSIDs = wasted airtime = reduced throughput.

---

# **5.9 VLAN & QoS Design for Wireless**

### **Wireless VLAN Guidelines:**

- One VLAN per SSID (Cisco best practice)
    
- Do not stretch too many VLANs everywhere
    
- Use 802.1X for dynamic VLAN assignment if needed
    

### **QoS (WMM):**

Four access categories:

1. Voice (AC_VO)
    
2. Video (AC_VI)
    
3. Best Effort (AC_BE)
    
4. Background (AC_BK)
    

Cisco Catalyst 9800 allows custom EDCA profiles.

---

# **5.10 Cisco RRM (Radio Resource Management)**

RRM is a primary differentiator of Cisco WLAN.

Functions:

### **Dynamic Channel Assignment (DCA)**

- Automatic channel selection
    
- Reacts to interference
    
- Avoids DFS events
    

### **Transmit Power Control (TPC)**

- Adjusts AP Tx power dynamically
    
- Prevents overpowering
    
- Improves roaming behavior
    

### **Coverage Hole Detection (CHD)**

Alerts when clients report poor RSSI.

### **RF Grouping**

APs form RF Groups over CAPWAP.

---

# **5.11 Summary Table – WLAN Design Overview**

| Design Component | CCNA      | CCNP              | Real Deployment     |
| ---------------- | --------- | ----------------- | ------------------- |
| AP Placement     | Basic     | Critical          | Required            |
| Channel Planning | Required  | Deep              | Mandatory           |
| Power Planning   | Mentioned | Deep tuning       | Mandatory           |
| Surveys          | Basic     | All survey types  | Real-world          |
| High-density     | No        | Yes               | Stadiums, events    |
| RRM              | Basic     | Deep              | Enterprise standard |
| SSID design      | Required  | Enterprise policy | Critical            |


# **CHAPTER 6 — WIRELESS SECURITY (CISCO ENTERPRISE SECURITY)**

Enterprise-grade, CCNA → CCNP → Professional Wireless Engineer level.  
Includes full WPA2/WPA3 crypto workflows, Cisco ISE integration, key hierarchies, WIPS/WIDS, rogue detection, DoS protections, and complex multi-layer diagrams.

---

# **6. Wireless Security (Enterprise-Level, Cisco Focus)**

Cisco wireless security is built around **strong authentication, encryption, policy enforcement, and continuous threat monitoring**.  
A secure WLAN requires:

1. **Strong identity (802.1X / ISE / certificates)**
    
2. **Strong encryption (WPA2/WPA3)**
    
3. **Secure key-handling mechanisms**
    
4. **Threat detection (WIPS/WIDS)**
    
5. **Rogue containment**
    
6. **Segmentation & policy enforcement**
    
7. **Secure AP ↔ WLC communication (DTLS CAPWAP)**
    

This chapter builds all necessary foundations.

---

# **6.1 Security Modes Overview (Cisco)**

|Security Mode|Use Case|Strength|Notes|
|---|---|---|---|
|**Open**|Guest portals|None|Captive portals add auth after connection|
|**WEP (deprecated)**|None|Broken|Should never be used|
|**WPA2-PSK (Personal)**|Home/small business|Medium|Weak vs dictionary attacks|
|**WPA2-Enterprise (802.1X)**|Enterprise|Strong|Based on RADIUS & EAP|
|**WPA3-SAE (Personal)**|Modern enterprise|Strong|Resistant to offline attacks|
|**WPA3-Enterprise**|Government, high security|Very strong|Suite-B / CNSA ciphers|
|**OWE (Opportunistic Wireless Encryption)**|Open encrypted SSID|Medium|No authentication|

---

# **6.2 Encryption and Key Hierarchy (WPA2/WPA3 Deep Dive)**

## **6.2.1 Key Hierarchy – Big Picture**

```less
PMK (Pairwise Master Key)
   ↓ derives
PTK (Pairwise Transient Key)
   ↓ splits into
KCK (Key Confirmation Key)
KEK (Key Encryption Key)
TK  (Temporal Key for data encryption)
```

### PMK origin:

- WPA2-PSK: Derived from password
    
- WPA2-Enterprise: Derived from 802.1X authentication (MSK → PMK)
    
- WPA3-SAE: Derived from Dragonfly handshake
    

---

# **6.3 Detailed Operation of WPA2 Security**

## **6.3.1 WPA2-PSK (Personal)**

### How PMK is created:

```less
PMK = PBKDF2(PSK, SSID, 4096 iterations)
```

### Weakness:

If PSK is weak → offline dictionary attacks are trivial.

---

## **6.3.2 WPA2-Enterprise (802.1X)**

### Components:

- **Supplicant (client)**
    
- **Authenticator (AP/WLC)**
    
- **Authentication Server (Cisco ISE – RADIUS)**
    

### Result of 802.1X:

ISE generates **MSK → PMK**, securely delivered to WLC.

---

# **6.4 WPA3 Security (Modern Standard)**

WPA3 introduces **stronger cryptography and protection against offline cracking**.

## **6.4.1 WPA3-SAE (Simultaneous Authentication of Equals)**

Replaces PSK.  
Performs **password-authenticated key exchange (PAKE)** using the Dragonfly protocol.

### Advantages:

- Cannot be brute-forced offline
    
- Each session uses unique key material
    
- Protects against passive attackers
    

---

## **6.4.2 WPA3-Enterprise**

Two modes:

|Mode|Cipher|Purpose|
|---|---|---|
|**128-bit**|AES-GCMP-128|Standard enterprise|
|**192-bit (Suite B)**|AES-GCMP-256, SHA-384|Government/Defense|

---

# **6.5 The 4-Way Handshake (Full Crypto-Level Explanation)**

### Purpose:

- Prove client & AP share PMK
    
- Generate PTK
    
- Install group keys
    

---

## **Complex Multi-Layer Diagram — WPA2/WPA3 4-Way Handshake**

```less
CLIENT STA                                             AP/WLC
-----------------------------------------------------------------------------------------------
            (PMK known to both sides via PSK or 802.1X)

1.  <------------------------------------ ANonce -----------------------------------------------

    - AP/WLC sends random nonce (ANonce)
    - Includes RSN capabilities

2.  SNonce + MIC(PMK) -------------------------------------------------------------------------->
    
    - Client generates SNonce
    - Derives PTK = PRF(PMK, ANonce, SNonce, MACs)
    - Sends EAPOL-Key message with MIC proving PMK possession

3.  <------------------------------------ GTK + PTK Install -------------------------------------

    - AP/WLC derives PTK same way
    - Sends Group Temporal Key (GTK) encrypted using KEK
    - Instructs client to install PTK

4.  EAPOL-Key Ack ------------------------------------------------------------------------------>

    - Client confirms installation
    - Secure communication can begin
```

**All further traffic is encrypted with TK (Temporal Key).**

---

# **6.6 Cisco 802.1X + ISE Architecture (Enterprise Standard)**

### Why 802.1X?

- Identity-based access
    
- Certificates (EAP-TLS) for strongest security
    
- Dynamic VLAN assignment
    
- Policy-driven access (ISE)
    
- Full auditing and accounting
    

---

# **6.7 Complex Multi-Layer Diagram — Cisco 802.1X Authentication Flow**

```less
CLIENT STA            AP              WLC (Catalyst 9800)                Cisco ISE (RADIUS)
====================================================================================================

EAPOL-Start ------->  |              |                                   |
                      | Forward EAP  |---RADIUS Access-Request (EAP)---->|
                      |-------------->                                   |
<---EAPOL-Req---------|              |<--Access-Challenge (EAP)----------|
                      |-------------->                                   |
EAPOL-Resp----------->|              |---Access-Request (EAP)----------->|
                      |              |                                   |
 ... Exchanges repeated until authentication completes ...

                         Final Access-Accept (contains MSK)
                      |<-----------------------------------------------|
                      |
                      |---Install PMK/Session Keys--------------------->|
                      |
4-Way Handshake occurs (WLC ↔ Client)

DATA ENCRYPTED
```


The **WLC acts as authenticator**, forwarding EAP to ISE.

---

# **6.8 Cisco Wireless Segmentation & Policy Enforcement**

Segmentation is part of security.

Cisco supports:

### **VLAN-based segmentation**

Each SSID → VLAN → firewall rules.

### **Dynamic VLAN assignment**

ISE RADIUS returns VLAN ID or SGT.

### **SGT (Security Group Tagging) – Cisco TrustSec**

Allows micro-segmentation:

- Firewall rules apply to identities, not IPs
    
- Works in Catalyst switches & WLC
    

---

# **6.9 Wireless Threats (Offensive View)**

## **6.9.1 Rogue AP**

Unauthorized AP connected to company network.

Cisco WLC detection:

- **Rogue Detector Mode APs**
    
- **Monitor Mode APs**
    

Classification:

- Malicious
    
- Friendly
    
- External
    

---

## **6.9.2 Evil Twin**

Attacker copies SSID to steal credentials.

Prevention:

- WPA3-SAE
    
- 802.1X + certificates
    
- WLC rogue alarms
    

---

## **6.9.3 Deauthentication Attacks**

Uses spoofed deauth frames.

Mitigations:

- 802.11w Protected Management Frames (PMF)
    
- WPA3 requires PMF
    

---

## **6.9.4 KRACK Attack (WPA2 Vulnerability)**

Key reinstallation issue.

Fixed by:

- Patching clients/APs
    
- Using WPA3
    

---

# **6.10 Cisco Wireless IPS/WIDS (Embedded or MSE)**

Cisco provides Wireless Intrusion Prevention:

### **Detection:**

- Rogue APs
    
- Rogue clients
    
- Ad-hoc networks
    
- Spoofed MACs
    
- Flood attacks
    
- Deauth/Disassoc attacks
    
- Fake beacons
    
- Impersonation attacks
    

### **Prevention (Containment):**

- APs send deauth frames to rogue devices
    
- Very effective but must be used legally
    

---

# **Complex Diagram — WIPS Architecture**

```less
                           +----------------------------+
                           |       Cisco WLC (9800)    |
                           |  Rogue Management, WIPS    |
                           +-------------^--------------+
                                         |
                           CAPWAP Rogue Alerts
                                         |
   +-------------------------+       +--------------------------+
   | AP in Monitor Mode      |       | AP in Local Mode         |
   | Continuous scanning      |       | Serves clients, scans    |
   +-----------^--------------+       +-----------^--------------+
               |                                  |
        RF Monitoring                       RF & Rogue Detection
               |                                  |
               +---------------+------------------+
                               |
                         RF Environment
```

# **6.11 Protected Management Frames (PMF)**

Required for WPA3, optional for WPA2.

Protects:

- Disassociation
    
- Deauthentication
    
- Action frames
    

Prevents many DoS-style attacks.

---

# **6.12 Guest Access Security**

Cisco recommends:

- Open SSID
    
- Redirection to captive portal (ISE or WLC built-in)
    
- DHCP, firewall, rate limiting
    
- VLAN isolation
    
- ACL restricting traffic
    

Optionally:  
**OWE** (encrypted, no auth) for modern clients.

---

# **6.13 Summary Table – Wireless Security**

|Topic|CCNA|CCNP|Real Deployment|
|---|---|---|---|
|WPA2-PSK|Required|Basic crypto|Home/small office|
|WPA2-Enterprise|Required|Deep|Enterprise standard|
|WPA3|Mentioned|Required|Modern deployments|
|802.1X|Required|Deep (ISE flows)|Mandatory|
|Key Hierarchy|Basic|Deep crypto|Troubleshooting|
|WIPS|Not required|Required|Compliance/security|
|PMF|Mentioned|Detailed|WPA3 requirement|

# **CHAPTER 7 — WLAN CONFIGURATION (Cisco Catalyst 9800, AireOS, APs, Switches)**

This chapter is _fully practical_: controller configuration, AP join verification, switch setup, FlexConnect, RF profiles, and all required CLI in **one continuous engineer-format block per topic** (your preferred style).  
Cisco-centric. CCNA → CCNP depth.

---

# **7. WLAN CONFIGURATION (ENGINEER PRACTICAL GUIDE)**

The goal:  
Understand **how to configure WLANs on Cisco Catalyst 9800 controllers** and how APs, switches, VLANs, and policies integrate.

We include references to AireOS where needed, but the primary platform is **Catalyst 9800**.

---

# **7.1 Components of a Cisco WLAN Configuration (Catalyst 9800)**

Catalyst 9800 uses the following main building blocks:

|Component|Purpose|
|---|---|
|**WLAN Profile**|Defines SSID, security, QoS|
|**Policy Profile**|VLAN, ACL, session timeout, PMF, AAA settings|
|**Policy Tag**|Assigns Policy + Site + RF tags to APs|
|**RF Profile**|Channel, power, RRM config|
|**Flex Profile**|Required for FlexConnect APs|
|**AP Join Profile**|AP discovery & join settings|
|**AP Tags**|Determines which AP uses which configs|

**AireOS simply had: WLAN → AP Group → Interface**, but Catalyst 9800 is modular.

---

# **7.2 Switch Configuration (Catalyst 2960-X and 9300)**

Every AP must connect to a switchport configured as a **trunk** with a native VLAN for AP management.

## **Trunk configuration (2960-X / 9300)**

_Engineer-format, one block:_

```less
# SWITCHPORT CONFIG FOR CISCO LIGHTWEIGHT AP (Catalyst 2960-X / Catalyst 9300)
conf t
!
interface GigabitEthernet1/0/24     # AP uplink port
 description AP-9105-Office
 switchport mode trunk              # required for LWAP
 switchport trunk native vlan 10    # AP mgmt VLAN
 switchport trunk allowed vlan 10,20,30,40
 spanning-tree portfast trunk
 power inline auto                  # PoE for AP
!
end
wr mem
```


**Notes:**

- The AP uses **the native VLAN** to request DHCP and find WLC.
    
- VLANs 20/30/40 are for SSIDs (e.g., Staff, Voice, Guest).
    
- Always enable **portfast** on AP ports.
    

---

# **7.3 WLC Discovery (How APs Find the Controller)**

APs find a controller using:

1. **Layer 2 broadcast (same VLAN)**
    
2. **DHCP Option 43**
    
3. **DNS: `cisco-capwap-controller.localdomain`**
    
4. **Static configuration (priming)**
    

---

# **7.4 AP Join Process (Catalyst 9800)**

The AP join flow:

```less
DISCOVERY → DTLS Establishment → CONFIG DOWNLOAD → RUN STATE
```

To verify:

```less
show ap summary
show ap config general <AP-NAME>
show logging | include CAPWAP
debug capwap events enable
debug capwap errors enable
```

# **7.5 Creating a WLAN (SSID) on Catalyst 9800**

A WLAN requires:

- WLAN Profile
    
- Policy Profile
    
- Policy Tag
    
- VLAN interface on switch
    
- AP Tag mapping
    

## **7.5.1 Engineer Code Block — Create WLAN (PSK example)**

```less
# CATALYST 9800 - CONFIGURE WLAN "CORPORATE" WITH WPA2-PSK
conf t
!
wlan CORPORATE 1 CORPORATE
 ssid CORPORATE
 security wpa psk set-key ascii <SUPER-SECRET-PASS>
 no shutdown
!
end
wr mem
```

## 7.6 Creating a Policy Profile (VLAN, ACLs, PMF)

```less
# POLICY PROFILE FOR CORPORATE WLAN
conf t
!
wireless profile policy CORPORATE-POLICY
 vlan 20                             # attach WLAN to VLAN 20
 no shutdown
 pmf optional                        # WPA2-PSK → PMF optional
 aaa-override                        # allow ISE dynamic policies
 accounting-list default
 session-timeout 86400
!
end
```

## 7.7 Binding Policy + RF + Site Profiles to APs (Policy Tagging)

```less
# CREATE POLICY TAG
conf t
!
wireless tag policy CORPORATE-TAG
  wlan CORPORATE policy CORPORATE-POLICY
!
end

# ASSIGN TAG TO AP
conf t
!
ap NAME-OF-AP
 policy-tag CORPORATE-TAG
 site-tag  Default-Site
 rf-tag     Default-RF-Profile
!
end
```

## 7.8 RF Profile (Tuning RRM, Channels, Power)

```less
# RF PROFILE FOR 5 GHz
conf t
!
wireless profile rf 5GHZ-CORP
 band 5ghz
 channel width 20mhz             # improve stability and reduce CCI
 max-client-count 50
 rrm dca-channel 36 40 44 48 149 153 157 161
 rrm dca-threshold -70
 rrm tpc-threshold -65
!
end
```

# **7.9 FlexConnect Configuration (Remote Branch AP)**

For APs not tunneling traffic back to WLC.

## Steps:

1. Flex profile
    
2. Enable local switching
    
3. Assign VLAN mapping per SSID
    
4. Apply Flex policy tag to APs
    

### **FlexConnect Engineer Config**

```less
# FLEX PROFILE
conf t
wireless profile flex FLEX-BRANCH
 vlan-based
 central-dhcp disable
 central-nat disable
!
end

# POLICY PROFILE (LOCAL SWITCHING)
conf t
wireless profile policy BRANCH-POLICY
 local-switching               # key FlexConnect feature
 vlan 30                       # VLAN at remote site
 no shutdown
!
end

# POLICY TAG FOR FLEX
conf t
wireless tag policy BRANCH-TAG
 wlan BRANCH-WLAN policy BRANCH-POLICY
!
end

# APPLY TAG TO FLEX AP
conf t
ap Branch-AP-1
 policy-tag BRANCH-TAG
 site-tag FLEX-SITE
 rf-tag DEFAULT-RF
!
end
```

# **7.10 Guest WLAN Configuration (Open + Captive Portal)**

Two options:

- WLC internal portal
    
- ISE centralized guest portal
    

### **Engineer Example (WLC Internal Portal)**

```less
# CREATE GUEST WLAN
conf t
wlan GUEST 5 GUEST
 ssid GUEST
 no security wpa
 no security wpa wpa2
 no security dot1x
 security web-auth
 no shutdown
!
end

# GUEST VLAN IN POLICY PROFILE
conf t
wireless profile policy GUEST-POLICY
 vlan 30
 webauth parameter-map GUEST-MAP
!
end
```

## 7.11 WPA2-Enterprise (802.1X + ISE) – Full Controller Config

```less
# AAA CONFIGURATION ON WLC
conf t
aaa new-model
radius server ISE-PRIMARY
 address ipv4 10.10.10.50 auth-port 1812 acct-port 1813
 key SUPER-SECRET-RADIUS-KEY
!
aaa group server radius ISE-GRP
 server name ISE-PRIMARY
!
aaa authentication dot1x default group ISE-GRP
!
end

# WLAN WITH DOT1X
conf t
wlan CORP-DOT1X 8 CORP-DOT1X
 ssid CORP-DOT1X
 security dot1x
 no shutdown
!
end

# POLICY PROFILE
conf t
wireless profile policy DOT1X-POLICY
 vlan 20
 aaa-override                 # allow VLAN assignment from ISE
 nac                         # network admission control
!
end
```

## 7.12 AireOS Reference (Legacy Platforms)

Equivalent basic SSID config on AireOS WLC:

```less
# CREATE WLAN ON AIREOS
config wlan create 1 CORPORATE CORPORATE
config wlan security wpa akm psk set-key ascii 1 SUPERSECRET
config wlan enable 1

```

Both will coexist in network engineering until AireOS is fully retired.

---

# **7.13 AP Debugging – Joining, Discovery, DTLS, CAPWAP**

Useful commands on the WLC:

```less
show ap summary
show ap join stats detailed <AP>
show ap config general <AP>
show wireless management trustpoint

debug capwap events enable
debug capwap detail enable
debug dtls detail enable
```

On AP console:

```less
debug capwap client no-reload
show capwap client
show capwap ip config
```

## 7.14 Complex Multi-Layer Diagram — End-to-End WLAN Configuration Pipeline

```less
+====================================================================================================+
|                         WLAN CONFIGURATION FLOW ON CATALYST 9800                                   |
+====================================================================================================+

1. SWITCH (2960-X / 9300)
   - AP port trunked
   - Native VLAN = mgmt VLAN
   - PoE enabled
   - VLANs for all SSIDs exist

2. WLC CONFIGURATION
   +---------------------+     +----------------------+     +------------------------+
   | WLAN PROFILE        |     | POLICY PROFILE       |     | TAGS                   |
   | - SSID              | --> | - VLAN assignment    | --> | - Policy Tag           |
   | - WPA2/WPA3         |     | - ACLs               |     | - RF Tag               |
   | - QoS               |     | - PMF                |     | - Site Tag             |
   +---------------------+     +----------------------+     +------------------------+

3. AP JOIN
   - Discovery (L2/DHCP/DNS)
   - CAPWAP DTLS tunnel setup
   - Config download
   - Tags applied

4. CLIENT CONNECTION
   - Probe → Auth → Assoc
   - WPA2/WPA3 4-way handshake
   - DHCP (Dynamic VLAN from ISE optional)
   - Encrypted data flow

5. DATA PATH
   Local Mode:
       CLIENT → AP → CAPWAP → WLC → Switch → Router → Internet/LAN

   FlexConnect:
       CLIENT → AP → Local switch → Router → LAN
```

# **7.15 Summary Table – Practical Skills**

|Task|CCNA|CCNP|Engineer|
|---|---|---|---|
|SSID creation|Required|Advanced|Daily task|
|WPA2/3 config|Required|Detailed|Mandatory|
|FlexConnect|Basic|Deep|Branch offices|
|AP join troubleshooting|Not deep|Required|Critical|
|Policy profiles/tags|Mentioned|Required|All deployments|
|RF profiles|Not required|Required|Performance tuning|
|802.1X + ISE|Required|Deep|Enterprise standard|

# **CHAPTER 8 — WIRELESS TROUBLESHOOTING (CISCO ENTERPRISE)**

Full engineer-level troubleshooting.  
CCNA → CCNP → Real-world network engineer.  
Includes multi-layer diagnostic models, AP join failures, RF troubleshooting, security handshake failures, CAPWAP issues, and complex ASCII diagrams.

---

# **8. Wireless Troubleshooting (End-to-End)**

Troubleshooting wireless networks requires **layered, structured analysis**.  
Cisco Catalyst 9800, AireOS, and lightweight AP architectures demand:

- Understanding of AP join states
    
- RF analysis (SNR, RSSI, CCI, ACI)
    
- Security analysis (802.1X, 4-way handshake, PMF)
    
- VLAN and switching validation
    
- CAPWAP control and data path troubleshooting
    
- Client-side logs and Wireshark captures
    

This chapter provides the complete methodology.

---

# **8.1 Troubleshooting Framework – Layered Approach**

Wireless failures are grouped into **four domains**:

1. **RF Layer (Physical)**
    
2. **Frame Exchange Layer (802.11 MAC)**
    
3. **Security Layer (802.1X/WPA2/WPA3)**
    
4. **Network Layer (DHCP, VLAN, routing, QoS)**
    

A failure at any layer blocks connectivity.

---

# **8.2 Global Multi-Layer Troubleshooting Diagram**


```less
+========================================================================================================+
|                                   WLAN TROUBLESHOOTING FLOW (ENGINEER)                                |
+========================================================================================================+

LEVEL 1 – RF LAYER
    - Is the AP transmitting? Is channel correct?
    - RSSI > -67 dBm? SNR > 20 dB?
    - CCI / ACI issues? Interference sources?

LEVEL 2 – 802.11 MAC LAYER
    - Client scanning?
    - Probe request/response?
    - Authentication frame exchange?
    - Association request/response?

LEVEL 3 – SECURITY LAYER
    - WPA2/WPA3 handshake?
    - 802.1X/EAP success?
    - PMK/GTK installation errors?
    - PMF negotiation issues?

LEVEL 4 – NETWORK LAYER
    - DHCP success?
    - Correct VLAN assignment?
    - NAT/firewall rules?
    - DNS working?
    - QoS restrictions?

LEVEL 5 – WLC/AP CONTROL PLANE
    - CAPWAP control up?
    - AP join successful?
    - DTLS errors?
    - Policies/tags applied?

LEVEL 6 – DATA PLANE
    - Is traffic bridged or tunneled correctly?
    - FlexConnect local switching?
    - ACLs blocking?
    - Routing issues?

LEVEL 7 – APPLICATION LAYER
    - Latency, jitter, packet loss?
    - Voice/video performance?
```

# **8.3 Tools for WLAN Troubleshooting**

## **On WLC (Catalyst 9800):**

```less
show wireless client summary
show wireless client mac <MAC>
show ap summary
show ap config general <AP-NAME>
show ap join stats detailed <AP>
show wireless policy summary
show wireless tag policy
show wireless rrm
show wireless wps statistics    # security stats
```

## Debug commands:

```less
debug wireless mac <MAC>
debug capwap events enable
debug capwap errors enable
debug dtls errors enable
debug dot1x all enable
```

## On Access Points (console):

```less
show capwap client
debug capwap client no-reload
debug capwap errors
debug dot11 events
debug dot11 state
```


## **Tools:**

- Wireshark (802.11 captures using monitor AP or external adapter)
    
- Ekahau/Airmagnet for RF
    
- Spectrum analyzer (Cisco CleanAir if supported)
    
- Client-side logs
    

---

# **8.4 Troubleshooting AP Join Failures**

APs may fail to join WLC if:

- Wrong VLAN
    
- DHCP Option 43 missing
    
- DNS discovery fails
    
- Controller unreachable
    
- DTLS errors
    
- Image mismatch
    
- Regulatory domain mismatch
    
- Certificate expiration (for APS running mTLS/DTLS)
    

---

## **8.4.1 AP Join Flow (Catalyst 9800)**

```less
DISCOVERY → JOIN → CONFIG → RUN
```

Failure? Look at:

```less
show ap join statistics detailed <AP>
```

## 8.5 AP Join Failure – Multi-Layer Diagnostic Diagram

```less
AP BOOT
  |
  +-- LAYER 2: Does AP get IP address?
  |       - DHCP working?
  |       - Native VLAN correct?
  |
  +-- DISCOVERY:
  |       - L2 broadcast?
  |       - DHCP Option 43?
  |       - DNS: cisco-capwap-controller?
  |
  +-- JOIN REQUEST:
  |       - Does WLC see AP?
  |       - Model supported?
  |       - Regulatory domain match?
  |
  +-- DTLS CONTROL CHANNEL:
  |       - Certificates valid?
  |       - Time synchronized?
  |
  +-- CONFIG DOWNLOAD:
  |       - Policy tags exist?
  |       - RF tags assigned?
  |
  +-- RUN STATE
          - AP online & serving clients
```

# **8.6 Common AP Join Issues & Fixes**

### **1. Wrong Native VLAN**

Symptoms:

- AP does not get IP
    
- `show capwap ip config` shows 0.0.0.0
    

Fix:

```less
switchport trunk native vlan <correct-vlan>
```

### **2. DHCP Option 43 incorrect**

Symptoms:

- AP keeps discovering but does not join
    

Fix:  
Correct format:

```less
Option 43: f104.c0a8.0a01   # example for 192.168.10.1
```

### **3. DNS discovery fails**

Ensure:

```less
cisco-capwap-controller.<domain>
```

### **4. DTLS certificate expiration (common with older APs)**

Fixes:

- Fix AP time (via DHCP option 42 NTP)
    
- Upgrade AP image
    
- Disable SSC validation (not recommended)
    

---

### **5. Regulatory domain mismatch**

AP region ≠ WLC region  
AP refuses to join.

Fix:

- Use APs with same -xxx regulatory suffix
    

---

# **8.7 Troubleshooting Client Connectivity (Authentication/Association)**

Failures occur in four stages:

|Stage|Symptoms|Likely Cause|
|---|---|---|
|Scanning|SSID not visible|AP down, RF issue, SSID disabled|
|Authentication|Client stuck at auth|Wrong credentials, PSK mismatch, cert issues|
|Association|Assoc request rejected|Unsupported rates, PMF mismatch|
|4-way handshake|Connect then disconnect|Wrong PSK, PMK mismatch, MIC errors|

---

# **8.8 Complex Diagram — Client Join Failure Analysis**

```less
CLIENT                           AP                        WLC
-----------------------------------------------------------------------------------------
SCAN
 |---- Probe Req --->            |                        |
 |<--- Probe Resp ---            |                        |
(Missing? Check RF, SSID, AP status)

AUTHENTICATION
 |---- Auth Req ---->            |                        |
 |<--- Auth Resp ---             |--- CAPWAP Auth ------->|
(Auth fails? Check PSK or 802.1X)

ASSOCIATION
 |---- Assoc Req -->             |                        |
 |<--- Assoc Resp -X (Failure)   |                        |
(Assoc failure? Check PMF, rates, capabilities mismatch)

4-WAY HANDSHAKE
 |---- Msg2 ------->             |                        |
 |<--- Msg3 -X (MIC Error)       |                        |
(WPA failure? Check PSK, RADIUS)
```

# **8.9 Troubleshooting 802.1X / WPA2-Enterprise**

Common failures:

|Symptom|Cause|Fix|
|---|---|---|
|Client stuck at “Authenticating”|Wrong EAP method|Set EAP-TLS/PEAP properly|
|EAP failure in WLC logs|Certificate trust issue|Install CA root on client|
|No VLAN assignment|AAA override disabled|Enable in policy profile|
|Random disconnects|Session timeout|Increase in ISE or WLC|
|Success but no IP|VLAN not trunked|Check switch config|

---

## **8.9.1 WLC Debug Example (802.1X)**

```less
debug dot1x all enable
debug aaa events enable
debug client <MAC>
```

# **8.10 Troubleshooting WPA2/WPA3 Handshake Failures**

Symptoms:

- Wrong PSK → fails at Msg3
    
- PMK mismatch → fails at Msg2
    
- PMF mismatch → association refusal
    
- WPA3 SAE → fails at commit/confirm messages
    

---

# **8.11 Troubleshooting DHCP Issues**

### Checklist:

- Correct WLAN → VLAN mapping in policy profile
    
- VLAN trunked on switch
    
- DHCP server reachable
    
- No ACL blocking DHCP ports (67/68)
    
- IP helper-address on L3 interface
    

### WLC Commands:

```less
show wireless client mac <MAC> detail
```

Look for:

```less
DHCP state: Not received
```

# **8.12 RF Troubleshooting (SNR, CCI, ACI)**

## **8.12.1 Low RSSI**

- AP too far
    
- Incorrect mounting
    
- High attenuation environment
    

## **8.12.2 Low SNR**

- Noise floor high
    
- Interference (microwaves, Bluetooth, radar)
    

## **8.12.3 High CCI**

- Too many APs on same channel
    
- Power too high
    

## **8.12.4 High ACI**

- Overlapping channels (2.4 GHz)
    

---

# **8.13 Complex RF Troubleshooting Diagram**

```less
+===============================================================================================+
|                                 RF TROUBLESHOOTING DECISION TREE                              |
+===============================================================================================+

RSSI < -70 dBm ?
   |
   +---> Yes → Increase AP density / move AP / reduce obstacles
   |
   +---> No →
            SNR < 20 dB ?
                |
                +---> Yes → Check interference (microwave, BT, radar)
                |
                +---> No →
                       CCI > threshold ?
                           |
                           +---> Yes → Reduce AP Tx power / adjust channel plan
                           |
                           +---> No → Check ACI (2.4 GHz overlapping channels)
```


# **8.14 Troubleshooting FlexConnect**

### Issues:

- AP local switching incorrect VLAN
    
- ACLs blocking DHCP on remote site
    
- No local RADIUS server configured for Local Auth
    
- Flex Profile missing
    

Check:

```less
show ap name <AP> config general
show ap name <AP> config flex
```

# **8.15 Troubleshooting WIPS/WIDS Events**

Common alerts:

|Alert|Meaning|Action|
|---|---|---|
|Rogue AP detected|Unauthorized AP present|Locate and isolate|
|Ad-hoc network|Client-to-client network|Disable via policy|
|Deauth flood|DoS attack|Enable PMF|
|Spoofed AP|Evil twin|Add MAC to block list|

---

# **8.16 Troubleshooting CAPWAP Data Plane**

Local Mode:

```less
CLIENT → AP → CAPWAP → WLC → LAN
```

Issues:

- WLC interface misconfigured
    
- Policy tag missing
    
- MTU mismatch
    

Debug:

```less
debug capwap data enable
show wireless client mac <MAC> detail
```

FlexConnect:

```less
CLIENT → AP → Switch → LAN
```

Issues:

- VLAN mapping
    
- ACL blocking routing
    
- Local DHCP server missing
    

---

# **8.17 Summary Table – Troubleshooting**

|Issue Type|Tools|Fix|
|---|---|---|
|AP join|CAPWAP/DTLS debug|Fix discovery, certs, VLAN|
|Auth/Assoc|MAC frame analysis|Fix PSK, PMF, rates|
|802.1X|AAA debug, ISE logs|Fix certs, EAP, VLAN|
|RF|SNR/RSSI/CCI|Adjust AP layout|
|DHCP|Switch/WLC logs|Fix VLAN, helper-address|
|CAPWAP|WLC/AP logs|Fix MTU, routing|
|FlexConnect|AP config|Fix VLAN mapping|


# **CHAPTER 9 — ADVANCED WIRELESS (CCNP ENCOR LEVEL)**

This chapter covers the _professional engineer_ concepts behind Wi-Fi performance optimization:  
QoS, EDCA/WMM, 802.11e, advanced RRM, Client Load Balancing, Band Steering, Mobility Anchoring, High-Density Tuning, MCS optimization, and enterprise-grade RF tuning.

Deep Cisco focus: Catalyst 9800, AireOS reference, LWAP/AP modes.

---

# **9. Advanced Wireless (CCNP ENCOR + Enterprise Engineer Level)**

Modern enterprise Wi-Fi engineering requires **predictive analysis, airtime budgeting, QoS shaping, MCS optimization, roaming stability, and high-density design**, all of which go beyond basic configuration.

This chapter consolidates everything required to operate a **large-scale Cisco WLAN**.

---

# **9.1 Wireless QoS & WMM (802.11e)**

QoS in Wi-Fi is NOT like Ethernet QoS.

Wi-Fi uses **airtime prioritization**, not simple DSCP tagging.  
Because of CSMA/CA, higher-priority frames get **shorter contention windows**.

---

## **9.1.1 WMM Access Categories (AC)**

|AC|802.11 Name|Typical Use|Priority|
|---|---|---|---|
|AC_VO|Voice|VoIP|Highest|
|AC_VI|Video|Streaming|High|
|AC_BE|Best Effort|Web|Normal|
|AC_BK|Background|File download|Lowest|

---

# **9.1.2 EDCA Parameters (Engineer's View)**

Each AC has:

- **AIFS** (Arbitration Interframe Space)
    
- **CWmin/CWmax** (Contention window)
    
- **TXOP** (Max airtime burst)
    

Lower contention → higher priority.

---

## **Cisco WLC Example – Modify EDCA Values**

```less
# MODIFY VOICE EDCA PARAMETERS (CATALYST 9800)
conf t
wireless profile qos VOICE-QoS
 dot11 5ghz
  ac-vo aifsn 2
  ac-vo cwmin 3
  ac-vo cwmax 4
 exit
!
end
```

---

# **9.2 Fast Lane / AVC (Cisco Application Visibility & Control)**

Catalyst 9800 can examine application traffic and enforce:

- Prioritization
    
- Rate limiting
    
- QoS marking
    
- Per-application policies
    

Example:

```less
show avc statistics
```

# **9.3 Advanced Cisco RRM (Radio Resource Management)**

RRM is the **autopilot** for enterprise WLAN optimization.

Components:

1. **DCA (Dynamic Channel Assignment)**
    
2. **TPC (Transmit Power Control)**
    
3. **CHD (Coverage Hole Detection)**
    
4. **Client Load Balancing**
    
5. **Band Steering**
    
6. **RRM Neighbor Discovery**
    
7. **RF Groups**
    

---

## **9.3.1 Dynamic Channel Assignment (DCA)**

DCA automatically selects best channel based on:

- CCI
    
- ACI
    
- Interference
    
- Noise floor
    
- Radar (DFS events)
    
- AP density

```less
show wireless rrm dca summary
```

## **9.3.2 Transmit Power Control (TPC)**

TCP performs:

- Power increases to cover holes
    
- Power reduction to avoid CCI
    
- Maintains client/AP Tx symmetry

```less
show wireless rrm tsp summary
```

## **9.3.3 Coverage Hole Detection (CHD)**

Triggered when:

- Clients report RSSI < configured threshold
    
- High retry rates
    

Catalyst 9800 automatically adapts TPC or raises alarms.

---

# **9.4 Load Balancing & Band Steering (Client Optimization)**

## **9.4.1 Band Steering**

Purpose:  
Encourage dual-band clients to use **5 GHz** → increases capacity.

Mechanism:

- AP delays or suppresses probe responses on 2.4 GHz
    
- Encourages clients to join 5 GHz
    

Catalyst 9800:

```less
wireless profile policy CORP-POLICY
 band-steering
```

## **9.4.2 Client Load Balancing**

Trigger when:

- AP1 client count > AP2 client count
    
- AP1 RSSI not too strong
    
- AP2 within range
    

WLC rejects association attempts on overloaded APs.

```less
wireless profile policy CORP-POLICY
 load-balancing
```

# **9.5 Mobility & Roaming – Advanced Concepts**

## **9.5.1 Mobility Anchoring (Foreign/Anchor WLC)**

Used for:

- Guest networks
    
- L3 roaming
    
- Inter-WLC roaming
    
- Multisite deployments
    

**Roles:**

- **Anchor WLC** → client IP stays here
    
- **Foreign WLC** → client physically connected here
    

Data path:

```less
CLIENT → AP → FOREIGN WLC → Mobility Tunnel → ANCHOR WLC → Network
```

## **9.5.2 Mobility Groups**

Must match between WLCs:

- Mobility group name
    
- MAC addresses of WLCs
    
- Same mobility protocol version

```less
show wireless mobility summary
```

## **9.5.3 Fast BSS Transition (802.11r)**

Optimizes voice roaming.  
Pre-derives PMK-R1 keys for neighbor APs.

Modes:

- FT-over-the-air
    
- FT-over-the-DS
    

Catalyst 9800 config:

```less
wlan CORP-VOICE
 security wpa akm ft over-the-air
```

# **9.6 High-Density Wi-Fi Design (Large Venues, Auditoriums, Stadiums)**

Cisco guidelines:

### Do:

- Use **directional antennas**
    
- Use **20 MHz** channels
    
- Increase AP count
    
- Lower AP Tx power
    
- Use band steering
    
- Disable legacy data rates
    
- Enable fast roaming
    
- Use voice-grade roaming design (if VoIP)
    

### Do NOT:

- Use 40/80 MHz channels in 5GHz in HD environments
    
- Use > 3 SSIDs
    
- Use high power levels
    
- Place APs too close (cell overlap too strong)
    

---

# **Complex Multi-Layer Diagram — High-Density Stadium WLAN**

```less
+======================================================================================================+
|                                   HIGH-DENSITY STADIUM WLAN DESIGN                                   |
+======================================================================================================+

                         Directional Antenna Grid (Sectorized)
                 +-----------+   +-----------+   +-----------+
                 | AP1 C1    |   | AP2 C6    |   | AP3 C11   |
                 | Tx 6 dBm  |   | Tx 6 dBm  |   | Tx 6 dBm  |
                 +-----------+   +-----------+   +-----------+
                      ^                ^                 ^
                      |                |                 |
                  Sector A         Sector B          Sector C
                (Audience)        (Audience)         (Audience)

AP-to-AP Channel Assignment:
- 20 MHz channels
- Non-overlapping pattern
- DFS channels used to increase availability

RRM Adjustments:
- TPC restrict range
- DCA excludes channels with interference
- Load balancing forces even client distribution
```

# **9.7 MCS (Modulation & Coding Scheme) Optimization**

MCS determines throughput.  
Depends on:

- RSSI
    
- SNR
    
- Channel width
    
- Spatial streams
    
- Guard interval
    

### Typical MCS targets:

- Minimum MCS 5 for office    
- MCS 7–9 desired for high performance    

Catalyst 9800 allows minimum mandatory rate tuning.

Example:

```less
wireless profile rf 5GHZ-CORP
  data-rate 6mbps disable
  data-rate 12mbps mandatory
```

# **9.9 Advanced Spectrum Analysis (CleanAir)**

If APs support CleanAir:

- Detects non-Wi-Fi interference    
- Identifies interferer types    
- Suggests mitigation    

View:

```less
wireless profile rf HIGH-DENSITY
 band 5ghz
 max-client-count 100
 client-aware
 coverage-hole-detection
 channel width 20mhz
 data-rate 24mbps mandatory
```

# **9.11 Advanced AP Modes (Monitor, Sniffer, SE-Connect)**

## Monitor Mode

- Scans all channels    
- No client service    

## Sniffer Mode

- For capturing 802.11 frames    
- Sends packets to Wireshark    

## SE-Connect

- Spectrum analysis mode    
- Used for RF troubleshooting    

---

# **9.12 Summary Table — Advanced Wireless**

|Feature|CCNA|CCNP|Engineer|
|---|---|---|---|
|QoS/WMM|Required|Deep|Must tune|
|Voice optimization|Mentioned|Deep|Critical|
|RRM|Required|Deep|AP density tuning|
|Mobility/Anchoring|Mentioned|Deep|Enterprise multisite|
|Band Steering|Basic|Deep|High-density|
|Load Balancing|Basic|Deep|Real deployments|
|High-density tuning|No|Yes|Stadium design|
|MCS tuning|No|Yes|Performance tuning|
|Spectrum analysis|No|Yes|RF troubleshooting|

# **CHAPTER 10 — WIRELESS IN SD-ACCESS (Cisco Fabric Wireless)**

This chapter covers how WLAN integrates into a **Software-Defined Access (SD-Access)** fabric using Cisco DNA Center + Catalyst 9800 controllers.  
It explains the SD-Access architecture (control plane, data plane, fabric borders), LISP mobility, SGT integration, and Wireless Overlays.

This is advanced, CCNP ENCOR → Enterprise Architect depth.

---

# **10. Wireless in SD-Access (Fabric Wireless)**

Traditional WLAN = APs → CAPWAP → WLC → VLANs → Access/Core.

SD-Access removes VLAN stretching, centralizes policy, and uses a **virtualized overlay fabric** with:

- LISP control plane
    
- VXLAN data plane
    
- SGT-based security
    
- DNA Center automation
    
- Catalyst 9k switches as Fabric Edge/Control/Borders
    
- Catalyst 9800 as Fabric WLC
    

This chapter explains how wireless fits into the fabric.

---

# **10.1 SD-Access Fundamentals (Quick Recap)**

SD-Access uses:

### **Underlay Network**

Physical IP transport:

- Routed access (no VLANs extended)
    
- ISIS/OSPF/EIGRP typically used
    
- Every switch has full IP connectivity
    

### **Overlay Network**

Created with **VXLAN tunnels**, carrying:

- User traffic
    
- Scalable Group Tags (SGT)
    
- Virtual networks (VNs)
    

### **Control Plane**

LISP database tracks:

- Client identities
    
- Locations
    
- Mobility events
    

### **Policy Plane**

ISE provides:

- Identity
    
- SGT assignment
    
- Policy matrix (SGACLs)
    

---

# **10.2 Wireless Integration Architecture**

In SD-Access Wireless, the WLC does **not** tunnel client traffic.

Instead:

**AP → WLC (CAPWAP) for control only**  
**Client data → encapsulated in VXLAN → directly into fabric**

This offers:

- End-to-end segmentation
    
- Faster mobility
    
- SGT propagation
    
- Simplified wireless design
    

---

# **10.3 SD-Access Wireless Roles**

|Component|Role|
|---|---|
|**Catalyst 9800**|Fabric Wireless Controller|
|**Fabric Edge Nodes**|Switches where APs connect|
|**Fabric Control Node**|LISP mapping server|
|**Border Node**|Connects fabric to external networks|
|**DNA Center**|Orchestration of the entire fabric|

---

# **10.4 Complex Multi-Layer Diagram — SD-Access Wireless Architecture**

```less
+=====================================================================================================+
|                                   SD-ACCESS FABRIC WIRELESS ARCHITECTURE                            |
+=====================================================================================================+

                UNDERLAY (IP ROUTED)
+-------------+       +-------------+       +-------------+
| Fabric Edge |-------| Fabric Edge |-------| Fabric Edge |
|   (9300)    |       |   (9300)    |       |   (9300)    |
+------+------+       +------+------+       +------+------+
       |                     |                     |
       | VXLAN Overlay       | VXLAN Overlay       |
       |                     |                     |
       v                     v                     v
+-------------+       +-------------+       +-------------+
|  Clients    |       |  Clients    |       |  Access     |
|  Wireless   |       |  Wired      |       |  Points     |
+------+------+       +-------------+       +------+------+
       |                                             |
       | CAPWAP Control                              |
       v                                             v
+------------------------+              +---------------------------+
|  Catalyst 9800 WLC     |              | DNA Center Control Plane  |
|  (Fabric Wireless Ctrl)|              | LISP + Policy Orchestration|
+------------------------+              +---------------------------+
```

# **10.5 How Wireless Traffic Flows in SD-Access**

## **Step-by-step:**

### **1. Client connects to AP**

Same:

- Probe → Auth → Assoc
    
- WPA2/WPA3 handshake
    

### **2. WLC authenticates client**

(Fabric WLC does NOT switch traffic)

### **3. WLC assigns SGT and VN (virtual network)**

Based on:

- SSID
    
- ISE identity
    
- Policy profile
    

### **4. AP sends traffic to Fabric Edge**

AP → Fabric Edge switch

### **5. Fabric Edge encapsulates traffic into VXLAN (based on VNID)**

VXLAN header includes:

- VNID (Virtual Network ID)
    
- SGT
    
- Campus fabric tags
    

### **6. Traffic flows through fabric overlay**

Edge → Intermediate Nodes → Border/Control Nodes

### **7. Traffic routed based on fabric control plane (LISP)**

---

# **10.6 LISP Control Plane Role in Wireless Mobility**

LISP tracks:

- Client MAC
    
- Client IP
    
- Location (edge switch)
    

When client roams, edge switches update entries:

```less
LISP Mapping:
Key: MAC/IP
Value: Edge RLOC (Routing Locator)
```

### Benefits:

- Instant L3 roaming
    
- No tunneling back to anchor
    
- No VLAN stretching
    

This is much better than old Anchor/Foreign model in AireOS.

---

# **10.7 Wireless Layer 3 Roaming in SD-Access**

In classic networks:

- L3 roam → anchor/foreign tunnels
    
- Complex
    
- High latency
    

In SD-Access:

- Roaming involves simple LISP map updates
    
- VXLAN tunnels updated to new Edge
    
- Client keeps same IP (always!)
    

---

# **10.8 Complex Diagram — Wireless L3 Roaming in SD-Access**

```less
INITIAL STATE:
Client connected to Edge-1
LISP DB: (Client IP/MAC) → Edge-1-RLOC

ROAM EVENT:
Client moves to Edge-2

EDGE-2:   Sends LISP Map-Register → Control Node  
CONTROL:  Updates mapping (Client → Edge-2-RLOC)  
EDGE-1:   Forgets mapping  

TRAFFIC FLOW:
All traffic dynamically redirected to Edge-2 via VXLAN
```

Zero delay L3 roaming. Ideal for voice.

---

# **10.9 Policy Enforcement with SGT (TrustSec)**

SGT provides identity-based segmentation.

### Example:

- Employee → SGT=10
    
- Guest → SGT=20
    
- Contractor → SGT=30
    

Policies stored in ISE:

```less
SGT 10 → Allow → ERP servers
SGT 20 → Deny → Internal LAN
SGT 30 → Allow → Internet only
```

Wireless SGT workflow:

1. ISE assigns SGT during 802.1X auth
    
2. WLC receives SGT
    
3. AP sends client traffic to Edge
    
4. Edge encapsulates SGT in VXLAN header
    
5. Fabric enforces policies at intermediate or border nodes
    

---

# **10.10 Wireless Overlays and Virtual Networks (VNs)**

SD-Access replaces VLANs with **VNs**.

|SSID|VN ID|Use Case|
|---|---|---|
|Corporate|VN 100|Internal apps|
|Voice|VN 200|VoIP, collaboration|
|Guest|VN 300|External Internet only|
|IoT|VN 400|Restricted devices|

Multiple SSIDs → separate VNs → complete segmentation without VLAN complexity.

---

# **10.11 How APs behave differently in Fabric Wireless**

In classic deployments:

- AP tunnels data → WLC → Switch
    

In SD-Access Wireless:

- AP tunnels only control → WLC
    
- Data goes into fabric at edge
    

### Result:

- WLC is no longer a bottleneck
    
- Roaming faster
    
- More scalable
    
- Cleaner policy enforcement
    

---

# **10.12 Catalyst 9800 Configuration for SD-Access Wireless**

DNA Center automates almost all configuration.

However, CLI reference for engineers:

```less
# ENABLE FABRIC WIRELESS MODE
conf t
wireless fabric enable

# MAP SSID TO FABRIC VN
wireless tag policy FABRIC-POLICY
  wlan CORP-WLAN policy CORP-POLICY
  fabric enabled
  vnid 100
!
end

# ENABLE SGT TAGGING
wireless profile policy CORP-POLICY
  trustsec sgt <value-from-ISE>
!
end
```

APs receive Fabric settings automatically via AP Join Profile.

---

# **10.13 SD-Access Guest Wireless**

Guest traffic is often "internet-only" and must NEVER enter internal fabric.

SD-Access provides:

- Fabric-enabled guest VN
    
- Guest traffic egress via **Border Node → Firewall → Internet**
    
- Captive Portal via ISE Guest
    

---

# **10.14 Benefits of SD-Access Wireless**

| Benefit                       | Explanation                         |
| ----------------------------- | ----------------------------------- |
| **True L3 roaming**           | No anchor/foreign tunnels           |
| **Identity-based networking** | SGT follows user everywhere         |
| **Scalability**               | VXLAN fabric is extremely efficient |
| **Automation**                | DNA Center configures everything    |
| **Security**                  | End-to-end segmentation             |
| **Performance**               | No bottleneck at WLC                |

# **CHAPTER 11 — WIRELESS THREATS (OFFENSIVE & DEFENSIVE ENGINEERING)**

This chapter covers **how attackers compromise Wi-Fi**, how Cisco WLAN infrastructure detects and prevents these attacks, and how enterprise engineers harden WLANs against real threats.  
Professional engineer + offensive security level.

---

# **11. Wireless Threats – Offensive & Defensive**

802.11 networks are inherently exposed because the medium is shared.  
Unlike wired networks, **anyone within RF range can attack the WLAN**, even without physical access.

Wireless threats fall into three major categories:

1. **Client-side attacks**
    
2. **Infrastructure attacks** (AP/WLC)
    
3. **RF-layer attacks**
    

This chapter covers all of them, from attacker techniques to Cisco detection and mitigation.

---

# **11.1 Threat Model Overview**

|Threat Type|Attacker Goal|Example Attack|
|---|---|---|
|**Confidentiality**|Capture data|WPA2 cracking, Evil Twin|
|**Integrity**|Modify data|MITM, rogue AP|
|**Availability**|Disrupt service|Deauth flood, RF jamming|
|**Authentication**|Steal credentials|EAP harvesting|
|**Authorization**|Gain unauthorized access|VLAN hopping, bypass 802.1X|

---

# **11.2 Offensive Attacks Against WPA2/WPA3**

---

## **11.2.1 PSK Cracking (WPA2-Personal)**

Attack flow:

1. Capture 4-way handshake
    
2. Perform offline dictionary attack
    
3. Weak passwords fall instantly
    

Tools:

- `aircrack-ng`
    
- `hashcat`
    

Mitigation:

- WPA3-SAE
    
- Strong PSKs
    
- PMF enabled
    

---

## **11.2.2 Evil Twin Attack (Rogue AP)**

Attacker sets up AP with:

- Same SSID
    
- Higher power
    
- No encryption or fake portal
    

Victims join attacker’s AP instead of real AP.

**Goal:** Steal credentials or perform MITM.

Mitigation:

- 802.1X + certificates
    
- PMF (prevents fake disassociation)
    
- Cisco Rogue AP Detection
    
- WIPS containment
    

---

## **11.2.3 Captive Portal Phishing (Evil Portal)**

Attacker emulates:

- Corporate login page
    
- Public Wi-Fi login page
    
- SSO or VPN login
    

Mitigation:

- 802.1X everywhere
    
- DNSSEC / validation
    
- Certificate validation
    
- Cisco Umbrella
    

---

# **11.3 Attacks Against WPA3**

Even WPA3 is not invulnerable.

### Known attacks:

- Downgrade attacks to WPA2
    
- Timing attacks against SAE if implementation is weak
    
- Dragonblood vulnerabilities (fixed in modern firmware)
    

Mitigation:

- Disable mixed-mode when possible
    
- Patch APs/clients
    
- Enforce PMF
    

---

# **11.4 EAP/802.1X Attacks**

### 1. **EAP Harvesting**

Attacker impersonates RADIUS server.

Goal: Capture MSCHAPv2 challenge/response → crack offline.

Mitigation:

- Only use **EAP-TLS** (certificate-based)
    
- Disable PEAP/MSCHAPv2 where possible
    
- Enforce server certificate validation
    

---

# **11.5 RF-Layer Attacks**

## **11.5.1 Deauthentication / Disassociation Floods**

Exploits unprotected mgmt frames.

Mitigation:

- PMF (Protected Management Frames)
    
- WPA3 requires PMF
    

---

## **11.5.2 Beacon Flooding**

Attacker:

- Spoofs fake APs
    
- Overwhelms clients
    
- Disrupts scanning
    

Defense:

- WLC alarms
    
- WIPS
    
- Spectrum analysis
    

---

## **11.5.3 RF Jamming**

High-power transmitters or malfunctioning devices emit noise.

Mitigation:

- CleanAir (Cisco APs)
    
- Spectrum analyzer
    
- Physically locate interferer
    

---

# **11.6 Rogue APs & Rogue Clients**

### Rogue AP:

Unauthorized AP plugged into network.

Cisco categorizes rogues as:

- Malicious
    
- Friendly
    
- External
    

Detection sources:

- AP monitor mode
    
- AP local mode (background scanning)
    
- Rogue Detector APs
    

Containment:

- Send disassoc frames
    
- Stop clients from joining
    

---

## **11.6.1 Complex Diagram – Rogue Detection & Containment**

```less
+-------------------------------------------------------------------------------------------+
|                            ROGUE AP DETECTION WORKFLOW                                    |
+-------------------------------------------------------------------------------------------+
Client STA ----joins----> ROGUE AP -----> Tries to reach internal network
                                    ^
                                    |
                     AP in Monitor Mode / Local Mode scanning
                                    |
                                    v
                          Cataly st 9800 WLC
                                    |
                    Rogue Classification (Friendly? Malicious?)
                                    |
                       If malicious → SEND DEAUTH / DISASSOC
                                    |
                                    v
                             MITIGATION ACTIVATED
```

# **11.7 Ad-Hoc / Peer-to-Peer Networks**

Attackers use ad-hoc networks to bypass enterprise security.

Mitigation:

- WLC disables ad-hoc client bridging
    
- WIPS detection
    

---

# **11.8 DHCP Spoofing / ARP Poisoning on Guest WLAN**

Guests may attempt to:

- Become DHCP server
    
- Perform MITM
    
- Redirect clients through them
    

Mitigation:

- Port Isolation / PVLAN
    
- DHCP Snooping
    
- ARP inspection
    
- Firewall segmentation
    
- No Layer 2 interactions between guests
    

---

# **11.9 VLAN Hopping (Wireless Side)**

Occurs when:

- AP configured incorrectly
    
- DTP left enabled
    
- Native VLAN misconfigured
    

Mitigation:

- Hard-code trunk settings
    
- Never use VLAN 1
    
- Disable DTP
    

---

# **11.10 Wireless Pivoting (Post Exploitation)**

Once attacker compromises Wi-Fi:

- Moves laterally into internal network
    
- Uses traffic tunneling
    
- Launches internal recon
    
- Targets AD, databases, file servers
    

Mitigation:

- SGT segmentation
    
- ACLs based on identity
    
- Network Access Control (ISE)
    
- Zero Trust architecture
    

---

# **11.11 Cisco Wireless Defensive Technologies**

|Feature|Purpose|Platform|
|---|---|---|
|**PMF**|Protect mgmt frames|WPA3, WPA2 optional|
|**WIPS/WIDS**|Attack detection|Catalyst 9800, AireOS|
|**Rogue AP Detection**|Identify unauthorized APs|WLC/AP scanning|
|**CleanAir**|Detect non-Wi-Fi interference|Supported APs|
|**TrustSec (SGT)**|Identity segmentation|Catalyst 9K|
|**ISE**|Policy & identity|Entire network|
|**Umbrella**|DNS-layer defense|Cloud|

---

# **11.12 Complex Multi-Layer Diagram — Wireless Threat Defense Stack**

```less
+==================================================================================================+
|                                   CISCO WIRELESS SECURITY STACK                                 |
+==================================================================================================+

LAYER 1 – RF DEFENSE
 - CleanAir
 - Spectrum Analysis
 - Interference Detection

LAYER 2 – WLAN FRAME PROTECTION
 - PMF (protect deauth/disassoc)
 - WPA3 SAE / WPA2-Enterprise

LAYER 3 – IDENTITY & ACCESS
 - 802.1X / EAP-TLS
 - Dynamic VLAN
 - SGT (TrustSec)

LAYER 4 – CONTROLLER PROTECTIONS
 - Rogue AP Detection
 - WIPS/WIDS
 - AP Authentication (DTLS)

LAYER 5 – POLICY & SEGMENTATION
 - ISE SGACLs
 - VN segmentation (SD-Access)
 - Guest isolation

LAYER 6 – APPLICATION & DNS DEFENSE
 - Cisco Umbrella
 - Application Visibility & Control

Combined effect: END-TO-END ZERO TRUST WIRELESS
```

# **11.13 Summary Table – Wireless Threats**

|Threat|Attack Method|Cisco Defense|
|---|---|---|
|PSK cracking|Capture handshake|WPA3-SAE|
|Evil twin|Rogue AP|WIPS + 802.1X + PMF|
|Deauth flood|Spoof mgmt frames|PMF|
|Jamming|RF noise|CleanAir|
|EAP harvesting|Fake RADIUS|EAP-TLS|
|Guest pivoting|Layer 2 access|PVLAN + ACLs|
|VLAN hopping|Misconfig|Trunk hardening|

# **CHAPTER 12 — UNIFIED DIAGRAM COLLECTION (MASTER DIAGRAMS)**

This chapter consolidates _all_ essential wireless engineering diagrams into one place.  
These diagrams provide a **visual reference** for CCNA → CCNP → Enterprise Engineer levels, covering WLAN architecture, RF propagation, security handshakes, roaming flows, SD-Access integration, and troubleshooting.

All diagrams use **complex multi-layer ASCII format**, following your engineer-document style.

---

# **12. Unified Diagram Collection (Master Diagrams)**

---

# **12.1 Cisco Split-MAC Architecture**


```less
+----------------------------------------------------------------------------------------------+
|                                   SPLIT-MAC ARCHITECTURE                                     |
+----------------------------------------------------------------------------------------------+

           802.11 MAC (Mgmt, Control, Data Frames)
           +---------------------------+
           |  CLIENT STA               |
           +---------------------------+
                       |
                       | 802.11 Frames
                       v
                +------------------+
                | CISCO AP (LWAP) |
                |------------------|
                | Local MAC:       |
                |  - 802.11 enc/dec|
                |  - ACK, RTS/CTS   |
                |  - QoS/WMM        |
                |  - RF metrics     |
                +---------+--------+
                          |
                          | CAPWAP Control (DTLS 5246)
                          | CAPWAP Data (optional 5247)
                          v
           +-------------------------------------------+
           |            CATALYST 9800 WLC              |
           |-------------------------------------------|
           | - Authentication (802.1X)                 |
           | - Association State Machine               |
           | - PMK/PTK/GTK management                  |
           | - Policy/VLAN assignment                  |
           | - RRM (TPC, DCA, CHD)                     |
           +-------------------------------------------+
```

## 12.2 CAPWAP Discovery & Join Flow

```less
AP BOOTS
  |
  +-- DHCP (native VLAN) ----> Get IP, Option 43?
  |
  +-- DNS: cisco-capwap-controller.<domain>?
  |
  +-- L2 broadcast discovery?
  |
  +-- JOIN REQUEST ----> WLC
  |
  +-- DTLS Handshake (Secure Control Channel)
  |
  +-- DOWNLOAD CONFIG (WLANs, Tags, RF, Flex)
  |
  +-- RUN STATE (AP active, clients can connect)
```

##  12.3 WPA2/WPA3 4-Way Handshake

```less
CLIENT STA                                        AP/WLC
-----------------------------------------------------------------------------------------

1. <-------------------- ANonce ----------------------

2. SNonce + MIC(PMK) ------------------------------->

3. <-------------- GTK + Install PTK ----------------

4. Confirm MIC ------------------------------------->

Traffic now encrypted with TK (Temporal Key)
```

## 12.4 Full WLAN Authentication Pipeline

```less
SCAN
 CLIENT ----------- Probe Req ----------> AP
 CLIENT <---------- Probe Resp ---------- AP

AUTHENTICATION (Open/SAE/802.1X)
 CLIENT -------- Auth Req -------------> AP
 CLIENT <------- Auth Resp ------------- AP

ASSOCIATION
 CLIENT -------- Assoc Req ------------> AP
 CLIENT <------- Assoc Resp ------------ AP

SECURITY (WPA2/WPA3)
 4-way handshake or SAE

DATA FLOW
 CLIENT -> AP -> CAPWAP -> WLC -> Switch -> Router -> Internet/LAN
```

## 12.5 RF Propagation Model in Enterprise Buildings

```less
+---------------------------------------------------------------------------------------------+
|                                      RF PROPAGATION MAP                                     |
+---------------------------------------------------------------------------------------------+

 AP1 (CH36) ---------- Drywall (-3 dB) -------- AP2 (CH44)
 RSSI: -62 dBm                                    RSSI: -65 dBm

 Reflection: Glass Wall (-4 dB)
 Multipath: Metal surfaces
 Human Absorption: ~3 dB per person (20 people = 60 dB)

 Overlap Zones:
 - Evaluate CCI (Co-channel interference)
 - Evaluate ACI (Adjacent-channel interference)
```

## 12.6 802.11 Frame Structure

```less
+-----------------------------------------------------------------------------------------------------+
|                                       802.11 FRAME FORMAT                                           |
+-----------------------------------------------------------------------------------------------------+
| Frame Control | Duration | Addr1 | Addr2 | Addr3 | Seq Ctrl | [Addr4] | QoS Ctrl | HT Ctrl | FCS    |
+-----------------------------------------------------------------------------------------------------+
```

## 12.7 SD-Access Wireless Fabric Architecture

```less
+======================================================================================================+
|                                  SD-ACCESS WIRELESS ARCHITECTURE                                     |
+======================================================================================================+

        UNDERLAY (ROUTED IP)
+---------+    +---------+    +---------+
| Edge 1  |----| Edge 2  |----| Edge 3  |
| 9300    |    | 9300    |    | 9300    |
+----+----+    +----+----+    +----+----+
     |              |              |
     | VXLAN Overlay|              |
     v              v              v

 Wireless Clients ----> Access Points ----> Fabric Edge
                               |
                               | CAPWAP Control only
                               v
                          Catalyst 9800 WLC
                               |
                               v
                         DNA Center (LISP/MS/Policy)
```


## 12.8 SD-Access Wireless L3 Roaming

```less
CLIENT moves from Edge-1 → Edge-2

Edge-2: Send LISP Map-Register (Client MAC/IP → Edge-2-RLOC)
Control Node: Updates mapping
Edge-1: Deletes mapping

Traffic routed dynamically to Edge-2
NO anchor/foreign tunnels needed
```

##  12.9 High-Density WLAN Architecture

```less
+==============================================================================================+
|                        HIGH-DENSITY STADIUM WLAN – SECTOR DESIGN                             |
+==============================================================================================+

               AP1 (Sector A)       AP2 (Sector B)        AP3 (Sector C)
                CH36 6 dBm           CH44 6 dBm            CH149 6 dBm
               [Directional]        [Directional]         [Directional]

      +-------------+        +---------------+       +-------------+
      | Audience    |        | Audience      |       | Audience    |
      | Dense Area  |        | Dense Area    |       | Dense Area  |
      +-------------+        +---------------+       +-------------+

Use:
- 20 MHz channels
- Directional antennas
- Lower transmit power
- Band steering
- Load balancing
```

## 12.10 Advanced RRM Architecture

```less
+----------------------------------------------------------------------------------------------+
|                                        RRM OVERVIEW                                          |
+----------------------------------------------------------------------------------------------+

          AP1                 AP2                 AP3                AP4
   (measurements)      (measurements)      (measurements)     (measurements)
         |                    |                    |                  |
         +--------- RRM Neighbor Discovery (RF Group) ---------------+
                               |
                               v
                   CATALYST 9800 RRM DECISION ENGINE
                               |
          ---------------------------------------------------
          |                 |                |              |
         DCA               TPC              CHD           Load Balancing
   (Channels)        (Tx power)        (Coverage)       (Client distribution)
```

## 12.11 Wireless Troubleshooting Decision Tree

```less
+=========================================================================================+
|                                 WIRELESS TROUBLESHOOTING                                 |
+=========================================================================================+

RF Layer:
  - RSSI < -67? Fix coverage.
  - SNR < 20? Remove interference.
  - CCI/ACI? Fix channels/power.

MAC Layer:
  - Probe? Auth? Assoc?
  - Unsupported rates? PMF mismatch?

Security Layer:
  - WPA2/WPA3 handshake?
  - 802.1X EAP success?
  - Cert issues?

Network Layer:
  - DHCP? DNS? VLAN?
  - ACL blocking?
  - Routing to gateway?

Control/Data Plane:
  - CAPWAP up?
  - DTLS errors?
  - Policy/Tag mismatch?
```

## 12.12 Rogue AP Detection Flow

```less
APs scan RF → Send rogue reports → WLC classifies rogue → If malicious → Containment
```

```lua
Client <------ Rogue AP <---- Attacker
    ^             |
    |             v
Monitor AP -----> WLC -----> Containment
```

## 12.13 End-to-End WLAN Data Flow Diagram

```less
CLIENT STA
   |
   | 802.11 Frames
   v
CISCO AP
   |
   | CAPWAP (control/data)
   v
CATALYST 9800 WLC
   |
   | 802.3 (Ethernet)
   v
SWITCH → ROUTER → SERVER/INTERNET

```

## 12.14 CAPWAP Control/Data Path Separation

```less
CONTROL PLANE:
  UDP/5246 (DTLS encrypted)
  - Auth, Assoc
  - RRM
  - Policies

DATA PLANE:
  UDP/5247 (optional encrypted)
  - Client data frames
```

## 12.15 E2E WLAN Architecture Summary Diagram

```less
AP <--> WLC <--> Switch <--> Router <--> Firewall <--> Internet
      CAPWAP        VLANs       Routing     Policies
```



## WIRELESS MINI WIKI (ENGINEER EDITION)

```less
===================================================================================
WIRELESS MINI WIKI – CISCO ENGINEER EDITION
===================================================================================

GENERAL CONCEPTS
----------------
Wi-Fi Bands:
 - 2.4 GHz: long range, 1/6/11 only, heavy interference.
 - 5 GHz: many channels, DFS, best for enterprise.
 - 6 GHz (Wi-Fi 6E): clean spectrum, short range.

RSSI & SNR:
 - RSSI target: -67 dBm (enterprise), -65 (voice).
 - SNR target: >20 dB (data), >25–30 dB (voice).
 - Noise floor: -90 to -100 dBm typical.

802.11 Standards:
 - a/g: 54 Mbps OFDM.
 - n: MIMO (2x2, 3x3), up to 600 Mbps.
 - ac: 5 GHz, MU-MIMO, >1 Gbps.
 - ax (Wi-Fi 6): OFDMA, BSS coloring.
 - e: WMM/EDCA.
 - r: Fast BSS transition.
 - k: Neighbor reports.
 - v: Client steering.
 - w: PMF (Mgmt Frame Protection).

ANTENNAS
--------
Types: Omni, Patch, Yagi, Panel, Sector.
Gain in dBi = directional focusing.
EIRP = Tx Power + Antenna Gain – Cable Loss.

RF PROBLEMS
-----------
 - CCI = same channel, shared airtime.
 - ACI = overlapping channels (2.4 GHz bad).
 - Multipath: used by MIMO, harmful in old PHYs.
 - Attenuation: drywall ~3 dB, concrete ~10–15 dB.

DESIGN RULES
------------
 - Use 5 GHz as primary band.
 - Use 20 MHz channels in enterprise deployments.
 - Keep AP Tx power low (8–14 dBm).
 - Limit SSIDs to 3–4 max.
 - Mount APs at 2.4–3 m height (ceiling).
 - Disable legacy data rates (<12 Mbps).
 - Plan for capacity, not just coverage.

RRM (RADIO RESOURCE MANAGEMENT)
-------------------------------
 - DCA: channel assignment.
 - TPC: power control.
 - CHD: coverage hole detection.
 - RF Groups: AP neighborship.
 - Band Steering: push clients to 5 GHz.
 - Load Balancing: reject assoc to overloaded APs.

AUTHENTICATION PROCESS
-----------------------
JOIN PROCESS:
 1. Probe request/response
 2. Authentication (Open / SAE / 802.1X)
 3. Association
 4. 4-way handshake (WPA2) OR Dragonfly (WPA3)
 5. DHCP
 6. Data

WPA2-PSK:
 - PMK derived from PSK.
 - Vulnerable to offline cracking.

WPA3-SAE:
 - Dragonfly handshake.
 - Resistant to offline dictionary attacks.
 - PMF mandatory.

802.1X (Enterprise):
 - EAP-TLS recommended.
 - WLC forwards EAP to ISE via RADIUS.
 - Result: MSK → PMK created.

KEY HIERARCHY:
 PMK → PTK → {KCK, KEK, TK}.

ROAMING
-------
L2 roam: Same subnet, fast.
L3 roam (classic): Anchor/Foreign mobility tunnel.
Fast Transition: 802.11r, pre-derives PMK-R1.

SD-ACCESS WIRELESS
------------------
 - AP → CAPWAP (control only)
 - Data → VXLAN in fabric edge.
 - LISP handles mobility (instant L3 roam).
 - SGT (TrustSec) provides identity-based segmentation.
 - VN (Virtual Network) replaces VLAN.

AP MODES
--------
 - Local: tunnel all data to WLC.
 - FlexConnect: local switching at remote sites.
 - Monitor: dedicated scanning.
 - Sniffer: packet capture.
 - Bridge/Mesh: outdoor backhaul.
 - SE-Connect: spectrum analysis.

CAPWAP
------
 - Control tunnel: UDP 5246 (DTLS encrypted).
 - Data tunnel: UDP 5247.
Discovery:
 - DHCP Option 43
 - DNS: cisco-capwap-controller
 - L2 broadcast

SWITCH CONFIG FOR AP
--------------------
interface Gi1/0/X
 switchport mode trunk
 switchport trunk native vlan <mgmt>
 switchport trunk allowed vlan <list>
 spanning-tree portfast trunk
 power inline auto

WLAN CONFIG COMPONENTS (CATALYST 9800)
---------------------------------------
 - WLAN Profile → defines SSID, security.
 - Policy Profile → VLAN, ACLs, QoS, PMF.
 - RF Profile → channels, power, RRM tuning.
 - Site Tag → AP join settings.
 - Policy Tag → binds WLAN + Policy + RF.
 - AP Tagging → determines AP behavior.

TROUBLESHOOTING FLOW
--------------------
L1 RF:
 - RSSI/SNR/CCI/ACI checks.
 - Check channel/power, interference.

L2 MAC:
 - Probe/Auth/Assoc exchange.
 - PMF/rate mismatch.

L3+ SECURITY:
 - 4-way handshake.
 - EAP failures, cert issues.

NETWORK:
 - DHCP, VLANs, ACLs, trunk issues.

CONTROL PLANE:
 - CAPWAP DTLS.
 - AP join failures (Option 43, DNS, certs).

DATA PLANE:
 - FlexConnect VLAN mapping.
 - ACLs dropping traffic.

KEY DEBUG COMMANDS
-------------------
WLC:
 debug wireless mac <MAC>
 debug capwap events enable
 debug dtls events enable
 debug dot1x all enable

AP:
 debug capwap client no-reload
 debug dot11 state
 show capwap ip config

COMMON FAILURES
---------------
 - Wrong PSK → handshake fails.
 - VLAN mismatch → DHCP fail.
 - PMF mismatch → assoc denied.
 - Cert invalid → 802.1X fail.
 - AP wrong domain → join fail.
 - DTLS expired cert → join fail.
 - High CCI → low throughput.
 - SSID overload → airtime waste.

HIGH-DENSITY RULES
------------------
 - 20 MHz channels only.
 - Lower AP Tx power.
 - Directional antennas.
 - Band steering enabled.
 - Load balancing enabled.
 - Remove 2.4 GHz if possible.

FORMULAS
--------
FSPL(dB) = 20log10(d) + 20log10(f) + 32.44
SNR = RSSI – Noise Floor
EIRP = Tx Power + Ant Gain – Cable Loss

===================================================================================
END OF WIRELESS MINI WIKI
===================================================================================

```
