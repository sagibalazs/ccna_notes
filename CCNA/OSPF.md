

## 

## Links and sources

- [networkacademy.io](https://www.networkacademy.io/)
- [study-ccna.com](https://study-ccna.com/)

### Connected Topics

- [[Cisco Bandwidth Command]] 
- [[Cisco Speed Command]]
- [[Cisco Clock Rate Command]]

## Routing Protocols Overview


![[Pasted image 20241122101516.png]]
## OSPF v2 - Open Shortest Path First

In this section i introduce the OSPFv2 Routing protocol. 

### OSPF - CLI (Cisco)

```bash
# ospf neigbohrs
R1#show ip ospf neighbor 
Neighbor ID Pri State Dead Time Address Interface
2.2.2.2 1 FULL/DR 00:00:30 192.168.0.2 FastEthernet0/0

# changing the ospf cost - use `sh ip ospf int`
conf t
int g0/0
ip ospf coast <1-65535>

# changing the ospf cost - use bandwith in kilobits

conf t
int g0/0
bandwidth <1-10000000>

# changing the ospf cost - change ospf reference bandwith 

conf t 
router ospf 1
auto-cost reference-bandwidth 100000

# ospf basic config - put all interface's ip address to the list which is assigned to the routers interfaces and will participate in the ospf

router ospf 1
network <ip addr> <wildcard mask> area <cr>
network <ip addr> <wildcard mask> area <cr>
network <ip addr> <wildcard mask> area <cr>

# by ARB (Area Border Router) - multie are config

router ospf 1
network <ip addr> <wildcard mask> area <0>
network <ip addr> <wildcard mask> area <A>
network <ip addr> <wildcard mask> area <A>
network <ip addr> <wildcard mask> area <B>
network <ip addr> <wildcard mask> area <B>




```

### OSPFv2 - Characteristic

- link-state routing protocol, belong to the IGP - operates within a single autonomous system (AS)
- classless routing protocol
- supports VLSM, [[CIDR]]
- incremental updates are supported
- only use interface cost as metric
- default administrative value is 110
- use multicast addresses 224.0.0.5 and 244.0.0.6 for routing updates
- load balancing
- *routing information from adjacent routers*
- *advertise routing information to adjacent routers*
- install the best route, if multiple choice available
- by topology changes, trigger a topology recalculation

### OSPF Cost - Metric

- OSPF is a link state protocol, that uses Link State Routing
- to represent the topology correctly, OSPF assign for each link a specified cost
- Here are the default Cost values for each interface type:

|                                     |     |
| ----------------------------------- | --- |
| Gigabit Ethernet Interface (1 Gbps) | 1   |
| Fast Ethernet Interface (100 Mbps)  | 1   |
| Ethernet Interface (10 Mbps)        | 10  |
| DS1 (1.544 Mbps)                    | 64  |
| DSL (768 Kbps)                      | 133 |



### OSPF Working Mechanism

- establish neighbor relationship
	- send Hello packets on each interface where OSPF is activated, using the multicast address 224.0.0.5
	- to become neighbors, the following fields in the Hello packets must be the same on both routers
		-  subnet
		- area id
		- hello and dead interval timers
		- authentication
		- area stub flag
		- MTU
	- default Hello interval 10 second, dead timer is default 40 second, after that neighbor will be declared to be down
- due of link state protocol, no routing tables are exchanged
	- they exchange information about network topology
- each router run SPF or Dijkstra algorithm to calculate best routes and those to the routing table
- each router knows the entire network topology, its nearby completely eliminate the change for a routing loop is occur
- use three table to store the topology information
	- **Neighbor table** – stores information about OSPF neighbors
	- **Topology table** – stores the topology structure of a network
	- **Routing table** –  stores the best routes
### OSPF Area

- we beginn always with area 0 - which is the backbone
- any other area must have contact/direct connection to this backbone area
- interfaces that

### OSPF Neighbor states

- before the neighbor relationship is established, the router must be go through these states
	**1. Init state** – a router has received a Hello message from the other OSPF router  
	**2. 2-way state** – the neighbor has received the Hello message and replied with a Hello message of his own  
	**3. Exstart state** – beginning of the Link State Database (LSDB) exchange between both routers. Routers are starting to exchange link state information.  
	**4. Exchange state** – DBD (Database Descriptor) packets are exchanged. DBDs contain LSAs headers. Routers will use this information to see what LSAs need to be exchanged.  
	**5. Loading state** – one neighbor sends LSRs (Link State Requests) for every network it doesn’t know about. The other neighbor replies with the LSUs (Link State Updates), which contain information about requested networks. After all the requested information have been received, other neighbor goes through the same process  **6. Full state** – both routers have the synchronized database and are fully adjacent to each other.

### OSPF - Link State Advertisements

- **LSA - Link State Advertisements**: used to exchange topology information, each of them contain routing and topology information to describe the part of the network
- **LSR - Link State Request**: used to request all missing entries from the neighbors, that LSA's not found in the database
- **LSU - Link State Update:** used to send all missing LSA entries to the neighbor that is requested
	- two router exchange their information, which is stored in a database, they send a list of all LSAs 
	- both check its topology database, then sen LSUs to request all missing LSAs
	- for last, each router send LSUs to update the database, containing all missing LSAs
	- 

### OSPF - Inteface Speed and Bandwidth, Clock Rate

#### Bandwidth command

- used to inform the higher level protocols about the bandwidth of an interface, eg. for routing protocols, QoS etc., used mostly if the actual physical media speed is slower then the interface where is connected
- used to influence higher level protocols
### OSPF DR vs. BDR

- **DR - Designated router**: receive and advertise the link states
- **BDR - Backup Designated Router**: if the designated router fails, it can be replace it immediately
- main goal, to reduce the network traffic by using one central router as a boss, who collect all LSAs, and advertise them to BROTHER routers and Backup Designated Router BDR
- 
### OSPF - Passive Interfaces

- interfaces where no additional router is connected which also running OSPF, using the `passive interface` command we can disable sending Hello packets, to reduce  the network traffic
- 

### OSPF - Terms

- **router-ID**: each router has a router ID, determined by:
	- set manual by the `router-id` command under the `router ospf` process
	- using the highest IP address of the router's loopback interfaces
	- using the highest IP address of the router's physical interfaces 
- **Hello packets**: used to .., to became
	- 
- **ARB - Area Border Router** - route which participate in more than 2 ospf area
- **
### OSPF Election Process

- first check the ospf priority, which is default 1
	- it can be changed between 1 and 255, highest number win
- second, its check the router ID, which is represented by
	- the highest IP address of the loopback interfaces
	- or the highest IP address of any active non loopback, physical interfaces

## OSPF - CLI Commands


```bash

Router# conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)# router ospf ?
  <1-65535>  Process ID
Router(config)# router ospf 1
Router(config-router)# end
Router#

```


![[Pasted image 20241128231542.png]]

## OSPF CLI

```bash
# 1ocal process id between 1 and 65535
# ospf is ospfv2 default, ospfv3 enables native support for IPv6
router ospf <process ID>
# 
network <ip_address_network> <wildcard_mask> <area [number]>
# like "network 10.101.10.0 0.0.0.255 area 0"


```

