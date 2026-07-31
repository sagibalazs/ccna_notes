

# IPv6 Addressing

## Disclaimer

## Sources and Links

- [networkacademy.io - IPv6](https://www.networkacademy.io/ccna/ipv6/introduction-to-ip-version-6)
- 



## Introduction  to IPv6 

> RFC1883

- larger 128 bit (32 bit hexadecimal) address space, allowing multi level subnetting and more efficient allocation from the regional internet providers
- end to end security, IPSec in built in, IPv6 has a header extension, that ease the implementation of encryption and authentication
- QoS
- stateless and statefull host addressing (SLAAC) - hosts can be addresses himself and using the network, if the dhcp failed
- simplier, more efficient header format >> non essential parts compared to IPv4 was removed, making it more efficient for intermediate routers
- extensibility: allow adding extensions headers after IPv6 header
- ARP and Broadcast replaced by ICMPv4 Neigborh Discovery, which use multicast messages instead of broadcast
- multiple IPv6 addresses per device on the same subnet, which allow better security, greater privacy, creates possibility for additional network features

> **Important, that all network features such as protocols and functions, that based on the IPv4 addressing, its not work under IPv6 due of the length of the addresses. Therefore, to migrate IPv6 in all layer, lead to rewrite all involved protocols and functions. **


## Representing IPv6 Addresses

- abbrevation



## ## IPv4 Header vs IPv6 Header

![[Pasted image 20251212225412.png]]


## IPv6 Header Format

- divided into two main Header, Main and Extensions Headers
- fewer fields make more efficient and faster to process it
- fixed 40 bit size


### Version Field

- in IPv4 is set to 4, sure by IPv6 its 6
![[Pasted image 20251212230726.png]]


## IPv6 Address Types



### Overview of IPv6 Types

![[Pasted image 20251208222328.png]]
![[Obsidian Notes/NE/CCNA/Untitled Diagram.svg]]


### Unicast Addresses

- general type of addresses, which are unique identifier for a single network interface
- allow one- to one communication between devices
- down under, we discuss the different types of unicast addresses

### Multicast Addresses

- network identifier for  a set of interfaces, belonging to a different IPv6 enabled nodes
- packet delivered for all interfaces, that are identified by that address
- one to many

### Anycast Addresses

- network layer identifier, for a set of interfaces, belonging to different IPv6 enabled nodes
- but unlike in multicast, the packets are delivered only for the closest destination in term of a routing metric

>  **No broadcast type in IPv6, since is replaced by using multicast addresses!!


### IPv6 Address Spaces

![[Pasted image 20251208223151.png]]


| **Group/Category**             | **Prefix/Range (in CIDR notation)**                                      | **Usage/Description**                                                                                                                                                                                                                         |
| ------------------------------ | ------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Global Unicast**             | `2000::/3` (i.e., `2000::` to `3FFF:FFFF:FFFF:FFFF:FFFF:FFFF:FFFF:FFFF`) | **Publicly Routable Addresses**. This is the primary block for the public internet, assigned by IANA/RIRs to ISPs and organizations.                                                                                                          |
| **Link-Local Unicast**         | `FE80::/10` (specifically `FE80::/64` is most common)                    | **Single-Link Communication**. Used for communication only on the local network segment. Devices use these for neighbor discovery, address autoconfiguration (SLAAC), and routing protocol messages. **Not routable** outside the local link. |
| **Unique Local Unicast (ULA)** | `FC00::/7` (specifically `FC00::/8` and `FD00::/8`)                      | **Private Addresses**. Analogous to IPv4's private ranges (like 10.0.0.0/8). Used for communications within a **set of private sites**. Designed to be unique globally but **not routable** on the public internet.                           |
| **Multicast**                  | `FF00::/8`                                                               | **One-to-Many Communication**. Used for sending a single packet to multiple destinations simultaneously. Includes well-known groups like All Nodes (`FF02::1`) and All Routers (`FF02::2`).                                                   |
| **Loopback**                   | `::1/128`                                                                | The equivalent of **127.0.0.1** in IPv4. Used by a host to send packets to itself for testing or local service access.                                                                                                                        |
| **Unspecified**                | `::/128`                                                                 | Represents **"no address"**. Used by a host during initialization or by a router to indicate the absence of a route.                                                                                                                          |
| **Embedded/Transition**        | `2001::/32` (Teredo), `2002::/16` (6to4), `::/96` (IPv4-compatible)      | Used for **transition mechanisms** to tunnel IPv6 traffic over IPv4 networks (e.g., **6to4, Teredo**). Many of these transition mechanisms are now deprecated or discouraged.                                                                 |
| **Reserved/Special**           | `0000::/8` (Excluding Loopback/Unspecified) and `FFFF::/8`               | Currently reserved for future use or special IETF functions.                                                                                                                                                                                  |




### Unicast Addresses
---

#### Global Unicast Addresses - Public IPv4

- same as the public IPv4 Addresses, they are used in the internet also public routable addresses
- unique

![[Pasted image 20251208222438.png]]

- aggregatable global unicast addresses are part of the global routing prefix
- structure of these addresses are enable for aggregation of routing entries to achieve a smaller global IPv6 routing table
- all global unicast address start with binary value 001 \(2000::/3\)
	- 48 bit global routing prefix
	- 16 subnet ID also refered Site-Level Aggregator (SLA)
	- 64 bit interface ID

![[Pasted image 20251208223913.png]]

#### Link Local Addresses - APIPA

- APIPA - local, not routable, auto-configured
- unique for each LAN
- used for temporary LAN, to still have file and resource sharing 
- link local prefix **FE80::/10** (first 10 bits equal to 1111 1110 10)

![[Pasted image 20251208224253.png]]

#### Loopback 

- same as by IPv4, no physical representation
- always up and running
- packages are returned "looped" to the source interface to testing the TCP/IP networking stack
- **0:0:0:0:0:0:0:1/128 is reserved for loopback identifier**
- **shortened to ::1/128**

#### Unspecified 

- in both IP protocol stack, the address with all binary to set 0
- IPv4 its 0.0.0.0/32
- IPv6 its 0:0:0:0:0:0:0:0 or shortened as ::/128
- routers do not forward packets with a source or destination address set to unspecified address.


#### Unique Local Addresses - Private IP Range

![[Pasted image 20251208225107.png]]


- same as IPv4 private addresses, its used on local network and routable in local network only
- globally unique prefix, will no conflict with other IPv6 global prefixes, no overlapping
- threated as regular global IPv6 addresses
- Internet Service Provider independent address space
- nearly globally unique, designed to replace site-local addresses 
- allow routing on multiple local networks

#### Embedded IPv4 and IPv6

![[Pasted image 20251208231041.png]]

- unicast address which **has only zeros in the first 96 bit**
- the rightmost **32 bit contain the IPv4 address**
- IPv4 embedded in hex digits like A.B.C.D >> A:B:C:D 
- its used in automatic tunnels, supporting both IP protocol stack
-

### Multicast Addresses
---
#### Multicast - FF

-  Technic used to send one to many
- destination is a set of interfaces, identified by one single multicast address, know as multicast group
- one to many addresses, **starting always with FF, prefix ff00::/8** (which is equivalent to the IPv4 multicast address space of 224.0.0.0/4)
- therefore the **leftmost bits are all set of 11111111**
- **There aren't broadcast addresses in IPv6.**
- 

### Anycast - one to nearest

- like multicast, anycast addresses identify multiple interfaces, but with a big difference
- anycast packet is delivered only one address, to the one which is based on the routing distance, the next nearest 
- special address, because we can apply this to multiple interface
- one to nearest
##### Well-know  Multicast Addresses

| Address   | Function                  |
|-----------|---------------------------|
| FF02::1   | All Nodes Address         |
| FF02::2   | All Routers Address       |
| FF02::4   | DVMRP Routers             |
| FF02::5   | All OSPFv3 routers        |
| FF02::6   | OSPFv3 Designated Routers |
| FF02::a   | All EIGRP (IPv6) routers  |
| FF02::D   | All PIM Routers           |
| FF02::12  | VRRP                      |
| FF02::16  | All MLDv2-capable routers |

#### Solicited-node Multicast Address

![[Pasted image 20251208233208.png]]

- generated automatically using an IPv6 unicast of an interface
- solicited-node multicast address is generated automatically based on the unicast address for this interface and the node joins the multicast group
- any unicast address has a corresponding solicited-node multicast address
- used for address resolution, neighbor discovery, and duplicate address detection
- As shown in figure 7, a solicited-node multicast address consists of the fixed prefix FF02::1:FF00:0/104 and the last 24 bits of the corresponding IPv6 address.


## Special IPv6 Addresses

| Address               | Meaning                                                                                                                                        |
| --------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| 0:0:0:0:0:0:0:0       | Equals `::`. This is the equivalent of IPv4’s `0.0.0.0` and is typically the source address of a host before it receives an IP address (DHCP). |
| 0:0:0:0:0:0:0:1       | Equals `::1`. The equivalent of IPv4 `127.0.0.1` (loopback).                                                                                   |
| 0::FFFF:192.168.100.1 | IPv4-mapped IPv6 address — shows how an IPv4 address is written in a mixed IPv6/IPv4 environment.                                              |
| 2000::/3              | The global unicast address range allocated for Internet access.                                                                                |
| FC00::/7              | The unique local unicast range (private IPv6).                                                                                                 |
| FE80::/10             | The link-local unicast range.                                                                                                                  |
| FF00::/8              | The multicast range.                                                                                                                           |
| 3FFF:FFFF::/32        | Reserved for examples and documentation.                                                                                                       |
| 2001:0DB8::/32        | Reserved for documentation (RFC 3849).                                                                                                         |
| 2002::/16             | Used with 6to4 tunneling (IPv4-to-IPv6 automatic transition mechanism).                                                                        |

## Stateless Address Autoconfiguration (SLAAC)

- this feature allow devices to address themselves with **local-link** or unicast address as well as with a **global unicast** address. 
- happens through the first learning the prefix information  form the router
- appending the device's own interfaces address as the interface ID
- MAC address used als interface ID
- but MAC is 48 bit, but interface ID is 64 bit long
- the remaining 16 bit has FF:FE value, which is placed in the middle of the MAC address
	- 0060:d673:1987 before padding
	-  0260:d6FF:FE73:1987 after padding
- so the device address is `prefix_information_from_router` + `device_id(padded_mac`
- fore futher information please visit or search for #EUI-64 rules


![[Pasted image 20251208152016.png]]

- the `02` at the begining of the address, which is determine if the address is Universal or Local (U/L) locally unique or globally unique
- its called modified EUI-64 format, using the 7th bit of the address
	- if the bit set to 1:
		- the address is generated from the MAC address
		- its become longer and less user friendly
		- its means, the address is global unique, routable
	- if the bit set to 0:
		- the address is generated by user manually, without using the MAC address
		- its become shorter, more user friendly
		- but only local valid, not routable

| Original Address (hex)               | Octet Before Flipping | Octet After Flipping | New Address (hex)                  |
|--------------------------------------|------------------------|----------------------|-----------------------------------|
| 00:1A:2B:3C:4D:5E (MAC)             | 02 (binary 00000010)  | 03 (binary 00000011)| 02:1A:2B:3C:4D:5E → 03:1A:2B:3C:4D:5E |
| 00:1A:2B:3C:4D:5E (MAC)             | 02                     | 03                   | 00:1A:2B:3C:4D:5E → 00:1A:2B:3C:4D:5E (flipped U/L bit in octet 7) |
| Example address: 2001:0db8::/64    | Interface ID: 02:00:5E:00:53:00 | Flipped: 03:00:5E:00:53:00 | 2001:0db8::03:00:5E:00:53:00   |




### DHCP Stateful

- using it based on the SLAAC isnt necessery, but
- for take the control over the network in centralized way its possible
-
#### Reasons to use the DHCPv6

1. **Centralized address management**: Assign and manage IP addresses from a central location.
2. **Network segmentation (VLANs)**: Assign addresses based on VLANs or segments to ensure isolation and security.
3. **Access control and security**: Enforce policies, assign specific DNS servers, gateways, or other network parameters.
4. **Ease of management**: Simplify device onboarding, reduce administrative tasks, and minimize errors.
5. **Address pool management**: Allocate and deallocate addresses from a pool to manage address availability and conservation.
6. **Client configuration**: Provide clients with specific configuration parameters, such as DNS servers, gateways, or NTP servers.
7. **Quality of Service (QoS)**: Assign addresses based on QoS policies to prioritize traffic.
8. **Mobility support**: Enable devices to move between networks and obtain new addresses without manual configuration.
9. **Stateful and stateless operation**: Operate in either stateful or stateless mode, depending on the client's needs.
10. **Scalability**: Support large numbers of clients and address assignments.

These reasons highlight the key benefits of using DHCPv6 for managing IP address assignments in IPv6 networks.


### Migrating IPv6 - Strategies

- [[#Dual Stacking]] - its allow to use both addressing simultaneously, if they implemented 
- [[#6to4 Tunneling]] - used by IPv6 network, that are using IPv4 network to reach another IPv6 network


#### Dual Stacking

- its allow us to use both addressing to communicate
- let us upgrade the devices one by one
- during the process, that more and more device has the IPv6 address, its become more and more used over the IPv4 stack
- once all devices are upgraded, the old IPv4 can be removed

####  6to4 Tunneling

- crepy, but sometimes we dont have the control over the whole network, eg. the WAN using still IPv4
- at certain point the IPv6 header will be changed to IPv4

![[Pasted image 20251208172937.png]]

- dual stack router needed to implement it
- one side is 6to4 on the opposite is 6to4 
- if the 6to4 tunnel reach a NAT translation point, it will brake tunnel encapsulation
	- many other kind of protocol already migrated in NAT, but a lot of instance has an older version, which means trouble
- the only solution to pass this nowadays, is  using Teredo, which pack our traffic in UDP packets, which can survive the NAT
- #Miredo is a tunneling technique, which is used on the native IPv6 Linux and BSD Unix machines, to communicate over IPv4 directly without a dual-stack router or 6to4 tunnel
- 
