
# **Configure & Verify IPv4 Addressing and Subnetting**

# **A) ONE-PAGE CCNA IPv4 CONFIGURATION & VERIFY SUMMARY**

**All CLI content in ONE code block, with comments.**

```java
#######################################################################
# CCNA IPv4 – CONFIGURATION & VERIFICATION SUMMARY (ONE PAGE)
#######################################################################

# ---------------------------------------------------------------------
# 1. WHAT IPv4 CONFIGURATION DOES ON A CISCO DEVICE
# ---------------------------------------------------------------------
# When you assign an IPv4 address to an interface:
# - The device calculates the network ID + broadcast address
# - A CONNECTED ROUTE appears in the routing table
# - The interface becomes a Layer-3 boundary
# - ARP begins working for that subnet
# - Hosts use it as a default gateway (if configured)
# - The router forwards packets via routing table decisions

# ---------------------------------------------------------------------
# 2. MINIMUM CONFIG FOR IPv4 CONNECTIVITY
# ---------------------------------------------------------------------
# On routers / L3 switches:
# - Interface IPv4 address + mask
# - "no shutdown"
# - Routing (static/dynamic) for remote networks

# On L2 switches:
# - Management SVI (VLAN interface)
# - "ip default-gateway" for management reachability

# ---------------------------------------------------------------------
# 3. BASIC IPv4 CONFIGURATION (ROUTER / L3 SWITCH)
# ---------------------------------------------------------------------

# CONFIGURE IPv4 ON ROUTER INTERFACE
interface gigabitethernet0/0
 description LAN Gateway
 ip address 192.168.10.1 255.255.255.0   # Assign IPv4 + mask
 no shutdown                            # Enable interface
exit

# CONFIGURE IPv4 ON L3-SWITCH ROUTED PORT
interface gigabitethernet1/0/1
 no switchport                          # Convert to Layer 3
 ip address 10.1.1.1 255.255.255.252
 no shutdown
exit

# CONFIGURE IPv4 ON SVI (L3 SWITCH DEFAULT GATEWAY FOR VLAN)
interface vlan 10
 ip address 192.168.10.1 255.255.255.0
 no shutdown
exit
ip routing                               # Mandatory on L3 switch

# CONFIGURE SUBINTERFACES (ROUTER-ON-A-STICK)
interface gigabitethernet0/1.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
exit

# DEFAULT GATEWAY (ONLY FOR L2 SWITCHES)
ip default-gateway 192.168.10.254

# ---------------------------------------------------------------------
# 4. VERIFY IPv4 CONFIGURATION
# ---------------------------------------------------------------------

show ip interface brief
# Shows: IP address, status (up/down), VLAN/port type

show running-config interface g0/0
# Confirms the applied IPv4 configuration

show ip route
# Shows connected routes (C) and local addresses (L)
# Example expected after config:
# C 192.168.10.0/24 is directly connected, GigabitEthernet0/0
# L 192.168.10.1/32 is directly connected, GigabitEthernet0/0

show arp
# Displays ARP table; verifies L2 resolution is happening

ping 192.168.10.1
ping 8.8.8.8
# Tests local subnet & routing

traceroute 8.8.8.8
# Shows hop-by-hop forwarding

show interfaces gigabitethernet0/0
# Physical link integrity, errors, MTU, duplex, bandwidth, drops

# ---------------------------------------------------------------------
# 5. COMMON IPv4 MISCONFIGURATIONS (WHAT GOES WRONG)
# ---------------------------------------------------------------------

# WRONG SUBNET MASK
# - Host appears “out of network”
# - ARP fails → ping fails
# - No connected route appears

# WRONG GATEWAY
# - Local communication OK
# - Remote networks unreachable

# INTERFACE DOWN / SHUTDOWN
# - No connected route
# - No ARP activity
# Fix: "no shutdown"

# VLAN / SVI DOWN
# - SVI line protocol = down
# - No active access ports in that VLAN
# Fix: add active ports or trunk the VLAN

# SUBINTERFACE WITHOUT ENCAPSULATION
# - Traffic dropped because no VLAN tag
# Fix: "encapsulation dot1Q X"

# DUPLICATE IP ADDRESS
# - ARP flapping
# - Cisco logs "%IP-4-DUPADDR"
# Fix: ensure unique IP assignments

# ---------------------------------------------------------------------
# 6. GOLDEN IPv4 RULES FOR CCNA
# ---------------------------------------------------------------------
# - Routers DO NOT use ip default-gateway → they use routing
# - L2 switches DO use ip default-gateway
# - Interfaces must be UP/UP to install connected routes
# - ARP is required for same-subnet forwarding
# - Wrong mask = wrong network, even if IP looks similar
# - Only ONE primary IPv4 per interface
# - SVI depends 100% on VLAN operational status
# - Subinterfaces require dot1Q tags or traffic won’t flow

#######################################################################
# END OF IPv4 THEORY SUMMARY – READY TO COPY
#######################################################################
```

# **B) IPv4 TROUBLESHOOTING DECISION TREE (CCNA LEVEL)**

This has two parts:

1. **Diagram — How an engineer thinks through IPv4 problems in the correct order**
    
2. **One continuous copy-paste code block with:**  
    • All troubleshooting steps  
    • All relevant commands  
    • Explanations as comments  
    • Conditions, interpretations, expected outputs  
    • Fix actions

```less
                           +--------------------------+
                           |     Problem Reported     |
                           |  "Can't reach IP/device" |
                           +------------+-------------+
                                        |
                                        v
                         +--------------+---------------+
                         | 1. Physical Interface Status |
                         +--------------+---------------+
                                        |
                      +----------------+------------------+
                      |                                   |
           If down/down                                  If up/up
   (cable, shutdown, VLAN missing)                        |
                      |                                   v
                      |                      +------------+--------------+
                      |                      | 2. Is IPv4 configured?    |
                      |                      | show ip int br → correct? |
                      |                      +------+---------------------+
                      |                             |
                      v                             v
Fix physical,        |                 If no IP OR wrong mask OR wrong network
correct VLAN         |                     → address/mask/gateway fix
                     |
                     v                             |
                     |                             v
             +-------+--------+        +-----------+-----------+
             | 3. ARP working? | ----> | 4. Routing table OK? |
             +-------+--------+        +-----------+-----------+
                     |                             |
                     v                             v
       If no ARP entry created       If no connected/route → routing issue
   or wrong MAC → L2 or VLAN issue   (static, default, dynamic missing)
                     |
                     v                             |
                     |                             v
             +-------+---------+        +----------+-------------+
             | 5. Ping gateway |------->| 6. Ping remote network |
             +-------+---------+        +----------+-------------+
                     |                             |
                     v                             v
           If fails → local L2/L3       If fails → routing/NAT/ACL mismatch
           gateway/VLAN/ARP issue
                     |
                     v
             +-------+--------+
             | 7. ACL/NAT?    |
             +-------+--------+
                     |
                     v
         If blocked: fix ACL/NAT/firewall
                     |
                     v
             +-------+--------+
             |   PROBLEM FIXED |
             +----------------+
```

# **2. One-Block IPv4 Troubleshooting Sheet (All Commands + Comments)**

```less
#######################################################################
# CCNA IPv4 – FULL TROUBLESHOOTING DECISION TREE (COMMAND SHEET)
#######################################################################

# ---------------------------------------------------------------------
# STEP 1 – CHECK PHYSICAL/LINK STATE
# ---------------------------------------------------------------------
show ip interface brief
# Look for: "up/up"
# If: "administratively down" → run "no shutdown"
# If: "down/down" → cable unplugged, wrong port, hardware failure
# If: "up/down" → L2 mismatch (speed/duplex/VLAN/trunk)

show interfaces <int>
# Check CRC errors → bad cable or duplex mismatch
# Check MTU mismatch → PMTUD issues

# FIX:
# - no shutdown
# - replace cable
# - fix speed/duplex (or use auto)
# - correct VLAN on switch


# ---------------------------------------------------------------------
# STEP 2 – VERIFY IPV4 CONFIGURATION (LOCAL FACTS)
# ---------------------------------------------------------------------
show running-config interface <int>
# Confirm:
# - IP address correct?
# - Mask correct?
# - Correct subnet?
# - Not assigned to wrong interface?

# If using L2 switch management:
show running-config | include ip\ default-gateway

# FIX:
# - correct IP/mask
# - correct default-gateway (L2 switch only)


# ---------------------------------------------------------------------
# STEP 3 – VERIFY CONNECTED ROUTES (ROUTER/L3 SWITCH)
# ---------------------------------------------------------------------
show ip route
# EXPECT:
# C 192.168.10.0/24 is directly connected, Gi0/0
# L 192.168.10.1/32 is directly connected, Gi0/0

# If missing:
# - Interface is DOWN
# - Wrong mask → device thinks you’re in a different network
# - Duplicate IP → ARP instability prevents route installation

# FIX:
# - bring interface up
# - correct mask
# - ensure unique IP


# ---------------------------------------------------------------------
# STEP 4 – ARP RESOLUTION (L2/L3 CONNECTIVITY)
# ---------------------------------------------------------------------
show arp
# If ARP does NOT learn MAC → device cannot discover host
# Causes:
# - host offline
# - VLAN mismatch
# - trunk misconfigured
# - wrong mask (host thinks router is remote)
# - duplicate IP responding with wrong MAC

# FIX:
# - check VLAN membership
# - check trunk allowed VLANs
# - correct IP/mask on host
# - eliminate duplicate IPs


# ---------------------------------------------------------------------
# STEP 5 – PING TESTS (LOCAL, THEN REMOTE)
# ---------------------------------------------------------------------

# TEST LOCAL INTERFACE (ROUTER TO ITSELF)
ping 192.168.10.1
# If this fails → interface is DOWN or no IP applied

# TEST SAME SUBNET
ping 192.168.10.50
# If fails AND ARP empty → VLAN/ARP/gateway issue

# TEST DEFAULT GATEWAY (ON HOST SIDE)
ping 192.168.10.1

# TEST REMOTE NETWORKS
ping 8.8.8.8
# If local works but remote fails → routing issue


# ---------------------------------------------------------------------
# STEP 6 – TRACE PATH TO SEE ROUTING DECISIONS
# ---------------------------------------------------------------------
traceroute <IP>
# Look for:
# - stops at gateway → gateway doesn't know route
# - stops at core → ACL/NAT/firewall blocking
# - looping → routing table corruption or static misconfig


# ---------------------------------------------------------------------
# STEP 7 – VERIFY ROUTING PROTOCOLS OR STATIC ROUTES
# ---------------------------------------------------------------------
show ip route <network>
# Confirms route exists

show ip protocols
# Confirms routing protocol status

show run | section router
# Shows routing configuration

# FIX:
# - add missing static routes
# - correct OSPF networks
# - correct EIGRP AS
# - add default route: ip route 0.0.0.0 0.0.0.0 <next-hop>


# ---------------------------------------------------------------------
# STEP 8 – ACL, FIREWALL, SECURITY BLOCKING?
# ---------------------------------------------------------------------
show access-lists
show ip interface <int>
# Look for "Inbound/Outbound access list"

# FIX:
# - remove or correct ACL
# - check NAT rules
# - ensure ICMP allowed for troubleshooting


# ---------------------------------------------------------------------
# STEP 9 – DUPLICATE IP DETECTION
# ---------------------------------------------------------------------
show log | include DUPADDR
# ERROR:
# %IP-4-DUPADDR: Duplicate address 192.168.10.1 on interface...

# Fix:
# - locate offending device via MAC
# - reassign IP


# ---------------------------------------------------------------------
# STEP 10 – VLAN & TRUNK CHECKS (SWITCH ISSUES)
# ---------------------------------------------------------------------
show vlan brief
# Is the VLAN active?

show interfaces trunk
# Is VLAN allowed on trunk?

show interfaces switchport
# Access or trunk mode correct?

# FIX:
# - add VLANs to trunk: switchport trunk allowed vlan add X
# - correct access VLAN: switchport access vlan X


# ---------------------------------------------------------------------
# STEP 11 – SUBINTERFACES ON ROUTERS
# ---------------------------------------------------------------------
show run interface g0/1.10
# Ensure:
# - encapsulation dot1Q 10
# - IP assigned
# Missing dot1Q → NO TRAFFIC WILL PASS

# FIX:
# interface g0/1.10
#  encapsulation dot1Q 10
#  ip address 192.168.10.1 255.255.255.0


# ---------------------------------------------------------------------
# STEP 12 – FINAL CONNECTIVITY VALIDATION
# ---------------------------------------------------------------------
ping <remote-host> source <local-interface>
# Tests routing path unidirectionally

show ip cef
# Confirms CEF forwarding entries exist

#######################################################################
# END OF IPv4 TROUBLESHOOTING DECISION TREE COMMAND SHEET
#######################################################################

```

# **C) IPv4 VERIFICATION CHECKLIST (ENGINEER LEVEL)**

This is the definitive checklist used in operations and troubleshooting.

```less
#######################################################################
# CCNA IPv4 – FULL VERIFICATION CHECKLIST (ENGINEER LEVEL)
#######################################################################
# ONE BLOCK – ALL COMMANDS + COMMENTS – READY TO COPY
#######################################################################

# ---------------------------------------------------------------------
# 0. PRE-VERIFICATION OVERVIEW (THE LOGICAL CHECK SEQUENCE)
# ---------------------------------------------------------------------
# VERIFY IN THIS EXACT ORDER:
# 1. Physical state (Layer 1)
# 2. Link & VLAN state (Layer 2)
# 3. IPv4 config correctness (address, mask, gateway)
# 4. Connected routes creation (Layer 3)
# 5. ARP resolution
# 6. Local subnet reachability
# 7. Default gateway operation
# 8. Routing table correctness (static/dynamic/default)
# 9. Path verification (ping/traceroute)
# 10. ACL / NAT / firewall influence
# 11. Return traffic (asymmetric routing issues)
# 12. End-to-end validation


# ---------------------------------------------------------------------
# 1. LAYER 1 – PHYSICAL LINK VERIFICATION
# ---------------------------------------------------------------------
show ip interface brief
# EXPECT: up/up
# If down/down  → bad cable, SFP mismatch, disabled port
# If admin down → "no shutdown"
# If up/down    → L2 mismatch, VLAN, encapsulation issue

show interfaces <int>
# Check:
# - Speed/duplex
# - CRC errors
# - Input errors
# - Output drops
# - MTU
# MTU mismatch can break ping > 1472 bytes with DF bit set.


# ---------------------------------------------------------------------
# 2. LAYER 2 – VLAN/TRUNK STATE
# ---------------------------------------------------------------------
show interfaces switchport
# Access mode? Trunk mode? Correct VLAN?

show vlan brief
# VLAN present and ACTIVE?

show interfaces trunk
# Trunk operational? VLAN allowed?

# FIX:
# switchport access vlan <vlan>
# switchport trunk allowed vlan add <vlan>


# ---------------------------------------------------------------------
# 3. LAYER 3 – IPv4 CONFIGURATION CORRECT?
# ---------------------------------------------------------------------
show running-config interface <int>
# Look for:
# - Correct IP address
# - Correct subnet mask
# - Not using duplicate IP
# - No overlapping subnets

# WRONG MASK = the #1 cause of IPv4 routing failures in CCNA.


# ---------------------------------------------------------------------
# 4. CONNECTED ROUTES INSTALLED?
# ---------------------------------------------------------------------
show ip route connected
# EXPECT:
# C 192.168.10.0/24 is directly connected, Gi0/0
# L 192.168.10.1/32 is directly connected, Gi0/0

# IF MISSING:
# - Interface down
# - Wrong mask
# - Duplicate IP/ARP instability
# - IP stuck in DHCP conflict


# ---------------------------------------------------------------------
# 5. ARP RESOLUTION FUNCTIONAL?
# ---------------------------------------------------------------------
show arp
# If no entry for 192.168.10.X after ping → ARP failure

# ARP fails when:
# - Wrong mask (host thinks gateway remote)
# - VLAN mismatch
# - Native VLAN mismatch
# - Duplicate IP
# - Host NIC down or wrong IP

# TEST ARP DIRECTLY:
ping <host-on-same-subnet>


# ---------------------------------------------------------------------
# 6. CAN DEVICE REACH ITSELF AND SAME-SUBNET NEIGHBORS?
# ---------------------------------------------------------------------
ping <interface-IP>     # tests local IP stack
ping <host-in-same-vlan>

# If same subnet ping fails:
# - ARP missing or incorrect
# - Host wrong IP or gateway
# - VLAN mismatch
# - Access port in wrong VLAN
# - Subinterface missing dot1q encapsulation


# ---------------------------------------------------------------------
# 7. DEFAULT GATEWAY VERIFICATION (HOST SIDE)
# ---------------------------------------------------------------------
# On router/L3 switch:
# Routers DO NOT use ip default-gateway.

# On L2 switch:
show running-config | include ip\ default-gateway

ping <default-gateway>
# If fails → local VLAN/gateway/ARP issue.


# ---------------------------------------------------------------------
# 8. ROUTING TABLE CORRECT? (STATIC/DYNAMIC)
# ---------------------------------------------------------------------
show ip route
# Verify:
# - Connected networks
# - Static routes
# - Default route (0.0.0.0/0)
# - Dynamic routes (OSPF, EIGRP, RIP)

# For specific network:
show ip route <network>

# Routing protocol status:
show ip protocols

# FIX EXAMPLES:
# ip route 0.0.0.0 0.0.0.0 <next-hop>
# router ospf 1
#  network 192.168.10.0 0.0.0.255 area 0


# ---------------------------------------------------------------------
# 9. PATH VERIFICATION (PING + TRACEROUTE)
# ---------------------------------------------------------------------
ping <remote-ip>
# Expect: <time> ms replies

traceroute <remote-ip>
# Identifies hop where forwarding fails.

# If path stops inside your network:
# - ACL blocks
# - Firewall blocks
# - No return route
# - NAT misconfigured
# - Asymmetric routing


# ---------------------------------------------------------------------
# 10. ACL / SECURITY FILTERS BLOCKING TRAFFIC?
# ---------------------------------------------------------------------
show access-lists

show ip interface <int>
# Look for:
# Inbound access list is X
# Outbound access list is X

# FIX:
# - Permit required traffic
# - Remove wrong ACL
# - Add temporary permit icmp any any for test


# ---------------------------------------------------------------------
# 11. NAT VERIFICATION (IF USED)
# ---------------------------------------------------------------------
show ip nat translations
show ip nat statistics

# If no translation appears → NAT rule wrong
# If overload not working → wrong ACL on "ip nat inside source list"


# ---------------------------------------------------------------------
# 12. RETURN PATH (ASYMMETRIC ROUTING DETECTION)
# ---------------------------------------------------------------------
# Problem:
# You can ping remote → but remote cannot ping back.

# Check:
show ip route <local-subnet> on the remote router

# If route missing → remote device sends replies wrong direction.


# ---------------------------------------------------------------------
# 13. END-TO-END VALIDATION (FINAL CHECK)
# ---------------------------------------------------------------------
# TEST WITH SPECIFIC SOURCE INTERFACE:
ping <destination> source <interface-IP>
# Reveals asymmetric routing problems.

# VERIFY FORWARDING TABLE:
show ip cef
# CEF must have entry for prefix

# VERIFY ARP AGAIN:
show arp

# INTERFACE COUNTERS FOR ERRORS:
show interfaces <int>

#######################################################################
# END – COMPLETE IPv4 VERIFICATION CHECKLIST
#######################################################################

```

# **D) COMMON CCNA IPv4 EXAM TRAPS & HOW TO DETECT THEM**

These are the _real_ traps Cisco uses repeatedly in the exam.  
If you know these, you avoid 90% of CCNA IPv4 mistakes.

```less
#######################################################################
# CCNA IPv4 – EXAM TRAPS & HOW TO DETECT THEM (FULL SHEET)
#######################################################################

# ---------------------------------------------------------------------
# TRAP 1 – WRONG SUBNET MASK
# ---------------------------------------------------------------------
# One of the most common traps:
# Example:
# Router IP: 192.168.10.1 255.255.0.0  (WRONG)
# Host   IP: 192.168.10.50 255.255.255.0 (CORRECT)
#
# RESULT:
# - Host believes router is LOCAL (same subnet)
# - Router believes host is REMOTE (different subnet)
# - ARP works on one side but not the other
# - Pings fail in one direction

# DETECTION:
show ip interface brief
show running-config interface <int>
show ip route | begin Connected
show arp

# FIX:
# Correct mask on either side.
# This is the most tested IPv4 configuration mistake in CCNA.


# ---------------------------------------------------------------------
# TRAP 2 – DUPLICATE IP ADDRESS
# ---------------------------------------------------------------------
# Two devices configured with the same IP.
# Symptoms:
# - ARP flapping / alternating MAC addresses
# - Connectivity unstable
# - Cisco logs: %IP-4-DUPADDR

# DETECTION:
show arp
show log | include DUPADDR

# FIX:
# - Assign unique IP to each interface.
# - Identify the offender by MAC.


# ---------------------------------------------------------------------
# TRAP 3 – INTERFACE SHUTDOWN
# ---------------------------------------------------------------------
# Cisco routers ship with interfaces ADMIN DOWN by default.

# DETECTION:
show ip interface brief
# Look for “administratively down, down”

# FIX:
interface <int>
 no shutdown


# ---------------------------------------------------------------------
# TRAP 4 – MISSING IP ROUTE BACK (NO RETURN ROUTE)
# ---------------------------------------------------------------------
# Classic exam trick:
# You can ping *outbound* but remote network cannot ping back.
#
# Why?
# The remote router does not know how to return traffic to your network.

# DETECTION:
show ip route <local-subnet> on remote router

# FIX:
# Add static route or dynamic routing.
# ip route <local-subnet> <mask> <next-hop>


# ---------------------------------------------------------------------
# TRAP 5 – WRONG DEFAULT GATEWAY (HOST OR SWITCH)
# ---------------------------------------------------------------------
# A host or L2 switch has incorrect default gateway.
# Local pings succeed; remote networks fail.

# DETECTION:
# On host: ipconfig / ifconfig
# On L2 switch:
show running-config | include default-gateway

# FIX:
ip default-gateway <router-IP>


# ---------------------------------------------------------------------
# TRAP 6 – SVI DOWN BECAUSE VLAN HAS NO ACTIVE PORTS
# ---------------------------------------------------------------------
# On multilayer switch, SVI = VLAN interface.
# An SVI line protocol is DOWN unless:
# - At least one port is UP AND assigned to that VLAN

# DETECTION:
show ip interface brief
# Vlan 10: up/down  ← VLAN exists but no active ports

show vlan brief

# FIX:
# - Put at least one port in VLAN 10:
interface g1/0/1
 switchport access vlan 10
 no shut


# ---------------------------------------------------------------------
# TRAP 7 – ROUTER-ON-A-STICK WITHOUT DOT1Q ENCAPSULATION
# ---------------------------------------------------------------------
# Classic CCNA scenario:
# Subinterfaces missing encapsulation → NO traffic passes.

# DETECTION:
show run interface g0/1.10
# If “encapsulation dot1Q <vlan>” is missing → dead interface

# FIX:
interface g0/1.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0


# ---------------------------------------------------------------------
# TRAP 8 – VLAN NOT ALLOWED ON TRUNK
# ---------------------------------------------------------------------
# Hosts in VLAN cannot reach gateway because VLAN not carried on trunk.

# DETECTION:
show interfaces trunk
# Look at Allowed VLANs

# FIX:
switchport trunk allowed vlan add <vlan>


# ---------------------------------------------------------------------
# TRAP 9 – WRONG NATIVE VLAN (MISMATCH)
# ---------------------------------------------------------------------
# Trunk native VLAN must match on both sides.
# A mismatch causes unpredictable ARP and broadcast behavior.

# DETECTION:
show interfaces trunk
show interfaces switchport

# FIX:
switchport trunk native vlan <vlan>


# ---------------------------------------------------------------------
# TRAP 10 – FORGETTING TO ENABLE IP ROUTING ON L3 SWITCH
# ---------------------------------------------------------------------
# Without “ip routing”, an L3 switch behaves like a pure L2 switch.
# VLAN SVI gateways exist but DO NOT route between VLANs.

# DETECTION:
show running-config | include ip\ routing

# FIX:
ip routing


# ---------------------------------------------------------------------
# TRAP 11 – STATIC ROUTE USING THE WRONG EXIT INTERFACE
# ---------------------------------------------------------------------
# Static route must reference:
# - next-hop IP OR
# - exit interface (only for point-to-point)

# BAD EXAMPLE:
ip route 10.1.0.0 255.255.0.0 g0/1   ← This may break ARP and routing

# GOOD EXAMPLE:
ip route 10.1.0.0 255.255.0.0 192.168.1.1

# DETECTION:
show ip route


# ---------------------------------------------------------------------
# TRAP 12 – ACL APPLIED IN WRONG DIRECTION OR WRONG INTERFACE
# ---------------------------------------------------------------------
# ACL effects:
# - applied inbound/outbound incorrectly
# - missing permit statements
# - implicit deny at the end

# DETECTION:
show access-lists
show ip interface <int>
# Look for:
# "Inbound access list ..."
# "Outbound access list ..."

# FIX:
# - move ACL to correct interface
# - change direction
# - add permit rules before deny


# ---------------------------------------------------------------------
# TRAP 13 – HOST MASK MAKES NETWORK OVERLAP IMPOSSIBLE
# ---------------------------------------------------------------------
# Hosts may appear in the same network but masks create mismatch.

# EXAMPLE TRAP:
Host A: 10.0.0.5/8
Host B: 10.0.0.6/24

# They look close but ARE NOT in the same subnet.

# DETECTION:
show ip interface brief
# Compare mask lengths.

# FIX:
# Use matching masks and networks.


# ---------------------------------------------------------------------
# TRAP 14 – MTU MISMATCH BREAKS ROUTING
# ---------------------------------------------------------------------
# Usually seen in WAN or GRE exam questions.

# DETECTION:
show interfaces <int> | include MTU

# FIX:
interface <int>
 mtu 1500


# ---------------------------------------------------------------------
# TRAP 15 – HOSTS USING WRONG DNS, BELIEVED AS CONNECTIVITY ISSUE
# ---------------------------------------------------------------------
# Many exam sims include DNS failing, not IPv4 itself.

# DETECTION:
ping 8.8.8.8   # works
ping google.com  # fails → DNS issue, NOT routing

# FIX:
Set correct DNS on host:
ip name-server <dns-ip>


#######################################################################
# END – FULL CCNA IPv4 EXAM TRAPS & DETECTION SHEET
#######################################################################
```


## **Device Type: L3 Switch**

(Also valid for multilayer switches such as 3560/3750/3850/9300.)

```bash
############################################################
# FASTETHERNET INTERFACE (Fa0/x)
############################################################
conf t
 interface fastethernet0/1
  description LAN Access Interface
  no switchport                  # Enable Layer 3 mode
  ip address 192.168.10.1 255.255.255.0
  no shutdown
 end

# VERIFICATION
show ip interface brief
show running-config interface fastethernet0/1
show interfaces fastethernet0/1


############################################################
# GIGABIT ETHERNET INTERFACE (Gi0/x)
############################################################
conf t
 interface gigabitethernet0/1
  description Routed Uplink
  no switchport
  ip address 10.10.10.1 255.255.255.252
  no shutdown
 end

# VERIFICATION
show ip interface brief
show interfaces gigabitethernet0/1
show ip route connected


############################################################
# 10 GIGABIT ETHERNET (TenGig0/x or Te1/0/x)
############################################################
conf t
 interface tengigabitethernet1/0/1
  description WAN/CORE Routed Link
  no switchport
  ip address 172.16.200.1 255.255.255.252
  no shutdown
 end

# VERIFICATION
show interfaces tenGigabitEthernet1/0/1
show ip int br


############################################################
# FIBER INTERFACE (SFP/SFP+ modules: Gi0/1, Te1/1, etc.)
############################################################
conf t
 interface gigabitethernet1/1/1
  description Fiber Uplink
  no switchport
  ip address 192.168.50.1 255.255.255.0
  no shutdown
 end

# VERIFICATION
show controllers ethernet-controller gi1/1/1
show interfaces gi1/1/1 transceiver
show ip interface brief


############################################################
# SVI (Switch Virtual Interface) – VLAN INTERFACE
############################################################
conf t
 interface vlan 10
  description User VLAN Gateway
  ip address 192.168.10.1 255.255.255.0
  no shutdown
 exit

# Ensure L3 Switching enabled
ip routing

# VERIFICATION
show ip interface brief
show running-config interface vlan 10
show ip route


############################################################
# BUNDLE/PORT-CHANNEL (ROUTED)
############################################################
conf t
 interface port-channel1
  description Routed LACP Bundle to Core
  no switchport
  ip address 10.1.1.1 255.255.255.252
 exit

# Add members
 interface gi0/2
  channel-group 1 mode active
 interface gi0/3
  channel-group 1 mode active

# VERIFICATION
show etherchannel summary
show ip interface brief
show interfaces port-channel1
```

##  Device Type: Router (ISR, CSR1000v)

```bash
############################################################
# FASTETHERNET (Fa0/x)
############################################################
conf t
 interface fastethernet0/0
  description LAN Gateway
  ip address 192.168.1.1 255.255.255.0
  no shutdown
 end

# VERIFY
show ip int br
show int fa0/0


############################################################
# GIGABIT ETHERNET (Gi0/x)
############################################################
conf t
 interface gigabitethernet0/1
  description WAN Link
  ip address 203.0.113.2 255.255.255.252
  no shutdown
 end

# VERIFY
show ip route
show interfaces gi0/1


############################################################
# SERIAL INTERFACE (WAN, HDLC/PPP)
############################################################
conf t
 interface serial0/0/0
  description Point-to-Point Link
  encapsulation ppp             # or HDLC by default
  ip address 10.0.0.1 255.255.255.252
  no shutdown
 end

# Clocking only on DCE:
# clock rate 64000

# VERIFY
show controllers serial0/0/0
show ip interface brief
show interfaces serial0/0/0


############################################################
# SUBINTERFACES (ROUTER-ON-A-STICK)
############################################################
conf t
 interface gigabitethernet0/0.10
  encapsulation dot1q 10
  ip address 192.168.10.1 255.255.255.0

 interface gigabitethernet0/0.20
  encapsulation dot1q 20
  ip address 192.168.20.1 255.255.255.0

 interface gigabitethernet0/0
  no shutdown
 end

# VERIFY
show ip int br
show interfaces gi0/0.10
show interfaces trunk


############################################################
# MULTIPOINT (Frame Relay – legacy but still CCNA relevant)
############################################################
conf t
 interface serial0/0/0
  encapsulation frame-relay
  ip address 172.16.1.1 255.255.255.0
 exit

# VERIFY
show frame-relay map
show interfaces serial0/0/0


############################################################
# LOOPBACK INTERFACE
############################################################
conf t
 interface loopback0
  ip address 1.1.1.1 255.255.255.255
 exit

# VERIFY
show ip interface brief
show ip route connected

```

# **Verification Commands (Universal)**

Use these always on routers and multilayer switches:

```less
show ip interface brief           # IP + status summary
show running-config | section interface
show ip route                     # Check connected networks
ping <IP>                         # Basic reachability
traceroute <IP>                   # Path verification
show interfaces <name>            # Duplex, errors, MTU, drops
show arp                          # ARP resolution
show protocols                    # IP enabled confirmation
```


# **1. What IPv4 Configuration Means on a Cisco Device**

When you assign an IPv4 address to a Cisco interface, you define:

1. **The network where this interface belongs** (based on IP + mask)
    
2. **The host address for this interface within that network**
    
3. **Which connected routes the router will add**
    
4. **How devices in that network reach their default gateway** (if this interface is the gateway)
    
5. **The Layer-3 identity for the interface (ARP, routing, forwarding)**
    

### The configuration creates the following internal elements:

### **1. Connected Route**

Example:

```nginx
ip address 192.168.10.1 255.255.255.0
```

This auto-creates:

```nginx
C 192.168.10.0/24 is directly connected, via Gi0/0
```

### **2. ARP Table Entry Behavior**

The interface now participates in Layer-2 resolution:

- It will **reply to ARP requests** for _its_ IP
    
- It will **send ARP requests** for hosts in its subnet
    

### **3. Routing Decision Inclusion**

The interface becomes valid for routing packets **only after line protocol is UP**  
(link must be up + protocol up).

### **4. Broadcast Domain Membership**

The device joins one broadcast domain based on the mask:

- All hosts in the same network = same broadcast domain
    
- The default gateway must be in the same network
    

---

# **2. What MUST Be Configured for IPv4 To Work**

IPv4 configuration is **not just an address**. Minimal operational IPv4 requires:

### **A. Interface IPv4 Address & Mask**

Identifies network + host.

### **B. Interface Admin State**

Interfaces ship as shutdown on routers.

```perl
no shutdown
```

### **C. Default Gateway**

(Only required for **L2 switches** or **hosts**)

```perl
ip default-gateway 192.168.10.254
```

Routers/L3 switches use **routing tables**, not default-gateway.

### **D. Routing**

Required to reach remote networks:

- Connected routes come automatically
    
- Static routes (if needed)
    
- Dynamic routing (OSPF, EIGRP, RIP)
    

Without routing, only same-subnet communication works.

# **3. What Happens When IPv4 Is Configured (Step-by-Step Mechanism)**

### **Step 1 — Interface Initialization**

`no shutdown` brings up:

- Physical layer (Layer 1)
    
- Line protocol (Layer 2)
    
- IP engine becomes active (Layer 3)
    

### **Step 2 — Device Calculates Network ID**

Example:


```perl
IP: 192.168.10.1
Mask: 255.255.255.0
```

→ Subnet = 192.168.10.0/24  
→ Broadcast = 192.168.10.255  
(no math explanation here; stating the result)

### **Step 3 — Device Creates a Connected Route**

Router/L3 switch adds this route automatically.

### **Step 4 — ARP Cache Activity**

Communication inside subnet:

- To send packet → device checks ARP table
    
- If no entry → ARP Request
    
- Host responds → entry stored in ARP table
    

### **Step 5 — Device Accepts Packets to Its Address**

Packets hitting interface with matching IP → accepted  
Packets hitting broadcast → processed for ARP etc.

### **Step 6 — Forwarding Behavior Established**

Router can now:

- Receive packets on this interface
    
- Forward packets using routing rules
    
- Drop packets outside its network if no route available
    

---

# **4. Verification Theory (How to Confirm IPv4 Works)**

Verification consists of four planes: Administrative, Operational, Control, Forwarding.

---

## **4.1 Administrative Verification**

“Is the configuration correct?”

```perl
show running-config interface <int>
```

Check:

- Status = up/up
    
- Correct IP
    
- SVI state (line protocol depends on VLAN)
    

If down/down → cable/hardware  
If up/down → Layer 2 mismatch or VLAN issue

---

## **4.3 Control-plane Verification**

“Does routing know about this network?”

```perl
show ip route
```

You should see:

```perl
C 192.168.10.0/24 is directly connected, Gi0/0
```

If connected route is **missing**:

- Interface is down
    
- Wrong mask
    
- Overlapping networks
    
- IP not applied correctly
    

---

## **4.4 Forwarding Verification**

“Can the device actually send packets?”

### **Ping**

```perl
ping <destination-IP>
```

Local → tests ARP  
Remote → tests routing

### **Traceroute**

```less
traceroute <IP>
```

Shows routing path, TTL hops.

### **ARP Cache**

```less
show arp
```

Verifies per-host Layer-2 resolution.

### **Interface-level counters**

```less
show interfaces <int>
```

Check:

- input/output errors
    
- collisions
    
- MTU mismatches
    
- runts/giants
    
- CRC errors
    

These indicate physical or duplex issues.

---

# **5. IPv4 Misconfiguration: What Goes Wrong and Why**

This is crucial CCNA theory.

### **A. Wrong Mask**

Symptoms:

- Hosts appear “out of network”
    
- ARP fails
    
- No connected route
    

Why: Network boundary is different → device thinks host is remote.

### **B. Wrong Gateway**

Symptoms:

- Can ping same subnet
    
- Cannot reach outside subnet
    

Why: Default gateway handles external routing.

### **C. Duplicate IP**

Symptoms:

- ARP flapping
    
- Intermittent connectivity
    
- `%IP-4-DUPADDR` log messages
    

### **D. Interface Down**

Symptoms:

- No connected route
    
- No ARP
    
- “down/down” or “administratively down”
    

### **E. VLAN Issues (SVI)**

Symptoms:

- SVI line protocol down
    
- No L2 members in VLAN
    
- Cannot ping gateway
    

Why: SVI depends on at least one active port in that VLAN.

### **F. Subinterface VLAN Tagging Errors**

Symptoms:

- Host cannot reach router
    
- "Native VLAN mismatch"
    
- "Encapsulation dot1Q <x>" missing
    

---

# **6. Key CCNA IPv4 Facts to Memorize**

1. **Routers do not use default-gateway** → they use routing tables.
    
2. **L2 switches require ip default-gateway** for management VLAN only.
    
3. **Interface MUST be up/up to install connected routes.**
    
4. **ARP is required for any IPv4 forwarding inside the subnet.**
    
5. **Wrong mask = hosts in different networks even if visually similar.**
    
6. **Clock rate only on DCE serial.**
    
7. **SVI requires VLAN to exist AND have active ports.**
    
8. **Subinterfaces require dot1q encapsulation to forward.**
    
9. **You can only have 1 primary IPv4 address per interface.**
    
10. **Duplicate IP = unpredictable behavior, logs, and ARP instability.**





