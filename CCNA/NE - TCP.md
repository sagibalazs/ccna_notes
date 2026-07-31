
# SCRIPT 2 — TRANSMISSION CONTROL PROTOCOL (TCP)

**Complete Network Engineer Guide (Deep Detail)**

---

# 1. HISTORY & EVOLUTION

- **Origin:** Specified in _RFC 793_ (1981), refined by many later RFCs.
    
- **Purpose:** Provide a reliable, ordered, congestion-aware, connection-oriented byte stream over IP.
    
- **Key milestones:**
    
    - **RFC 793:** Original TCP definition.
        
    - **RFC 1122:** Host requirements, important clarifications.
        
    - **RFC 1323:** High-performance extensions → Window Scale, Timestamps, PAWS, SACK.
        
    - **RFC 2018:** Selective Acknowledgment (SACK).
        
    - **RFC 2581, 5681:** Congestion control standards.
        
- **Design philosophy:**
    
    - Reliability and correctness have priority.
        
    - Congestion control protects the Internet.
        
    - Ordering + flow control ensures stream integrity.
        
    - Stateful behavior allows efficient recovery & fairness.
        

---

# 2. HEADER TOPOLOGY (FULL BIT DIAGRAM, EXACT FIELDS)


```less
0                   1                   2                   3
0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-------------------------------+-------------------------------+
|        Source Port (16)       |     Destination Port (16)     |
+-------------------------------+-------------------------------+
|                       Sequence Number (32)                    |
+---------------------------------------------------------------+
|                   Acknowledgment Number (32)                 |
+---------------+---+-----------+-------------------------------+
| Data Offset(4)|Res|  Flags(9) |         Window (16)           |
|               |   |NS CWR ECE URG ACK PSH RST SYN FIN        |
+---------------+---+-----------+-------------------------------+
|       Checksum (16)            |      Urgent Pointer (16)     |
+---------------------------------------------------------------+
|                          Options (0–40 bytes)                 |
+---------------------------------------------------------------+
|                          Application Data …                   |
+---------------------------------------------------------------+
```

### Sizes:

- **Minimum header:** 20 bytes
    
- **Maximum header:** 60 bytes (20 + 40 bytes options)
    
- Options MUST align to 32-bit boundary (padding via NOP).
    

---

# 3. FIELD-BY-FIELD DEEP INTERPRETATION

## 3.1 Source & Destination Ports (16 bits each)

- Identify the local and remote application endpoints.
    
- Combined with IP + protocol → **socket** = unique flow identifier.
    

## 3.2 Sequence Number (32 bits)

- **Main reliability mechanism.**
    
- Indicates the byte position of the first data byte in this segment.
    
- In SYN packets: sequence number = Initial Sequence Number (ISN).
    
- Wraps modulo 2³².
    

## 3.3 Acknowledgment Number (32 bits)

- Shows the **next byte expected** by the receiver.
    
- Cumulative ACK: acknowledges everything up to (ACK-1).
    
- Used to drive retransmission logic.
    

## 3.4 Data Offset (4 bits)

- Specifies header length in 32-bit words.
    
- Example: value 5 → 5×4 = 20 bytes → no options.
    

## 3.5 Reserved (3 bits)

- Historically reserved.
    
- Modern use: leftmost bit often labeled NS (ECN Nonce).
    
- Must be zero unless explicitly used by advanced ECN behavior.
    

## 3.6 Flags (9 bits)

Exact bit order: **NS, CWR, ECE, URG, ACK, PSH, RST, SYN, FIN**

|Flag|Purpose|
|---|---|
|**NS**|ECN nonce (rare today)|
|**CWR**|Congestion Window Reduced (ECN feedback)|
|**ECE**|ECN-Echo (sender congestion indication)|
|**URG**|Urgent pointer valid|
|**ACK**|Acknowledgment field valid|
|**PSH**|Push data immediately to application (advisory only)|
|**RST**|Abort/reset connection|
|**SYN**|Begin connection, synchronize sequence numbers|
|**FIN**|Graceful close request|

## 3.7 Window (16 bits)

- **Receiver-advertised window (rwnd).**
    
- Tells the sender how many bytes it can send before waiting.
    
- Base value is 16 bits → max 65,535 bytes.
    
- With **Window Scale (option)** you can shift this up to 1 GB scale.
    

## 3.8 Checksum (16 bits)

- 1’s complement checksum over:
    
    1. TCP header (checksum field = 0),
        
    2. TCP data,
        
    3. IP pseudo-header.
        
- Detects corruption/misdelivery; not cryptographic.
    

## 3.9 Urgent Pointer (16 bits)

- Points to the last byte of “urgent” data.
    
- Rarely used today; historical TELNET mechanism.
    

## 3.10 Options (0–40 bytes)

Key ones:

### MSS (Maximum Segment Size)

- Sent in SYN only.
    
- Indicates max payload this host is willing to accept.
    
- Usually MTU − 40 for standard TCP/IP.
    

### Window Scale

- Expands window size beyond 65,535 by left-shifting.
    
- Example: Shift = 7 → effective window max ≈ 8 MB.
    
- Negotiated in SYN/SYN-ACK only.
    

### SACK Permitted / SACK Blocks

- **SACK Permitted:** appears in SYN; enables SACK.
    
- **SACK:** carries selective acknowledgment ranges.
    
- Enables recovery of specific lost blocks without damaging throughput.
    

### Timestamps (TSval/TSecr)

- Used for:
    
    - RTT measurement
        
    - PAWS (Protection Against Wrapped Sequence numbers)
        

### NOP (Padding)

- Align options to 32-bit boundaries.
    

---

# 4. TCP PSEUDO-HEADER (CHECKSUM)

## IPv4 Pseudo-header


```less
Source IP (32)
Destination IP (32)
Zero (8)
Protocol = 6 (8)
TCP Length (16)
```

## IPv6 Pseudo-header

```less
Source IP (128)
Destination IP (128)
TCP Length (32)
Zero (24)
Next Header = 6 (8)
```

# 5. TCP CONNECTION WORKFLOW (STATE MACHINE LOGIC)

## 5.1 3-Way Handshake (Connection Establishment)

1. **Client → Server: SYN**
    
    - Client chooses ISN.
        
    - Options sent: MSS, WS, SACK Permitted, TS.
        
2. **Server → Client: SYN-ACK**
    
    - Server acknowledges client ISN+1.
        
    - Server sends its own ISN.
        
3. **Client → Server: ACK**
    
    - ACK = server ISN+1.
        
    - Connection enters **ESTABLISHED**.
        

## 5.2 Data Transfer

### Reliability Mechanisms

- **Sequence numbers** order the byte stream.
    
- **ACK** is cumulative acknowledgment.
    
- **Retransmission** triggered by:
    
    - Timeout (RTO)
        
    - Fast retransmit (3 duplicate ACKs)
        
    - SACK gap reports
        

### Flow Control (Receiver side)

- rwnd limits how much data the sender can have “in flight.”
    

### Congestion Control (Sender side)

TCP implements three major phases:

1. **Slow Start**
    
    - cwnd grows exponentially (cwnd += 1 segment per ACK).
        
2. **Congestion Avoidance**
    
    - Linear growth: cwnd += 1 segment per RTT.
        
3. **Fast Recovery**
    
    - Triggered by 3 duplicate ACKs.
        
    - Retransmit missing segment; reduce cwnd; avoid full slow start.
        

### ECN (Explicit Congestion Notification)

- Router marks IP CE bit instead of dropping packet.
    
- Receiver sends ECE flag.
    
- Sender lowers rate and sets CWR.
    

## 5.3 Connection Teardown

### Graceful Close (4-way FIN handshake)

1. FIN →
    
2. ACK ←
    
3. FIN ←
    
4. ACK →
    

### RST Abort

- Immediate termination; state discarded.

# 6. ENGINEERING RATIONALE (WHY TCP IS BUILT THIS WAY)

- **Sequence numbers + ACKs:** ensure perfect ordering and recovery.
    
- **Flow control:** protects receivers from overload.
    
- **Congestion control:** prevents collapsing the Internet.
    
- **Connection state:** allows precise control of transmission.
    
- **Options:** mechanisms for modern networks (high RTT, high BDP, reordering).
    

TCP must guarantee correctness even over lossy, long, congested paths.

---

# 7. TYPICAL USE CASES & REASONS

| Application            | Why TCP?                                      | Ports           |
| ---------------------- | --------------------------------------------- | --------------- |
| HTTP / HTTPS           | Reliable byte stream, security layering (TLS) | 80 / 443        |
| SSH                    | Requires perfect ordering & reliability       | 22              |
| SMTP                   | Mail must not lose content                    | 25              |
| FTP                    | Ordered file transfer                         | 20/21           |
| Database access        | Data integrity                                | vendor-specific |
| Remote shells (Telnet) | Interactive + reliable                        | 23              |

# 8. SECURITY CONSIDERATIONS

## 8.1 SYN Flooding

- Attack fills SYN backlog with half-open connections.
    
- Mitigations: SYN cookies, TCP intercept, rate limiting, increasing backlog.
    

## 8.2 Session Hijacking & RST Injection

- Attacker guesses sequence numbers, injects RST/ACK.
    
- Mitigation: encrypted channels (TLS, SSH), firewalls, random ISN.
    

## 8.3 Banner/Service Scanning

- Because TCP uses well-known ports.
    
- Mitigation: ACLs, port-knocking, segmentation.
    

## 8.4 TCP-AO / MD5 (Control Plane Security)

- Used in BGP, critical for routing-plane integrity.
    
- Prevents spoofed connection resets/injections.
    

---

# 9. CCNA TAKEAWAYS

- TCP = **connection-oriented**, **reliable**, **ordered**, **flow-controlled**, **congestion-controlled**.
    
- Requires **3-way handshake** to start and **4-way FIN** to close.
    
- Uses **sequence numbers + acknowledgments** to manage data.
    
- Implements **windowing** for flow control.
    
- Implements **slow start, congestion avoidance, fast recovery**.
    
- Larger header than UDP.
    
- Supports **options (MSS, WS, SACK, TS)** that significantly affect performance.

# 10. LAB EXERCISES (MANDATORY FOR DEEP UNDERSTANDING)

## 10.1 Capture a TCP Handshake (Wireshark)

1. Run: `curl http://example.com/`
    
2. Capture traffic.
    
3. Inspect three packets:
    
    - SYN (check options)
        
    - SYN-ACK (check MSS negotiation)
        
    - ACK (establish stream)
        

## 10.2 Observe Congestion Control

1. Send large file via `scp` or `iperf3`.
    
2. Plot throughput vs. time.
    
3. Watch cwnd growth patterns.
    

## 10.3 Inspect TCP Options

- Analyze SYN packet: list MSS, WS, SACK Permitted, Timestamps.
    
- Verify Data Offset matches header size.
    

## 10.4 Manual Checksum

- Export packet → compute 1’s complement sum manually to verify correctness.

# MODULE 2 — TCP HEADER TOPOLOGY

**Full Bit-Level, Field-Level, and Operational Expansion**

---

# 1. THE EXACT TCP HEADER BIT DIAGRAM (RFC-accurate)

```less
0                   1                   2                   3
0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-------------------------------+-------------------------------+
|        Source Port (16)       |     Destination Port (16)     |
+-------------------------------+-------------------------------+
|                       Sequence Number (32)                    |
+---------------------------------------------------------------+
|                   Acknowledgment Number (32)                 |
+---------------+---+-----------+-------------------------------+
| Data Offset(4)|Res|  Flags(9) |         Window (16)           |
|               |   |NS CWR ECE URG ACK PSH RST SYN FIN        |
+---------------+---+-----------+-------------------------------+
|       Checksum (16)            |      Urgent Pointer (16)     |
+---------------------------------------------------------------+
|                        Options (0–40 bytes)                   |
+---------------------------------------------------------------+
|                         Application Data …                    |
+---------------------------------------------------------------+
```

Note: Maximum header = **60 bytes** (20 base + 40 options).

---

# 2. HEADER FIELD POSITIONS AND BIT RANGES

| Field                 | Start Bit | End Bit | Size       | Notes                              |
| --------------------- | --------- | ------- | ---------- | ---------------------------------- |
| Source Port           | 0         | 15      | 16 bits    | App on sender                      |
| Destination Port      | 16        | 31      | 16 bits    | App on receiver                    |
| Sequence Number       | 32        | 63      | 32 bits    | Byte indexing                      |
| Acknowledgment Number | 64        | 95      | 32 bits    | Next expected byte                 |
| Data Offset           | 96        | 99      | 4 bits     | Header length                      |
| Reserved              | 100       | 102     | 3 bits     | Typically 0                        |
| Flags                 | 103       | 111     | 9 bits     | NS,CWR,ECE,URG,ACK,PSH,RST,SYN,FIN |
| Window                | 112       | 127     | 16 bits    | Receiver buffer space              |
| Checksum              | 128       | 143     | 16 bits    | Includes pseudo-header             |
| Urgent Pointer        | 144       | 159     | 16 bits    | Rarely used                        |
| Options               | 160       | …       | 0–320 bits | MSS, WS, SACK, TS                  |

## 3. MINIMUM AND MAXIMUM LENGTH RULES

|Header Part|Size|
|---|---|
|Base Header (mandatory fields)|**20 bytes**|
|Options (if present)|1–40 bytes|
|Max Header (base + max options)|**60 bytes**|

Options MUST pad to 4-byte (32-bit) alignment using **NOP** (Option 1).

---

# 4. DEEP EXPLANATION OF EVERY TCP HEADER FIELD

---

## 4.1 Source Port (16 bits)

- Identifies sending application.
    
- Ephemeral ports usually 49152–65535.
    
- Combined with Destination Port + IPs + protocol → unique socket.
    

---

## 4.2 Destination Port (16 bits)

- Identifies receiving service.
    
- Well-known ports: 80 (HTTP), 443 (HTTPS), 22 (SSH), etc.
    

---

## 4.3 Sequence Number (32 bits)

**The heart of TCP reliability.**

- Specifies the byte number of the _first_ data byte in the segment.
    
- On the first SYN packet: sequence number = ISN (Initial Sequence Number).
    
- Wraps modulo 2³².
    
- Ensures:
    
    - Ordered delivery
        
    - Loss detection
        
    - Duplicate elimination
        
    - Stream reassembly
        

Sequence numbers apply to **bytes**, not packets.

---

## 4.4 Acknowledgment Number (32 bits)

- Indicates the next byte expected (ACK = last contiguous byte received + 1).
    
- Used for cumulative acknowledgments.
    
- Drives retransmission: missing bytes → duplicate ACKs → fast retransmit.
    
- Must be valid when ACK flag is set (almost every segment after SYN).
    

---

## 4.5 Data Offset (Header Length) (4 bits)

- Indicates header size in 32-bit words.
    
- Value × 4 = header size in bytes.
    
- Minimum = 5 (20 bytes).
    
- Maximum = 15 (60 bytes).
    

Exam question: “Why is the minimum 20 bytes?” → Because no options.

---

## 4.6 Reserved Bits (3 bits)

- Historically unused.
    
- Must be set to zero.
    
- Some stacks use leftmost bit as NS (Nonce Sum) for ECN.
    

---

## 4.7 Flags (9 bits, exact order)

|Bit|Name|Meaning|
|---|---|---|
|8|**NS**|ECN nonce (rare)|
|7|**CWR**|Congestion Window Reduced|
|6|**ECE**|ECN-Echo|
|5|**URG**|Urgent Pointer valid|
|4|**ACK**|Acknowledgment field valid|
|3|**PSH**|Push immediately to app|
|2|**RST**|Abort/reset connection|
|1|**SYN**|Start connection; sync seq numbers|
|0|**FIN**|Graceful close|

### Critical notes:

- SYN consumes one sequence number.
    
- FIN consumes one sequence number.
    
- RST DOES NOT consume a sequence number.
    
- PSH is advisory only (“deliver promptly”), not mandatory behavior.
    
- CWR/ECE are ECN congestion signals (advanced).
    

---

## 4.8 Window (16 bits)

**Receiver-Advertised Window (rwnd)**  
Flow control mechanism.

- Tells sender how much data the receiver can handle.
    
- Maximum unscaled = 65,535 bytes.
    
- Often too small for high-performance networks → Window Scale option.
    

Actual effective window = `rwnd << shift_value`.

---

## 4.9 Checksum (16 bits)

- 1’s complement checksum.
    
- Covers:
    
    - TCP header
        
    - TCP data
        
    - IP pseudo-header (both IPv4 and IPv6)
        
- Detects corrupted or misdelivered packets.
    
- Mandatory for both IPv4 and IPv6.
    

---

## 4.10 Urgent Pointer (16 bits)

- Points to the byte after the urgent data.
    
- Works only when URG flag is set.
    
- Legacy TELNET feature; largely obsolete.
    

---

## 4.11 TCP Options (0–40 bytes)

Options must:

- Appear only after Urgent Pointer
    
- End with **End of Options List (kind=0)**
    
- Be padded with **NOPs** (kind=1)
    
- Fit within a 4-byte alignment boundary
    

### The important options:

---

### MSS (Maximum Segment Size)

- Kind = 2, Length = 4
    
- Only valid in SYN and SYN-ACK
    
- Typically MTU – 40 bytes
    
- Example: 1460 for Ethernet (1500 – 20 IP – 20 TCP)
    

---

### Window Scale (WS)

- Kind = 3, Length = 3
    
- Negotiated in SYN phase
    
- Extends window by left shift: window × (2^shift)
    

---

### SACK Permitted / SACK Blocks

- Allow selective acknowledgment.
    
- Improves loss recovery dramatically.
    
- Essential on lossy or reordering networks.
    

---

### Timestamps (TSopt)

- Kind = 8, Length = 10
    
- Two 32-bit values:
    
    - TSval = sender timestamp
        
    - TSecr = echoed timestamp
        
- Used for:
    
    - RTT measurement
        
    - PAWS (Protection Against Wrapped Sequence numbers)
        

---

### NOP (No Operation)

- Kind = 1
    
- Used for alignment between options.
    

---

# 5. CHECKSUM PSEUDO-HEADER (MANDATORY EXPLANATION)

## IPv4 Pseudo-Header:

```less
Source IP (32)
Destination IP (32)
Zero (8)
Protocol = 6 (8)
TCP Length (16)
```

## IPv6 Pseudo-Header:

```java
Source IP (128)
Destination IP (128)
TCP Length (32)
Zeros (24)
Next Header = 6 (8)
```

Reason:

- Protects against misdelivery by routers.
    
- Ensures integrity across layer boundaries.
    

---

# 6. ALIGNMENT & PADDING RULES (EXTREMELY IMPORTANT)

- TCP header MUST be a multiple of 32 bits.
    
- Options MUST be padded with NOPs until the Data Offset specifies an aligned boundary.
    
- Example:
    
    - Options = MSS(4) + WS(3) + NOP(1) + SACK Permitted(2) = 10 bytes
        
    - Must pad to 12 bytes → 3 NOPs added
        

---

# 7. HOW WIRESHARK SHOWS THE TCP HEADER (FIELD-BY-FIELD)

Expect to see:

```yaml
Transmission Control Protocol
    Source Port: 54523
    Destination Port: 443
    Sequence Number: 123456789
    Acknowledgment Number: 987654322
    Header Length: 32 bytes
    Flags: 0x018 (PSH, ACK)
    Window: 65535
    Checksum: 0xabcd (valid)
    Options:
        Maximum Segment Size: 1460
        Window Scale: 7
        SACK Permitted: True
        Timestamps: TSval=..., TSecr=...
```

# 8. COMMON MISINTERPRETATIONS (WARNING FOR CCNA/ENGINEERS)

1. **ACK is set in almost all packets**, but that doesn’t mean it’s an acknowledgment-only packet.
    
2. **PSH does not force immediate transmission** — it is an advisory signal.
    
3. **RST does not consume sequence numbers.**
    
4. **Urgent Pointer is practically useless today.**
    
5. **Data Offset must match header length** — mismatch indicates malformed packet or attack.
    
6. **SYN and FIN each consume one sequence number.**
    
7. **Large window sizes require Window Scale**, not just a big Window field.



## 8. DESIGN RATIONALE (WHY EACH FIELD EXISTS)

| Field           | Reason                                   |
| --------------- | ---------------------------------------- |
| Seq/Ack numbers | Reliability, ordering, retransmission    |
| Window          | Flow control to protect receiver         |
| Flags           | Connection lifecycle control             |
| Options         | Performance tuning, congestion awareness |
| Checksum        | End-to-end integrity                     |
| Urgent Pointer  | Legacy interactive program support       |
| Ports           | Application multiplexing                 |

TCP is built for _correctness_, not minimal overhead.  
It is stateful because it must manage congestion control and reliability.

---

# 10. TROUBLESHOOTING USING TCP HEADER FIELDS

### 10.1 Missing ACKs

→ Congestion, drops, asymmetric routing.

### 10.2 Low window values

→ Receiver bottleneck or application slowness.

### 10.3 Repeated sequence numbers

→ Retransmissions → packet loss.

### 10.4 Duplicate ACKs (3)

→ Fast retransmit → indicates single-segment loss.

### 10.5 SACK blocks present

→ Receiver got some later segments → reordering occurred.

### 10.6 Options missing in SYN

→ Middlebox interference.

# 11. CCNA EXAM NOTES (TRANSPORT-LAYER ESSENTIALS)

- TCP is **reliable**, **connection-oriented**, **ordered**, **flow-controlled**, **congestion-controlled**.
    
- Performs **3-way handshake** and **4-way close**.
    
- Uses sequence numbers + ACKs for reliability.
    
- Uses **window** for flow control.
    
- Has larger header than UDP (20–60 bytes).
    
- Uses **flags** to manage state and control the connection.
    
- Supports **MSS, Window Scale, SACK, Timestamps**.