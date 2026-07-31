
# CCNA - TCP and UDP


# TCP vs. UDP (CCNA-level, network-engineer view)

## 1) Side-by-side comparison (overview)


| Aspect              | TCP                                                                                                  | UDP                                                    | Why it matters                                                                                 |
| ------------------- | ---------------------------------------------------------------------------------------------------- | ------------------------------------------------------ | ---------------------------------------------------------------------------------------------- |
| Connection model    | Connection-oriented; 3-way handshake (SYN, SYN-ACK, ACK) and 4-way close (FIN/ACK) before/after data | Connectionless; no handshake, no formal session        | Impacts latency and overhead; TCP adds setup/teardown, UDP is immediate.                       |
| Reliability         | Built-in error recovery with sequencing & acknowledgments; ordered delivery                          | No error recovery or ordering by the transport layer   | Choose TCP when every byte must arrive and in order; choose UDP when timeliness > reliability. |
| Flow control        | Windowing to pace senders and protect buffers                                                        | None                                                   | TCP adapts to congestion and receiver capacity; UDP leaves pacing to the app/network.          |
| Header size         | Larger; many fields (ports, seq/ack, flags, window, checksum, etc.)                                  | Small; minimal fields (ports, length, checksum)        | Smaller headers reduce overhead; UDP is leaner.                                                |
| Multiplexing        | Uses ports; socket = {IP, protocol, port}                                                            | Same                                                   | Ports bind transport to apps/services on the host.                                             |
| Typical uses        | Web (HTTP/S), mail (SMTP/POP3/IMAP), SSH, Telnet, file transfer (FTP control/data), etc.             | VoIP, streaming, DNS queries, DHCP, TFTP, syslog, SNMP | Match protocol traits to application needs.                                                    |
| Performance profile | More CPU/bandwidth overhead; may intentionally slow down to recover/avoid loss                       | Very low overhead; sends without waiting               | UDP shines in real-time flows; TCP optimizes for correctness.                                  |
| CCNA exam focus     | “Compare TCP to UDP” and know typical functions & ports                                              | Same                                                   | Multiplexing via ports (both), flow control & reliability (TCP), ordered delivery (TCP).       |


# 1) UDP in depth

## 1.1 Header “topology” (bit layout)

```less
0                   1                   2                   3
0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-------------------------------+-------------------------------+
|        Source Port (16)       |     Destination Port (16)     |
+-------------------------------+-------------------------------+
|           Length (16)         |         Checksum (16)         |
+-------------------------------+-------------------------------+
|                          Data ...                             |
+---------------------------------------------------------------+
```

- Minimum header length: 8 bytes; no options.
    
- Payload: up to path MTU minus 8 (unless fragmentation at IP).
    

## 1.2 Fields (length → semantics → notes)

- Source Port (16): Sender’s port; may be ephemeral (49152–65535 by IANA convention).
    
- Destination Port (16): Service port (e.g., 53 for DNS, 67/68 DHCP, 69 TFTP, 161 SNMP, 514 syslog).
    
- Length (16): Header+data in bytes. Minimum value is 8. Can be used to detect truncation.
    
- Checksum (16): 1’s-complement over UDP header+data+IP pseudo-header.
    
    - IPv4: optional but widely used; 0 means “not used.”
        
    - IPv6: mandatory; cannot be 0. Protects against misdelivery and corruption.
        

## 1.3 Pseudo-header (for checksum)

- For IPv4: src IP (32), dst IP (32), zero (8), protocol=17 (8), UDP length (16).
    
- For IPv6: src IP (128), dst IP (128), UDP length (32), zeros (24), next-header=17 (8).
    

## 1.4 Operational behavior

- Connectionless: no handshake, no state in the transport layer.
    
- Ordering/retransmission: not provided. Loss/duplication possible. Application handles reliability if needed.
    
- Common patterns:
    
    - Query/response (DNS): small request, small response; app implements retry/timeouts.
        
    - Streaming/real-time (VoIP, RTP): timeliness over perfect delivery.
        
    - One-to-many (mDNS, syslog, telemetry): low overhead fan-out.
        

## 1.5 Security notes

- Amplification/reflection (open DNS/NTP/SSDP): filter spoofed sources at edge; close/limit public UDP services.
    
- No state/handshake: rely on ACLs, rate limits, and application-layer auth. For confidentiality/integrity, use DTLS or a tunnel (IPsec/WireGuard).
    

# 2) TCP in depth

## 2.1 Header “topology” (bit layout, classic view)

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
| Data Offset(4)| R |   Flags(9)|         Window (16)           |
|               |sv |(NS CWR ECE URG ACK PSH RST SYN FIN)      |
+---------------+---+-----------+-------------------------------+
|       Checksum (16)            |      Urgent Pointer (16)     |
+---------------------------------------------------------------+
|                    Options (0–40 bytes) ...                  |
+---------------------------------------------------------------+
|                           Data ...                           |
+---------------------------------------------------------------+
```

- Minimum header length: 20 bytes (no options).
    
- Maximum header length: 60 bytes (40 bytes of options), padded to 32-bit boundary.
    

## 2.2 Flag field and control bits

- Data Offset (4): Header length in 32-bit words (e.g., 5 → 20 bytes, 10 → 40 bytes).
    
- Reserved (3): Historically reserved; in modern stacks treated as 0. Some diagrams show the left-most of the 9 flags as **NS** (ECN nonce).
    
- Flags (9): NS, CWR, ECE, URG, ACK, PSH, RST, SYN, FIN.
    
    - SYN: open a connection; synchronize initial sequence numbers (ISNs).
        
    - ACK: acknowledgment field valid (almost all segments after SYN).
        
    - FIN: graceful close request.
        
    - RST: abort/reset a connection.
        
    - PSH: “push” hint to deliver to app promptly (no buffering requirement in standards).
        
    - URG: urgent pointer valid (rare today; semantics historically ambiguous).
        
    - ECE/CWR/NS: Explicit Congestion Notification (ECN) signaling.
        

## 2.3 Every field (length → semantics → key notes)

- Source Port (16) / Destination Port (16): Socket endpoint; same ranges as UDP.
    
- Sequence Number (32): Byte index of first data byte in this segment; for SYN, it’s the ISN+1 in ACKs.
    
- Acknowledgment Number (32): Next byte expected; cumulative ACK (SACK provides detail).
    
- Window (16): Receiver’s advertised window (flow control). Units are bytes, scaled by Window Scale option if present.
    
- Checksum (16): 1’s-complement over TCP header+data+IP pseudo-header (like UDP).
    
- Urgent Pointer (16): Offset from sequence number to mark “urgent” data end; largely deprecated.
    
- Options (0–40 bytes):
    
    - **MSS (2,4)**: Max Segment Size; sent in SYN; typical 1460 for Ethernet (no options); adapt for PMTU.
        
    - **Window Scale (3,3)**: Left-shift of 0–14; expands 16-bit window to 1 GB scale; negotiated in SYNs.
        
    - **SACK Permitted (4,2)** and **SACK (5, var)**: Receiver can report non-contiguous blocks received; sender retransmits only missing blocks.
        
    - **Timestamps (8,10)**: RTT measurement and PAWS (Protection Against Wrapped Sequence numbers).
        
    - **NOP (1)**: Padding; often between options.
        
    - Others (less common in CCNA): MD5 (BGP), Fast Open cookie, etc.
        

## 2.4 Connection lifecycle (state machine & mechanics)

- **3-way handshake**: SYN → SYN-ACK → ACK. Establishes sockets, ISNs, options (MSS/WS/SACK/TS).
    
- **Data transfer**:
    
    - **Reliability**: Cumulative ACKs acknowledge contiguous bytes; loss implies retransmission.
        
    - **Ordering**: Receiver reassembles by sequence number; app sees a clean byte stream.
        
    - **Flow control**: Receiver advertises Window; sender limits “in-flight” = min(cwnd, rwnd).
        
    - **Congestion control** (sender-side):
        
        - Slow Start (cwnd grows exponentially each RTT to ssthresh).
            
        - Congestion Avoidance (linear growth).
            
        - Fast Retransmit/Fast Recovery (on 3 dupACKs).
            
        - ECN: network marks (CE); endpoints signal ECE/CWR to reduce rate without loss.
            
- **Teardown**:
    
    - **Graceful**: FIN/ACK, then reverse FIN/ACK (four-way close).
        
    - **Abort**: RST.
        

## 2.5 Checksum details (practical)

- Include IP pseudo-header (v4 or v6), TCP header (with checksum=0), and data.
    
- 16-bit 1’s-complement sum; final result is 1’s-complement of the sum.
    
- Detects corruption and misdelivery. Not cryptographic—use TLS/IPsec for integrity.
    

## 2.6 Performance behaviors you should recognize

- **Nagle’s Algorithm**: Coalesce tiny writes until ACKed; good for small-packet chatter, bad for latency-sensitive interactive flows. Disable (TCP_NODELAY) when needed.
    
- **Delayed ACKs**: Receiver may delay ACK briefly (e.g., up to 200 ms) to piggyback; can interact poorly with Nagle.
    
- **MSS & PMTUD**: Choose segment sizes to avoid fragmentation.
    
- **Window Scaling**: Essential on high-BDP paths to keep the pipe full.
    

## 2.7 Security considerations

- **SYN flood**: Half-open connection state exhaustion; mitigations—SYN cookies, backlog tuning, TCP Intercept, rate-limit, SYN proxy.
    
- **RST/ACK injection / session hijack**: Guard with encryption (TLS/IPsec), sequence-number randomness, firewalling.
    
- **Banner/port scanning**: Minimize exposure; use stateful inspection and least privilege.
    
- **BGP TCP MD5 / TCP-AO** (beyond CCNA but good to know): Authenticates control-plane sessions.
    

# 3) TCP vs. UDP—decision guide (engineering view)

- Choose **UDP** when: latency is king; small query/response; app handles retries; 1-to-many; real-time media that tolerate loss.
    
- Choose **TCP** when: every byte must arrive, in order; back-pressure/flow control is needed; app is stream-oriented; security layers (TLS) are straightforward to apply.
    

# 4) Quick reference tables

## 4.1 Port ranges (IANA convention)


| Range           | Values      | Typical use                                                |
| --------------- | ----------- | ---------------------------------------------------------- |
| Well-known      | 0–1023      | System services (HTTP 80, HTTPS 443, SSH 22, DNS 53, etc.) |
| Registered      | 1024–49151  | Vendor or user services                                    |
| Dynamic/Private | 49152–65535 | Ephemeral client ports                                     |

4.2 TCP options (you’ll see in SYN/SYN-ACK)

|Option|Kind|Len|Purpose|Exam-relevant notes|
|---|--:|--:|---|---|
|MSS|2|4|Max payload per segment|Advertised only in SYN; derived from path MTU|
|Window Scale|3|3|Expand 16-bit window|Value is shift count (0–14)|
|SACK Permitted|4|2|Enables SACK|Must appear in SYN+ACK to use SACK later|
|SACK|5|var|Report received blocks|Improves loss recovery on reordering/loss|
|Timestamps|8|10|RTT, PAWS|Two 32-bit values: TSval/TSecr|
|NOP|1|1|Padding/alignment|Often between options|

# 5) Usage patterns (with why)

- **Web/SSH/Email/DB protocols → TCP**  
    Guarantees byte-accurate transfer; congestion & flow control protect hosts and network.
    
- **DNS queries → UDP first; TCP fallback**  
    UDP for speed; TCP for large/zone transfer or when response truncates/blocked.
    
- **VoIP/RTP/Streaming → UDP**  
    Loss is cheaper than delay; app adds FEC/jitter-buffer.
    
- **Telemetry/syslog/SNMP traps → UDP**  
    Low overhead; best-effort fire-and-forget fits the use case.
    

# 6) Mini-workflows (packet-level)

## 6.1 TCP connect & transfer (canonical)

1. Client → Server: SYN (MSS, WS, SACK-Perm, TS)
    
2. Server → Client: SYN-ACK (MSS, WS, SACK-Perm, TS)
    
3. Client → Server: ACK
    
4. Client → Server: PSH,ACK [seq=x, ack=y] Data…
    
5. Server → Client: ACK [ack=x+len]
    
6. Loss? Receiver sends dupACKs; sender fast-retransmits missing bytes; cwnd reduces; recovery proceeds.
    

## 6.2 UDP query/response (DNS)

1. Client → Server: UDP/53 query (TXID, flags, QNAME)
    
2. Server → Client: UDP/ephemeral response (same TXID)
    
3. No response? Client retransmits after timeout or tries another server; may switch to TCP if TC bit set.
