# 1.0 Preface

Notes based on the CompTIA Network + Study Guide Exam N10-008. 5^th Edition
Todd Lammle - ISBN 

## IP Terminologie

bit
byte
octet
network address
ip address
broadcast address
## Hierarchical IP Addressing Scheme

- 32 bit address is know as a structured or hierarchical address as opposed to a flat or not hierarchical address
- flat scheme not used due of the disadvantage which related to routing, if all address were unique, all routers on the internet would learn and store the address of each and every machine of the internet, so there is no way to do a efficient routing
- 2 or 3 level hierarchical scheme is used
	- first section: area code to designate a very large area
	- second section: prefix, which is narrows the scope to local calling area
	- third section: final segment, customer number, zooms in the specific connection

## 1.3 Network Addressing





## Address Classes

For efficient network routing, internet designers assigned special rules for each network class. Each network class has mandate for the leading-bit section. 

**Class A**
First byte assigned to the network, three remaining to the host addresses. *network.host.host.host* eg. *10.0.0.1 /24*
Always starting with a binary 0, so the router know this packet should be speed up, just after read the first bit of the address. 
Class A 1 byte long, first bit reserved and 7 remaining available to for manipulating or addressing. 

| Class       | Public IP Range            | Private IP Range                | Subnet Mask   | # of Networks | # Hosts per Network |
| ----------- | -------------------------- | ------------------------------- | ------------- | ------------- | ------------------- |
| Class A 0   | 1.0.0.0 to 127.0.0.0       | 10.0.0.0 to 10.255.255.255      | 255.0.0.0     | 126           | 16777214            |
| Class B 10  | 128.0.0.0 to 191.255.0.0   | 172.16.0.0 to 172.31.255.255    | 255.255.0.0   | 16382         | 65534               |
| Class C 110 | 192.0.0.0 to 223.255.255.0 | 192.168.0.0. to 192.168.255.255 | 255.255.255.0 | 2097150       | 254                 |
*source [Meridianoutpost](https://www.meridianoutpost.com/resources/articles/IP-classes.php)


### Notation Types

#### CIDR - 172.10.1.1/24

Classless Inter-Domain Routing, also referred as VLSM, - Variable Length Subnet Mask

#### DDN - 2.2.2.2

- dotted decimal notation



## Host Addressing

## Private IP Addressing RFC 1918

| Class       | Private IP Range                | Subnet Mask |     |
| ----------- | ------------------------------- | ----------- | --- |
| Class A 0   | 10.0.0.0 to 10.255.255.255      | 255.0.0.0   | /8  |
| Class B 10  | 172.16.0.0 to 172.31.255.255    | 255.240.0.0 | /12 |
| Class C 110 | 192.168.0.0. to 192.168.255.255 | 255.255.0.0 | /16 |

### Static

### APIPA - Automatic Private IP Addressing  

- Range 169.254.0.1 to 169.254.255.254 Class B (255.255.0.0)

### Dynamic Addressing - DHCP


## IPv4 Addressing


### Broadcast types

#### L2 Broadcast


#### L3 Broadcast




