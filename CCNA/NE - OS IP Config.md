
CCNA
"Verify IP parameters for Client OS (Windows, Mac OS, Linux)"

# **VERIFY IP PARAMETERS FOR CLIENT OS (Windows, macOS, Linux)**

_(Engineer Format Document – CCNA Prep)_

---

##  SUPER CHEAT SHEET (ALL OS IN ONE BLOCK)

```less
#######################################################################
# SUPER CHEAT SHEET – WINDOWS + MACOS + LINUX
# VERIFY & CONFIGURE IPv4 / IPv6 PARAMETERS (FULL CCNA SCOPE)
#######################################################################

#######################################################################
# WINDOWS
#######################################################################
ipconfig /all                        # IPv4/IPv6, DNS, DHCP
ipconfig /release /renew             # DHCP renew
Get-NetIPConfiguration               # full interface info
Get-NetIPAddress                     # show addresses
Get-DnsClientServerAddress           # DNS servers
route print                          # routing table
arp -a                               # ARP
netsh interface ipv6 show neighbors  # IPv6 ND
ping 8.8.8.8                         # IPv4 test
ping -6 2001:4860:4860::8888         # IPv6 test
nslookup google.com                  # DNS test

# IPv4 static
netsh interface ip set address "Ethernet" static 192.168.1.10 255.255.255.0 192.168.1.1
netsh interface ip set dns "Ethernet" static 8.8.8.8

# IPv4 DHCP
netsh interface ip set address "Ethernet" dhcp
netsh interface ip set dns "Ethernet" dhcp

# IPv6 static
netsh interface ipv6 add address "Ethernet" 2001:db8:1:1::10/64
netsh interface ipv6 add route ::/0 "Ethernet" 2001:db8:1:1::1
netsh interface ipv6 add dnsserver "Ethernet" 2001:4860:4860::8888

# IPv6 autoconf
netsh interface ipv6 set interface "Ethernet" routerdiscovery=enabled


#######################################################################
# MACOS
#######################################################################
ifconfig                              # interfaces, IPv4/IPv6
ipconfig getifaddr en0                # IPv4 only
ipconfig getpacket en0                # DHCP info
scutil --dns                          # DNS config
netstat -nr                           # routing table
arp -a                                # ARP
ndp -a                                # IPv6 ND
ping 8.8.8.8                          # IPv4 test
ping6 2001:4860:4860::8888            # IPv6 test
dig google.com                        # DNS test

# IPv4 static
networksetup -setmanual "Wi-Fi" 192.168.1.10 255.255.255.0 192.168.1.1
networksetup -setdnsservers "Wi-Fi" 8.8.8.8 1.1.1.1

# IPv4 DHCP
networksetup -setdhcp "Wi-Fi"

# IPv6 static
networksetup -setv6manual "Wi-Fi" 2001:db8:1:1::10 64 2001:db8:1:1::1

# IPv6 autoconfig
networksetup -setv6automatic "Wi-Fi"
networksetup -setv6networkserviceenabled "Wi-Fi" on


#######################################################################
# LINUX (iproute2 + NetworkManager)
#######################################################################
ip a                                  # IPv4/IPv6
ip -4 a                               # IPv4 only
ip -6 a                               # IPv6 only
ip route                              # IPv4 route
ip -6 route                           # IPv6 route
ip neigh                              # ARP/ND
resolvectl status                     # DNS (systemd)
ping 8.8.8.8                          # IPv4 test
ping6 2001:4860:4860::8888            # IPv6 test
dig google.com                        # DNS test

# IPv4 static (runtime)
sudo ip addr add 192.168.1.10/24 dev eth0
sudo ip route add default via 192.168.1.1

# IPv6 static (runtime)
sudo ip -6 addr add 2001:db8:1:1::10/64 dev eth0
sudo ip -6 route add default via 2001:db8:1:1::1

# NetworkManager (persistent)
nmcli connection modify "Wired connection 1" ipv4.addresses 192.168.1.10/24
nmcli connection modify "Wired connection 1" ipv4.gateway 192.168.1.1
nmcli connection modify "Wired connection 1" ipv4.method manual
nmcli connection modify "Wired connection 1" ipv4.method auto       # DHCP
nmcli connection modify "Wired connection 1" ipv6.method manual
nmcli connection modify "Wired connection 1" ipv6.method auto       # SLAAC
nmcli connection modify "Wired connection 1" ipv4.dns "8.8.8.8 1.1.1.1"

# Apply NM settings
nmcli connection down "Wired connection 1"
nmcli connection up "Wired connection 1"

# DNS (systemd-resolved)
sudo resolvectl dns eth0 8.8.8.8 1.1.1.1

#######################################################################
# END OF SUPER CHEAT SHEET
#######################################################################

```






## **Section 0 – Scope & Purpose**

This document provides a complete, exam-relevant and engineer-level reference for verifying and configuring IPv4 and IPv6 parameters on client operating systems (Windows, macOS, Linux).  
Focus is on CLI tools, as required in CCNA troubleshooting workflows.

Covered for each OS:

- Verification of IPv4 & IPv6 address, mask/prefix, gateway, DNS
    
- Routing table, ARP (IPv4), ND (IPv6)
    
- DHCP, DHCPv6, SLAAC
    
- Basic interface configuration (static, dynamic)
    
- DNS configuration
    
- Tools and usage patterns
    
- Unified cheat sheet (single code block per OS)
    

---

## **Section 1 – Fundamentals**

### 1.1 Required IP Parameters to Verify

- IPv4 address & subnet mask
    
- IPv6 link-local, global, ULA, temporary addresses
    
- Default gateway (v4 + v6)
    
- DNS servers (local, DHCP-derived, static)
    
- DHCP lease (v4), DHCPv6 status
    
- IPv6 SLAAC / RA information
    
- ARP table (IPv4)
    
- Neighbor Discovery table (IPv6)
    
- Routing table
    
- Interface link status, MAC address
    

### 1.2 General Verification Workflow (OS-Independent)

1. **Check link status** (interface up/down)
    
2. **Check addressing** (IPv4/IPv6)
    
3. **Check gateway reachability**
    
4. **Check routing table**
    
5. **Check DNS resolution**
    
6. **Check ARP/ND**
    
7. **Check DHCP/DHCPv6/SLAAC**
    
8. **Check local firewall restrictions** (if relevant)
    

---

# **Section 2 – Windows (Verification + Configuration)**

The entire Windows cheat sheet is consolidated below.

---

## **2.X – Windows Unified Cheat Sheet**

```less
#######################################################################
# WINDOWS - COMPLETE IPv4/IPv6 VERIFICATION & CONFIGURATION SUITE
# Single engineer-format block for CCNA troubleshooting & labs
#######################################################################

#######################################################################
# 1. SHOW / VERIFY IP PARAMETERS
#######################################################################

# Show all IPv4 and IPv6 configuration (basic)
ipconfig

# Show detailed info including DNS, DHCP, IPv6 SLAAC, lease info
ipconfig /all

# Show DHCP renew (IPv4)
ipconfig /release
ipconfig /renew

# Flush DNS cache
ipconfig /flushdns

# Display DNS client records
ipconfig /displaydns


#######################################################################
# 2. POWERFUL VERIFICATION (POWERSHELL COMMANDS)
#######################################################################

# Show IP configuration (interface name, IPv4, IPv6, gateway, DNS)
Get-NetIPConfiguration

# Show only IPv4 or IPv6 addresses
Get-NetIPAddress

# Show DNS server list
Get-DnsClientServerAddress

# Show routing table
Get-NetRoute

# Show IPv6 neighbors (ND)
Get-NetNeighbor

# Show interface operational status
Get-NetAdapter

# Show adapter advanced settings
Get-NetAdapterAdvancedProperty


#######################################################################
# 3. CLASSIC VERIFICATION TOOLS
#######################################################################

# ARP table (IPv4 only)
arp -a

# Routing table (IPv4 + IPv6)
route print

# Ping IPv4
ping 8.8.8.8

# Ping IPv6
ping -6 2001:4860:4860::8888

# Trace route IPv4
tracert 8.8.8.8

# Trace route IPv6
tracert -6 2001:4860:4860::8888


#######################################################################
# 4. CONFIGURE IPv4 ADDRESSING (STATIC)
#######################################################################

# Set static IPv4 address, mask, gateway
# netsh interface ip set address "<InterfaceName>" static <IPv4> <Mask> <Gateway>
netsh interface ip set address "Ethernet" static 192.168.1.10 255.255.255.0 192.168.1.1

# Set DNS server (primary)
netsh interface ip set dns "Ethernet" static 8.8.8.8

# Add secondary DNS server
netsh interface ip add dns "Ethernet" 1.1.1.1 index=2


#######################################################################
# 5. CONFIGURE IPv4 (DHCP)
#######################################################################

# Enable DHCP for IPv4
netsh interface ip set address "Ethernet" dhcp

# Set DNS from DHCP
netsh interface ip set dns "Ethernet" dhcp


#######################################################################
# 6. CONFIGURE IPv6 ADDRESSING (STATIC)
#######################################################################

# Set static IPv6 address
# netsh interface ipv6 add address "<InterfaceName>" <IPv6>/<PrefixLength>
netsh interface ipv6 add address "Ethernet" 2001:db8:1:1::10/64

# Set IPv6 default gateway
netsh interface ipv6 add route ::/0 "Ethernet" 2001:db8:1:1::1

# Add IPv6 DNS
netsh interface ipv6 add dnsserver "Ethernet" 2001:4860:4860::8888 index=1


#######################################################################
# 7. CONFIGURE IPv6 (AUTOCONFIG / SLAAC / DHCPv6)
#######################################################################

# Enable IPv6 autoconfiguration (SLAAC)
netsh interface ipv6 set interface "Ethernet" routerdiscovery=enabled

# Disable IPv6 autoconfiguration
netsh interface ipv6 set interface "Ethernet" routerdiscovery=disabled

# Show IPv6 RA & autoconfig state
netsh interface ipv6 show interface "Ethernet"


#######################################################################
# 8. INTERFACE CONTROL
#######################################################################

# Disable interface
Disable-NetAdapter -Name "Ethernet" -Confirm:$false

# Enable interface
Enable-NetAdapter -Name "Ethernet"

# Reset all network settings (dangerous but useful)
netsh int ip reset
netsh winsock reset


#######################################################################
# 9. CONNECTIVITY TROUBLESHOOTING
#######################################################################

# Test DNS name resolution
nslookup www.google.com

# Test gateway reachability
ping 192.168.1.1

# Verify IPv6 neighbor discovery
netsh interface ipv6 show neighbors

# Verify RA messages
netsh interface ipv6 show interfaces

#######################################################################
# END OF WINDOWS SUITE
#######################################################################
```


##  3.X – macOS Unified Cheat Sheet

```less
#######################################################################
# MACOS - COMPLETE IPv4/IPv6 VERIFICATION & CONFIGURATION SUITE
# All commands consolidated for CCNA troubleshooting and labs.
# macOS uses BSD networking stack; tools differ from Linux/Windows.
#######################################################################

#######################################################################
# 1. SHOW / VERIFY IP PARAMETERS
#######################################################################

# Show all interface parameters (IPv4 + IPv6 + MAC + flags)
ifconfig

# Show IPv4 addresses only
ipconfig getifaddr en0         # replace en0 with your NIC (Ethernet/Wi-Fi)

# Show IPv6 addresses only
ifconfig en0 | grep inet6

# Show detailed IPv4 configuration (DHCP, gateway)
ipconfig getpacket en0         # shows DHCP lease, DNS from DHCP, router

# Show default gateway (IPv4)
route -n get default

# Show default gateway (IPv6)
route -n get -inet6 default

# Show routing table
netstat -nr

# Show DNS servers
scutil --dns

# Test DNS resolution
dig google.com
nslookup google.com

# Test connectivity IPv4
ping 8.8.8.8

# Test connectivity IPv6
ping6 2001:4860:4860::8888

# Trace route IPv4
traceroute 8.8.8.8

# Trace route IPv6
traceroute6 2001:4860:4860::8888


#######################################################################
# 2. LAYER 2 VERIFICATION
#######################################################################

# Show ARP table (IPv4)
arp -a

# Show IPv6 Neighbor Discovery table
ndp -a

# Show router advertisements received on interface
ndp -r


#######################################################################
# 3. NETWORKSETUP - HIGH-LEVEL CONFIGURATION TOOL
#######################################################################

# List all network services (names must be used precisely)
networksetup -listallnetworkservices

# Show IPv4 settings of a service
networksetup -getinfo "Wi-Fi"

# Show DNS servers
networksetup -getdnsservers "Wi-Fi"

# Show IPv6 configuration state
networksetup -getv6networkserviceenabled "Wi-Fi"


#######################################################################
# 4. CONFIGURE IPv4 ADDRESSING (STATIC)
#######################################################################
# Set static IPv4 address, mask, gateway
# networksetup -setmanual "<Service>" <IP> <Mask> <Gateway>

networksetup -setmanual "Wi-Fi" 192.168.1.10 255.255.255.0 192.168.1.1

# Configure DNS (replace existing)
networksetup -setdnsservers "Wi-Fi" 8.8.8.8 1.1.1.1

# Clear DNS configuration (use DHCP-provided DNS)
networksetup -setdnsservers "Wi-Fi" "Empty"


#######################################################################
# 5. CONFIGURE IPv4 (DHCP)
#######################################################################

# Enable DHCP for IPv4
networksetup -setdhcp "Wi-Fi"

# Renew DHCP lease (IPv4)
ipconfig set en0 DHCP


#######################################################################
# 6. CONFIGURE IPv6 ADDRESSING (STATIC)
#######################################################################

# Enable IPv6
networksetup -setv6networkserviceenabled "Wi-Fi" on

# Set static IPv6 address
# networksetup -setv6manual "<Service>" <IPv6> <PrefixLength> <Gateway>
networksetup -setv6manual "Wi-Fi" 2001:db8:1:1::10 64 2001:db8:1:1::1

# Remove static IPv6 DNS
networksetup -setdnsservers "Wi-Fi" "Empty"


#######################################################################
# 7. CONFIGURE IPv6 (AUTOCONFIG/SLAAC)
#######################################################################

# Enable IPv6 automatically (SLAAC)
networksetup -setv6automatic "Wi-Fi"

# Disable IPv6 entirely (useful for troubleshooting)
networksetup -setv6networkserviceenabled "Wi-Fi" off


#######################################################################
# 8. ADVANCED VERIFICATION
#######################################################################

# Show all network services + hardware ports
networksetup -listallhardwareports

# Show current location profile
scutil --nc list

# Show system resolver state (DNS, search domains, order)
scutil --dns

# Show live network statistics (per interface)
netstat -ib

# Show open connections and listening ports
netstat -an

# Flush DNS cache (varies by macOS version)
sudo dscacheutil -flushcache; sudo killall -HUP mDNSResponder

#######################################################################
# END OF MACOS SUITE
#######################################################################
```


# **Section 4 – Linux (Verification + Configuration)**

Engineer-format, unified, **all commands in ONE block**, covering:

- iproute2 (modern standard)
    
- ifconfig (legacy)
    
- nmcli + nmtui (NetworkManager distros: Ubuntu, Fedora, CentOS Stream, RHEL)
    
- systemd-networkd verification (Ubuntu Server, Debian, minimalist distros)
    
- IPv4 + IPv6 verification and configuration
    
- DHCP, static, DNS, ND, ARP, routing, connectivity
    

---

# **4.X – Linux Unified Cheat Sheet (All Distros)**


```less
#######################################################################
# LINUX - COMPLETE IPv4/IPv6 VERIFICATION & CONFIGURATION SUITE
# iproute2 + NetworkManager + systemd-networkd
# All commands consolidated for CCNA troubleshooting and labs.
#######################################################################

#######################################################################
# 1. INTERFACE + IP VERIFICATION (MODERN: iproute2)
#######################################################################

# Show all interfaces + IPv4 + IPv6
ip address show
ip a

# Show IPv4 only
ip -4 a

# Show IPv6 only
ip -6 a

# Show link-layer/MAC status
ip link show
ip l

# Show interface statistics (RX/TX errors, drops)
ip -s link show

# Show default gateway (IPv4)
ip route show default
ip r | grep default

# Show default gateway (IPv6)
ip -6 route show default

# Show full routing tables
ip route
ip -6 route

# Show DNS (systemd-resolved systems)
resolvectl status

# Test DNS resolution
dig google.com
nslookup google.com

# Show ARP table (IPv4)
ip neigh
arp -a   # legacy

# Show IPv6 Neighbor Discovery table (ND)
ip -6 neigh
ip -6 neigh show

# Ping IPv4
ping 8.8.8.8

# Ping IPv6
ping6 2001:4860:4860::8888

# Trace route IPv4
traceroute 8.8.8.8

# Trace route IPv6
traceroute6 2001:4860:4860::8888


#######################################################################
# 2. CONFIGURE IPv4/IPv6 (RUNTIME, TEMPORARY) USING ip
# Changes last until reboot (or interface down/up)
#######################################################################

# Assign IPv4
# ip addr add <IP>/<Prefix> dev <IFACE>
sudo ip addr add 192.168.1.10/24 dev eth0

# Remove IPv4
sudo ip addr del 192.168.1.10/24 dev eth0

# Assign IPv6
sudo ip -6 addr add 2001:db8:1:1::10/64 dev eth0

# Remove IPv6
sudo ip -6 addr del 2001:db8:1:1::10/64 dev eth0

# Configure default gateway IPv4
sudo ip route add default via 192.168.1.1

# Configure default gateway IPv6
sudo ip -6 route add default via 2001:db8:1:1::1

# Flush all IPv4 addresses (useful for recovering)
sudo ip -4 addr flush dev eth0

# Flush all IPv6 addresses
sudo ip -6 addr flush dev eth0

# Bring interface up/down
sudo ip link set eth0 up
sudo ip link set eth0 down


#######################################################################
# 3. CONFIGURE DNS (RUNTIME + PERMANENT)
#######################################################################

# systemd-resolved (Ubuntu 20+, Debian 12+)
# Set DNS for interface
sudo resolvectl dns eth0 8.8.8.8 1.1.1.1

# Set search domain
sudo resolvectl domain eth0 example.com

# Traditional /etc/resolv.conf (non-systemd networks)
sudo nano /etc/resolv.conf
# nameserver 8.8.8.8
# nameserver 1.1.1.1


#######################################################################
# 4. NETWORKMANAGER (nmcli) – COMMON ON DESKTOP DISTROS
#######################################################################

# Show all connections
nmcli connection show

# Show devices
nmcli device status

# Show IPv4/IPv6 info on device
nmcli device show eth0

# Show DHCP leases
nmcli connection show <name> | grep DHCP

# Set IPv4 static
sudo nmcli connection modify "Wired connection 1" ipv4.addresses "192.168.1.10/24"
sudo nmcli connection modify "Wired connection 1" ipv4.gateway "192.168.1.1"
sudo nmcli connection modify "Wired connection 1" ipv4.method manual

# Set IPv4 DNS
sudo nmcli connection modify "Wired connection 1" ipv4.dns "8.8.8.8 1.1.1.1"

# Enable DHCP IPv4
sudo nmcli connection modify "Wired connection 1" ipv4.method auto

# Set IPv6 static
sudo nmcli connection modify "Wired connection 1" ipv6.addresses "2001:db8:1:1::10/64"
sudo nmcli connection modify "Wired connection 1" ipv6.gateway "2001:db8:1:1::1"
sudo nmcli connection modify "Wired connection 1" ipv6.method manual

# Enable IPv6 autoconf (SLAAC)
sudo nmcli connection modify "Wired connection 1" ipv6.method auto

# Enable/disable IPv6 on connection
sudo nmcli connection modify "Wired connection 1" ipv6.ip6-privacy 0   # disable temp addresses
sudo nmcli connection modify "Wired connection 1" ipv6.ip6-privacy 2   # enable privacy extensions

# Apply settings (disconnect + reconnect)
sudo nmcli connection down "Wired connection 1"
sudo nmcli connection up "Wired connection 1"


#######################################################################
# 5. NETWORKMANAGER – TEXT UI (nmtui)
#######################################################################

# Start graphical TUI for editing
sudo nmtui

# Options inside:
# - Edit a connection
# - Activate a connection
# - Set hostname


#######################################################################
# 6. SYSTEMD-NETWORKD VERIFICATION (SERVER DISTROS)
#######################################################################

# Check link state
networkctl status
networkctl status eth0

# Check IPs managed by systemd-networkd
networkctl list

# Check resolved DNS
resolvectl dns
resolvectl status

# Show networkd logs
journalctl -u systemd-networkd

# Example config file (persistent):
# /etc/systemd/network/10-eth0.network
# [Match]
# Name=eth0
#
# [Network]
# Address=192.168.1.10/24
# Gateway=192.168.1.1
# DNS=8.8.8.8
# IPv6AcceptRA=yes

# Apply changes
sudo systemctl restart systemd-networkd


#######################################################################
# 7. LEGACY COMMANDS (ifconfig, route) - OLD BUT STILL ON MANY SYSTEMS
#######################################################################

# Show interfaces
ifconfig -a

# Bring interface up/down
sudo ifconfig eth0 up
sudo ifconfig eth0 down

# Assign IPv4 (legacy)
sudo ifconfig eth0 192.168.1.10 netmask 255.255.255.0

# Show routes (legacy)
route -n


#######################################################################
# 8. CONNECTIVITY TROUBLESHOOTING
#######################################################################

# Verify DNS
host google.com
dig google.com

# Verify gateway reachability (IPv4/IPv6)
ping 192.168.1.1
ping6 fe80::1%eth0   # link-local requires interface identifier

# Show Neighbor Discovery messages & router advertisements
sudo rdisc6 eth0       # if installed

# Flush ND cache
sudo ip -6 neigh flush all

# Flush ARP cache
sudo ip neigh flush all

#######################################################################
#
```



# **Section 5 – Cross-OS Comparison (Verification + Configuration)**

## **5.1 Verification Matrix**

|Function|Windows|macOS|Linux|
|---|---|---|---|
|Show IPv4/IPv6 addresses|`ipconfig /all`|`ifconfig`, `ipconfig getifaddr`, `scutil --dns`|`ip a`, `ip -4 a`, `ip -6 a`|
|Show default gateway|`route print`|`route -n get default`|`ip route`, `ip -6 route`|
|Show DNS|`ipconfig /all`, `Get-DnsClientServerAddress`|`scutil --dns`|`resolvectl status`, `/etc/resolv.conf`|
|Show routing table|`route print`|`netstat -nr`|`ip route`, `ip -6 route`, `netstat -nr`|
|ARP table|`arp -a`|`arp -a`|`ip neigh`, `arp -a`|
|IPv6 neighbor table|`netsh interface ipv6 show neighbors`|`ndp -a`|`ip -6 neigh`|
|Show DHCP lease|`ipconfig /all`|`ipconfig getpacket`|`nmcli`, logs, `resolvectl`|
|Show RA/SLAAC|`netsh interface ipv6 show interfaces`|`ndp -r`|`rdisc6`, `ip -6 a`|
|Test IPv4 connectivity|`ping`|`ping`|`ping`|
|Test IPv6 connectivity|`ping -6`|`ping6`|`ping6`|
|DNS troubleshooting|`nslookup`|`dig`, `nslookup`|`dig`, `nslookup`, `host`|

---

## **5.2 Configuration Matrix**

|Function|Windows|macOS|Linux|
|---|---|---|---|
|Configure static IPv4|`netsh interface ip set address`|`networksetup -setmanual`|`ip addr add`, `nmcli mod ipv4.method manual`|
|DHCP IPv4|`netsh interface ip set address dhcp`|`networksetup -setdhcp`|`nmcli ipv4.method auto`, DHCP client|
|Configure static IPv6|`netsh interface ipv6 add address`|`networksetup -setv6manual`|`ip -6 addr add`, `nmcli ipv6.method manual`|
|IPv6 autoconf|`routerdiscovery=enabled`|`networksetup -setv6automatic`|SLAAC auto-enabled, `nmcli ipv6.method auto`|
|Set DNS|`netsh interface ip/ipv6 add dnsserver`|`networksetup -setdnsservers`|`resolvectl dns`, `/etc/resolv.conf`, `nmcli ipv4.dns/ipv6.dns`|
|Routing (static)|`route add`|`route add`|`ip route add`, `ip -6 route add`|
|Enable/disable NIC|`Enable-NetAdapter` / `Disable-NetAdapter`|GUI or `ifconfig up/down`|`ip link set <if> up/down`|

---

# **Section 6 – Troubleshooting Flow (IPv4 & IPv6)**

## **6.1 IPv4 Troubleshooting Flow**

1. **Verify link status**
    
    - Link up? Cable? Wi-Fi associated?
        
    - Commands:
        
        - Windows: `Get-NetAdapter`
            
        - macOS: `ifconfig`
            
        - Linux: `ip link`
            
2. **Verify IPv4 addressing**
    
    - Is the address present?
        
    - Correct mask?
        
    - Windows: `ipconfig /all`
        
    - macOS: `ifconfig`, `ipconfig getifaddr`
        
    - Linux: `ip -4 a`
        
3. **Check default gateway**
    
    - Exists? Correct network?
        
    - Ping test:
        
        - `ping <gateway>`
            
4. **Check routing table**
    
    - Ensure default route present
        
    - Commands:
        
        - Windows: `route print`
            
        - macOS: `netstat -nr`
            
        - Linux: `ip route`
            
5. **Check ARP**
    
    - Does ARP resolve the gateway?
        
    - Windows/macOS: `arp -a`
        
    - Linux: `ip neigh`
        
6. **Check DNS**
    
    - Using correct DNS server?
        
    - Windows: `Get-DnsClientServerAddress`
        
    - macOS: `scutil --dns`
        
    - Linux: `resolvectl status`
        
7. **Check DHCP operation**
    
    - Lease obtained?
        
    - Windows: `ipconfig /all`
        
    - macOS: `ipconfig getpacket`
        
    - Linux: `nmcli device show`, logs
        

---

## **6.2 IPv6 Troubleshooting Flow**

1. **Check link-local address (mandatory)**
    
    - Should be present immediately (`fe80::/10`).
        
    - If missing → interface issue.
        
    - Windows: `ipconfig /all`
        
    - macOS: `ifconfig`
        
    - Linux: `ip -6 a`
        
2. **Check Router Advertisements (RA)**
    
    - Determines prefix, gateway, on-link info.
        
    - Tools:
        
        - Windows: `netsh interface ipv6 show interfaces`
            
        - macOS: `ndp -r`
            
        - Linux: `rdisc6 eth0`
            
3. **Check global IPv6 address**
    
    - SLAAC: address begins with advertised prefix
        
    - DHCPv6: address appears later
        
4. **Check IPv6 default gateway**
    
    - Commands:
        
        - Windows: `route print`
            
        - macOS: `route -n get -inet6 default`
            
        - Linux: `ip -6 route`
            
5. **Check Neighbor Discovery**
    
    - Windows: `netsh interface ipv6 show neighbors`
        
    - macOS: `ndp -a`
        
    - Linux: `ip -6 neigh`
        
6. **Check DNS64 / IPv6 DNS**
    
    - Verify IPv6 DNS server is set
        
    - Test with:
        
        - `dig AAAA google.com`
            
7. **Connectivity Tests**
    
    - Ping global IPv6:
        
        - `ping6 2001:4860:4860::8888`
            
    - Ping local link gateway:
        
        - Linux: `ping6 fe80::1%eth0`  
            (interface ID required for link-local)
            

---

## **6.3 Common Issues & Symptoms**

### **IPv4**

- **APIPA (169.254.x.x)** → DHCP failure
    
- **Wrong mask** → cannot reach gateway, only local hosts
    
- **Wrong default gateway** → internet unreachable
    
- **No ARP entry for gateway** → L2/L3 mismatch, gateway down
    
- **DNS unreachable** → hostnames fail, IP works
    

### **IPv6**

- **No link-local** → driver/interface failure
    
- **No global address** → RA not received or blocked
    
- **No IPv6 default route** → router does not advertise gateway
    
- **Temporary IPv6 addresses keep changing** → privacy extensions enabled
    
- **Cannot reach link-local gateway** → missing interface identifier (`%eth0`)



# **Section 7 – ONE-PAGE SUMMARY (Commands Only, with short #comments)**

Everything required for CCNA-level verification & configuration on **Windows, macOS, Linux**.  
**Single consolidated code block**, engineer format, minimal comments.

```less
#######################################################################
# ONE-PAGE SUMMARY – WINDOWS / MACOS / LINUX
# CCNA VERIFY & CONFIGURE IPv4/IPv6 PARAMETERS
#######################################################################

#######################################################################
# WINDOWS
#######################################################################

# ---- VERIFY ----
ipconfig /all                       # full IPv4/IPv6, DNS, DHCP
Get-NetIPConfiguration              # modern full view
Get-NetIPAddress                    # show all addresses
Get-DnsClientServerAddress          # show DNS
route print                         # routing table
arp -a                              # ARP table
netsh interface ipv6 show neighbors # IPv6 ND table

# ---- TEST ----
ping 8.8.8.8                        # test IPv4
ping -6 2001:4860:4860::8888        # test IPv6
nslookup google.com                 # test DNS

# ---- CONFIG IPv4 ----
netsh interface ip set address "Ethernet" static 192.168.1.10 255.255.255.0 192.168.1.1
netsh interface ip set address "Ethernet" dhcp
netsh interface ip set dns "Ethernet" static 8.8.8.8

# ---- CONFIG IPv6 ----
netsh interface ipv6 add address "Ethernet" 2001:db8:1:1::10/64
netsh interface ipv6 add route ::/0 "Ethernet" 2001:db8:1:1::1
netsh interface ipv6 add dnsserver "Ethernet" 2001:4860:4860::8888


#######################################################################
# MACOS
#######################################################################

# ---- VERIFY ----
ifconfig                            # IPv4/IPv6 + interface flags
ipconfig getifaddr en0              # IPv4 only
ipconfig getpacket en0              # DHCP info
scutil --dns                        # DNS settings
netstat -nr                         # routing table
arp -a                              # ARP
ndp -a                              # IPv6 ND

# ---- TEST ----
ping 8.8.8.8
ping6 2001:4860:4860::8888
dig google.com

# ---- CONFIG IPv4 ----
networksetup -setmanual "Wi-Fi" 192.168.1.10 255.255.255.0 192.168.1.1
networksetup -setdhcp "Wi-Fi"
networksetup -setdnsservers "Wi-Fi" 8.8.8.8 1.1.1.1

# ---- CONFIG IPv6 ----
networksetup -setv6manual "Wi-Fi" 2001:db8:1:1::10 64 2001:db8:1:1::1
networksetup -setv6automatic "Wi-Fi"
networksetup -setv6networkserviceenabled "Wi-Fi" on


#######################################################################
# LINUX (iproute2 + NetworkManager)
#######################################################################

# ---- VERIFY (iproute2) ----
ip a                                # show IPv4/IPv6
ip -4 a                             # IPv4 only
ip -6 a                             # IPv6 only
ip route                            # routing table
ip -6 route                         # IPv6 routing
ip neigh                            # ARP / ND table
resolvectl status                   # DNS (systemd-resolved)

# ---- TEST ----
ping 8.8.8.8
ping6 2001:4860:4860::8888
dig google.com

# ---- CONFIG IPv4 (runtime) ----
sudo ip addr add 192.168.1.10/24 dev eth0
sudo ip route add default via 192.168.1.1

# ---- CONFIG IPv6 (runtime) ----
sudo ip -6 addr add 2001:db8:1:1::10/64 dev eth0
sudo ip -6 route add default via 2001:db8:1:1::1

# ---- CONFIG via NetworkManager ----
nmcli connection modify "Wired connection 1" ipv4.addresses "192.168.1.10/24"
nmcli connection modify "Wired connection 1" ipv4.gateway "192.168.1.1"
nmcli connection modify "Wired connection 1" ipv4.method manual
nmcli connection modify "Wired connection 1" ipv4.method auto          # DHCP IPv4
nmcli connection modify "Wired connection 1" ipv6.method manual        # static IPv6
nmcli connection modify "Wired connection 1" ipv6.method auto          # SLAAC

# ---- DNS ----
sudo resolvectl dns eth0 8.8.8.8 1.1.1.1
sudo nano /etc/resolv.conf          # traditional systems

#######################################################################
# END OF ONE-PAGE SUMMARY
#######################################################################

```


# **Section 8 – MINI WIKI (Single Code Block, #comments only)**

Everything important for CCNA-level host-side IPv4/IPv6 understanding, formatted engineer-style.

```less
#######################################################################
# MINI WIKI – IPv4 / IPv6 PARAMETERS FOR CLIENT OS
# All explanations inside #comments. No outside text.
#######################################################################

#######################################################################
# IP FUNDAMENTALS
#######################################################################

# IPv4 address
# - 32-bit address written as dotted decimal (e.g., 192.168.1.10)
# - Assigned manually or via DHCP
# - Combined with subnet mask to determine network boundary

# Subnet Mask
# - Identifies network and host portion (e.g., /24 = 255.255.255.0)
# - Incorrect mask = loss of connectivity outside local LAN

# Default Gateway
# - Router address used to leave local network
# - Only one default gateway per interface per protocol family
# - Missing/wrong gateway = no internet

# DNS (Domain Name System)
# - Translates hostnames to IP addresses
# - DNS failures present as “no internet” even when IP works
# - Client queries → DNS server → recursive resolution

# DHCP (IPv4)
# - Automated IPv4 addressing: IP, mask, gateway, DNS
# - Lease parameters: T1 (renew), T2 (rebinding)
# - APIPA 169.254.x.x means DHCP failure


#######################################################################
# IPv6 FUNDAMENTALS
#######################################################################

# IPv6 Address Structure
# - 128-bit address written in hex (e.g., 2001:db8:1::10)
# - Multiple addresses per interface are normal
# - Types: Link-local, Global Unicast, ULA, Temporary, Multicast

# Link-Local Address
# - Every interface auto-creates one: fe80::/10
# - Required for Neighbor Discovery
# - Only valid on the local link (not routed)
# - Essential for pinging local router (gateway)

# Global Unicast Address (GUA)
# - Public IPv6 address, routable on the internet
# - Usually assigned by SLAAC or DHCPv6

# Unique Local Address (ULA)
# - fc00::/7 → private IPv6 equivalent of RFC1918 ranges
# - Local use, not routed on internet

# Temporary / Privacy Addresses
# - Generated for outbound connections
# - Enabled by default on many OSes (Windows, Linux)
# - Help prevent tracking

# Multicast Addresses
# - ff00::/8
# - Replaces broadcast in IPv6
# - Every host joins: ff02::1 (all-nodes), ff02::2 (routers)


#######################################################################
# IPv6 AUTOCONFIGURATION (SLAAC / RA / DHCPv6)
#######################################################################

# SLAAC (Stateless Address Autoconfiguration)
# - Router Advertisement (RA) delivers:
#   - Prefix (e.g., 2001:db8:1:1::/64)
#   - On-link determination
#   - Default gateway
# - Host generates:
#   - Global unicast address using interface ID (EUI-64 or random)
#   - Temporary addresses (optional)

# Router Advertisement (RA)
# - Sent from router → ff02::1 (all-nodes multicast)
# - Contains flags:
#   - A flag: Autoconfiguration allowed
#   - L flag: Prefix is on-link
#   - M flag: Managed by DHCPv6
#   - O flag: Other information via DHCPv6 (DNS)
# - No RAs = no IPv6 beyond link-local

# DHCPv6
# - Provides IPv6 address (stateful) or DNS only (stateless)
# - Works together with SLAAC when M=0 but O=1


#######################################################################
# NEIGHBOR DISCOVERY (ND) – IPv6 EQUIVALENT OF ARP
#######################################################################

# ND Components:
# - Neighbor Solicitation (NS)
# - Neighbor Advertisement (NA)
# - Router Solicitation (RS)
# - Router Advertisement (RA)
# - Redirect messages

# NS/NA
# - NS: "Who has this IPv6 address?"
# - NA: "I have it"
# - Uses solicited-node multicast → extremely efficient compared to ARP broadcast

# Router Solicitation (RS)
# - Host → ff02::2 to request RA immediately after link comes up

# ND Table
# - Same role as ARP table
# - Stores MAC-to-IPv6 mapping
# - Commands:
#   - Windows: netsh interface ipv6 show neighbors
#   - macOS: ndp -a
#   - Linux: ip -6 neigh


#######################################################################
# DUAL STACK OPERATION (IPv4 + IPv6)
#######################################################################

# Hosts typically prefer IPv6 over IPv4 if both available
# DNS returns AAAA records = IPv6
# If IPv6 default route exists but connectivity broken → long delays
# Troubleshooting priority:
# 1. Link-local
# 2. RA
# 3. Global IPv6
# 4. IPv6 routing
# 5. DNS (AAAA queries)


#######################################################################
# TROUBLESHOOTING PRINCIPLES (HOST-SIDE)
#######################################################################

# Always verify in this order:
# 1. Interface up? (physical/link-layer)
# 2. Address assigned? (IPv4/IPv6)
# 3. Gateway reachable? (ping gateway)
# 4. Routing? (default route exists?)
# 5. DNS? (resolve hostname)
# 6. ARP/ND? (MAC resolution)
# 7. DHCP/DHCPv6/SLAAC? (lease/RA/ND behavior)

# Common IPv4 issues:
# - APIPA address = DHCP failure
# - Wrong mask = cannot reach gateway
# - Wrong DNS = no hostname resolution

# Common IPv6 issues:
# - No fe80:: address = interface failure
# - No global address = no RA or DHCPv6
# - Link-local ping to gateway requires interface ID (e.g., fe80::1%eth0)
# - Temporary addresses cause changing IPv6 source

#######################################################################
# END OF MINI WIKI
#######################################################################

```
