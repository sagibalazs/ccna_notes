# NAT (Network Address Translation)

> **CCNA Topic Document**  
> Framework: *How We Learn* (14 sections)

---

# 1. Base Data

| Item | Value |
|------|------|
| Topic | NAT |
| Layer | Network (L3) |
| Protocol | IPv4 (primarily) |
| RFCs | RFC 1918, RFC 3022, RFC 4787 |
| Cisco Exam | CCNA 200-301 |
| Importance | ⭐⭐⭐⭐⭐ |

---

# 2. Visualization

```
                Internet
          Public IP 198.51.100.10
                  |
           +---------------+
           | Cisco Router  |
           | NAT / PAT     |
           +---------------+
     G0/0              G0/1
Outside              Inside
203.0.113.5      192.168.1.1
                     |
      -------------------------------
      |             |              |
192.168.1.10  192.168.1.20  192.168.1.30
```

PAT Translation Table

```
192.168.1.10:51514 -> 203.0.113.5:30001
192.168.1.20:49331 -> 203.0.113.5:30002
192.168.1.30:61222 -> 203.0.113.5:30003
```

---

# 3. TL;DR

- NAT translates IP addresses.
- PAT translates IP addresses **and** TCP/UDP ports.
- PAT allows many hosts to share one public IPv4.
- Static NAT = one-to-one.
- Dynamic NAT = many-to-many using a pool.
- PAT (Overload) = many-to-one.
- NAT is configured on edge routers.

---

# 4. Importance

Problem:
- IPv4 has only about 4.3 billion addresses.
- Organizations require thousands of internal devices.
- Public IPv4 addresses are scarce.

Solution:
- Use RFC1918 private addressing internally.
- Translate only at the network edge.

Benefits:
- Conserves public IPv4
- Simplifies ISP changes
- Hides internal addressing

---

# 5. Description

Private IPv4 ranges (RFC1918)

| Network | Prefix |
|----------|--------|
|10.0.0.0|/8|
|172.16.0.0|/12|
|192.168.0.0|/16|

Address terminology

| Term | Meaning |
|------|---------|
|Inside Local|Private host address|
|Inside Global|Translated public address|
|Outside Global|Real external address|
|Outside Local|Outside address as seen internally|

Types

- Static NAT
- Dynamic NAT
- PAT (NAT Overload)

---

# 6. How it Works

Example

```
PC
192.168.1.10

↓

Router receives packet

↓

Looks for NAT rule

↓

Creates translation entry

↓

Rewrites source IP

↓

Recalculates checksum

↓

Forwards packet
```

Return traffic is matched against the translation table and rewritten back.

Order (simplified)

1. Routing decision
2. NAT lookup/create
3. Header rewrite
4. Forward

---

# 7. Pros & Cons

## Pros

- Saves IPv4 addresses
- Easy Internet access
- Hides internal network
- ISP migration easier

## Cons

- Breaks end-to-end connectivity
- Some protocols need helpers (ALGs)
- Makes troubleshooting harder
- IPsec requires NAT-T in many cases

---

# 8. Variants

## Static NAT

```
192.168.1.10
      ⇅
203.0.113.10
```

Use:
- Web servers
- Mail servers

## Dynamic NAT

Pool:

```
203.0.113.10-20
```

Hosts receive any available address.

## PAT

```
Many private hosts
      ↓
One public IP
Different ports
```

Most common enterprise and home deployment.

---

# 9. Devices / Media / Protocols

Devices

- Cisco ISR
- Cisco C891F-K9
- Firewalls
- SOHO routers

Protocols

- IPv4
- TCP
- UDP
- ICMP
- IPsec (NAT-T)
- DNS

Interfaces

```
ip nat inside
ip nat outside
```

---

# 10. Best Practices

- Perform NAT only on edge devices.
- Document translation rules.
- Use PAT unless one-to-one mapping is required.
- Reserve Static NAT for published services.
- Verify ACLs before troubleshooting NAT.
- Monitor translation tables.

---

# 11. No-Goes

- Do not configure both interfaces as inside.
- Do not forget overload for PAT.
- Do not overlap NAT pools.
- Avoid unnecessary double NAT.
- Don't forget default routes.

---

# 12. Terms

- NAT
- PAT
- RFC1918
- Translation Table
- Overload
- NAT Pool
- Static NAT
- Dynamic NAT
- Inside Local
- Inside Global
- Outside Local
- Outside Global
- NAT-T
- ALG

---

# 13. Practical Tasks

## Packet Tracer

Objective

- Configure PAT for one LAN.

Topology

```
PC ---- R1 ---- ISP
```

Configuration

```cisco
access-list 1 permit 192.168.1.0 0.0.0.255

interface g0/0
 ip nat inside

interface g0/1
 ip nat outside

ip nat inside source list 1 interface g0/1 overload
```

Verification

```cisco
show ip nat translations
show ip nat statistics
show access-lists
show ip interface brief
```

---

## GNS3

- Two routers
- One ISP router
- One LAN
- Verify Internet connectivity
- Capture packets with Wireshark before/after NAT

---

## Real Hardware (Cisco C891F-K9)

Objectives

- Configure PAT
- Configure Static NAT
- Verify translation table
- Test ICMP, HTTP, SSH

Fault Injection

- Wrong ACL
- Wrong inside/outside interface
- Missing overload
- Missing default route

Cleanup

```cisco
no ip nat inside source list 1 interface g0/1 overload
clear ip nat translation *
```

Expected Results

- Internet access works
- Translation entries appear
- Counters increase

CCNA Checks

✓ Explain Inside Local vs Inside Global

✓ Explain Static vs Dynamic vs PAT

✓ Configure PAT

✓ Troubleshoot failed translations

---

# 14. Sources

Primary references

- Cisco CCNA 200-301 Official Cert Guide
- Cisco IOS XE NAT Configuration Guide
- RFC 1918 — Address Allocation for Private Internets
- RFC 3022 — Traditional NAT
- RFC 4787 — NAT Behavioral Requirements

