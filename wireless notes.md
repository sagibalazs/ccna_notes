d

### Links and Sources

- CCNA 200-301 Official Cert Guide Vol1.
- 


### Wireless LAN Topologies

- ![[Pasted image 20251222231810.png]]

- ![[Pasted image 20251222231840.png]]

- ![[Pasted image 20251222231918.png]]

- #### BSS - Basic Service Set

#### Distribution System


#### ESS - Extended Service Set


#### ISS - Independent Service Set

### Other Wireless Topologies

#### Repeater

#### Workgroup Bridges

#### Outdoor Bridge

#### Mesh Network

### RF Overview

#### Wireless Bands and Channels

#### APs and Wireless Standards

## Wireless Architectures

### Autonomous AP

- in a 3 layer network architecture (CDA)

### Cloud Based Architecture



### Split Mac - control and provisioning wireless access points

- CAPWAP separate tunnels
	- controll messages UDP 5246, encrypted and encapsulated, secure
	- data UDP 5247: default not encrypted, but it can be with Datagram Transport Layer
Security
- UDP port
- CAPWAP defined in RFCs 5415, 5416, 5417, 5418, based on cisco propertiary Lightweight Access Point Protocol (LWAPP)

- X.509 certificate to authenticate both side each other on network, make sure that no unauthorized AP on the network
- after capwap tunnels ready, wlc beginn to offering some more services
	- dynamic channel assigment
	- transmit power optimization (how to controll manualy?)
	- Self-healing wireless coverage
	- flexible client roaming
	- dynamic client load balancing
	- security management, authenticate clients from AAA, trusted DHCP to access wlan
	- Wifi IPS and IDS


#### Wireless LAN Controller WLC


#### CAPWAP Tunnels - Control and Provisioning of Wireless Access Points

![[Pasted image 20251222233235.png]]

- Control Tunnel use the UDP 5246, encrypted 
- Data Tunnel use the UDP 5427, packets are not encrypted but still protected with**Datagram Transport Layer Security** #DLTS to secure wireless connectivity for wireless users



### Comparing WLC Deployment 

- where? multiple options
- unified- centralized (3L CDA) - to rich much LAP as possible
	- to enforce security and 
	- up to 6000 LAPs
	- scale with add more physical WLC
- cloud based deployment
	-  as virtual machine rather than physical device
	- up to 3000 LAPs
	- scale with add more virtual WLCs
- embedeed deployment:
	- can be co-located with the distributions layer switches
	- support up to 200 APs
	- AP do not necessery connected to the wlc-host swithces
- cisco mobility express WLC deployment:
	- Host AP that forms WLC and CAPWAP tunnel, located alongside the other APs.
	- up to 100 APs

>  **See the summary table in CCNA book**


#### Cisco AP Modes


- local: default, one or more BSS on specific channel
    - by standby, scan other channels to measure noise- interference, discover rouge devices, and match aganist IDS events
- monitor: do not transmitt att all
    - act as a dedicated sensor
    - checks for ids systems
    - detect rouge APs
    - determines the position of stations through location based services
- flex content
    - AP at remote site, can locally switch traffic between an SSID and a VLAN if its CAPWAP tunnel to the WLC is down and its configured to do so.
- sniffer: receive with his radios all 802.11 traffic form other devices
    - like a sniffer or packet capture device
    - packet forwarded to PC, analyzer softwares like Wildpacket OmniPeek or WireShark where can proccess the data later
- rouge decector
    - detecting devices MAC address in both, wired and wireless network an compare them
    - rouge devices appear on both sides
    
- bridge
    - Poin to Point ot Point to Multipont between two network
    - two AP in bridge connect two locations which are separated by a long distance
    - multiple AP can bild an indoor or outdoor Mesh network

>**Default, LAP is running on Local mode and providing BSS to the devices. In any other mode, the BSS is disabled.**

## Securing Wireless Networks

There is 3 main bricket in the wireless security:

- encryption methods
- authentication methods
- message integrity methods

Important links:
- [Wi-Fi Alliance](https://wi-fi.org)

Topics to cover here or separate. 
Network fundamentals - wireless principles - encryption. 
Security Fundamentals

### Wireless security approach

- identifying the endpoints of a wireless connection
- identifying the end user
    - performed in a various way
- protecting the wireless data from eavesdroppers
- protecting the wireless data from tampering
    - WEP, PSK, TKIP, MIC, AES, EAP, EAP-FAST, EAP-TLS, LEAP, PEAP, WPA,
WPA2, WPA3, CCMP, GCMP, a



### Anatomy of secure connection

- secure connection >> reasons >> attackers

### Authentication

- discove BSS
- request permission to associate with it
- fake AP MIM - Man in the Middle
	- authenticate the APs before the user authenticate itself
		inklusive all management frames, one by one
	- to archieve this, the communication between the devices must be encrypted
- one network, one kind of encryption 
- but one seaparate key for each client
- group key for broadcast messages

### Message Integrity Check - MIC

- needs because if somebody modify the message, we will not recognize it. 
- message will be marked with special code name MIC, which calculated from the message
- original data >> compute MIC
- encrypt data + mic
- transport #hash to AP
- decrypt `<message>` +mic; compute mic > mic compare mic = mic

### Wireless authentication methods

- open authetication - "free wifi"

#### WEP - Wired Equivalent Privacy - shared key security method

- 40 or 128 bit long represented by 10 or 26 hex digits
- longer key doesnt mean better security, since the weaknesses are alredy leaked, in 2004 deprecated, do not use
- CR4 cipher algorithm
- same key for client and AP
- wep key used als authentication and message encryption
- key must be shared between clients and AP ahead of time
	- so that any client can associate with the APs
- AP test clients knowdledge of the WEP key, sending random challenge phrases
- the client encrypt the messages, and send back to the AP to compare their encryption
        
#### 802.1x/EAP - Extensible Authentication Protocol

- extensible, does not consist any authentication method
- instead define a set of functions that actual authentical method can use to authenticate users
- there is a many of them, but each follows the EAP Framework
- it can integrate with the IEEE 802.1x port based access control standard
- its limit access to the network but allowed to associate with an AP, but will not be able to pass data to any other part of the network
- on WEP, wireless client would be authenticated localy on the AP, but in 802.1x its changed- client uses open authentication ot associate with the AP, and the client authentication proccess occurs on the dedicated authentication server
	- supplicant: client device that requesting access
	- authenticator: network device that provides access to the network (usually WLC)
	- AS- Authentication Server: device that takes user or client credentials and permits or denies access based on an user database and policies (Radius, TACACS+)
	- WLC become a middleman in the process
        
#### LEAP - Lightweight EAP

- cisco propertiary method
- client provide name and password credentials
- authentication credentials are passed protected by passing protected access credential (PAC) between AS and supplicant...

> describe all the process
        
        
#### EAP-FAST - Flexible Authentication by Secure Tunneling

- inner and outer authentication
- authentication credentials are protected by passing a protected access credentials (PAC) between AS and supplicant
- PAC generated by AS and used for mutual authentication
	- pac generated or provisioned and installed on the client
	- after that suplicant and AS authenticated each other the negotiate a Transtport Layer Security TLS tunnel
	- end user authenticated throught the tls tunnel
- its consist of two separate authentication process and canal
- they occur in a nested fashion, inner and outer authentication
- radius server is required, which also operate as ans EAP-FAST server to able to generate PACs, one per user

			
#### PEAP - Protected EAP

- inner and outer authentication
- digital certificate in the outher channel from the AS, if suplicant accept, tls will occur
- certificate the AS consist of standard data format which is identifies itself, validated and signed by a third party service
- third party know as a certificate Authory CA, and know as trusted by both party
- certificate also used to pass public key in plain view, which can be used to help decrypt messages from the AS
- just the AS authenticate itself via certificate, supplicant not
- supplicant must be authenticated via:
	MSCHAPv2: Microsoft Challenge Authentication Protocol version 2
	GTC: Generic Token Card - hardware or device that generates one-time passwords for the user or a manually generatd password
			

#### EAP-TLS

- same as PEAP, but require certificate on AS and every client device.
- AS and supplicants authenticate each other with certificates
- after that, tls tunnel will be created to exchange the encryption key securely
- sonsidered as the most secure wireless authentication method, but implementing can be very challenging
- each client need certificate which is impractical
- instead, a Publik Key Infrastructure (PKI) that could supply certificates securely and efficiently and revoke them if the user do not have longer an access to the network
- its usually means, that the CA must be self owned / implemented, or a trust relationship with a third party CA required

- IMPORTANT, that many devices (communicators, medical devices, RFID tags) or their underlying OS cannot interface witha a CA or use certificates. 

### Wireless Privacy and ingrity methods

#### TKIP

- TKIP has a set of security features:
	- MIC - Message Integrity Check
	- Time Stamp: to prevent replay attacks to reuse or replay frames
	- Senders MAC Address: MIC includes senders MAC address
	- TKIP sequence counter: provides a record of frames sent by a unique MAC address, to prevent frames from being replayed as an attack
	- key mixing algorithm: computes 128bit WEP key for each frame
	- longer initialization  Vector IV: double size (48bit), making virtually impossible to exhaust all WEP keys by brute -force
	- after releasing the 802.11-2012 was deprecated
	
#### CCMP - CBC-MAC Protocol

- considered as securer als TKIP
- consist of 2 algorithms
	- AES (Advanced Encrytion Standard) counter mode encryption
	- Cipher Block Chaining Message Authentication Code (CBC-MAC) used to as a message integrity check (MIC)
- AES is the current standard, most secure encyption algorithm, which is adopted by U.S. National Institute of Standards and Technology (NIST)
- before using CCMP, client devices and AP must be support the AES counter mode and CBC-MAC in hardware.

#### GCMP - Galois/Counter Mode Protocol (GMPC)

- robust authenticated encryption more secure and sufficient as CCMP
- AES counter mode encryption
- Galois Messsage Authentication Code (GMAC) used as a message integrity check (MIC)
- GCMP used in WPA3

### WPA, WPA2, WPA3 - Wifi Protected Access

- Wi-Fi Alliance, a nonprofit wireless industry association has worked out the Wifi Protected Access WPA certifications
- each hardware would be tested aganist these standards and classyfied 
- each device in a specified class should be able to communicate with each other, becasue they use the same authentication, encryption, and message integrity algorithm

#### WPA

- based on parts the 802.11i 
- included 802.1x authentication
- TKIP
- method for dynamic encryption key management

#### WPA2

- once 802.1i was ratified and published, WPA2 was introduced
- based around the superior AES CCMP algorithm, instead of the deprecated TKIP

#### WPA3 - 2018

- encryption by AES with the Galois/Counter Mode Protocol (GCMP)
- protected management frames (PMF) to secure management frames between client and APs
	- so that help prevent malicious activity that might spoof or tamper with BSS operation
	
#### Compare table for WPAs

- table here





- notice that all WPA version support two client modes: 

> **NOTE**
> The personal modes of all WPA version is usally easy to deploy in small environment or client that are embededd in a certain devices, due of the simple teyt key string. Be aware using the same pre-shared key to all devices that using the wlan. If the key updated, all client devices must be updated. Pre-shared key must be kept secret.




##### PSK - Preshared Key - personal mode

- key string must be shared or configured on every client and AP to connect the wireless network
- preshared key in normally kept confidential, so unauthorized people users have no knowdledge of it
- keystring is never sent over the air
- instead of it, client and AP work through a four-way handshake procedure, which uses the pre-shared key to construct and exchange encryption key material that can  be openly exchanged
- once process is succesful, the AP can authenticate the client and the two can secure data frames that are sent over the air


- by WPA and WPA2-Personal modes the malicious user can eavesdrop and capture the 4 way handshake, so that captured data can be used to a dictionary attack to automate guessing the pre-shared key
- WPA3-Personal mode use Simultaneous Authentication of Equals (SAE), to strenghening the key exchange between clients and APs
	- its means, instead a client authenticating aganist a server or AP, the Client and AP can initiate teh authentication process equally and simultaneously. 
	- even if the password or key is compromissed, WPA3-personal offers forward secrecy, which prevent the encryption the data which is already trasmitted over the air. 

#####  802.1x

	based on the scale of the deployment

- since all support 802.1x or enterprise authentication, which implies EAP-based authentication, but the WPA versions do not require any specific EAP-based authentication
- instead of this, Wi-Fi certificates interoperability  with well-know EAP method like #EAP-TLS, #PEAP, #EAP-TTLS, #EAP-SIM.
- enterprise authentication is more complex to deploy then personal mode, because the servers must be set-up and configured as a critical enterprise resource. 
    
> **Note**
> Each WPA version is meant to replace its predecessors,  because of improved security mechanisms. Always use the highest available WPA version, that clients and wireless infrastructure will support.

- here comes the oveview talbe!!



## Building Wireless LAN

This section contain the following contents:

- Network Access - 

2.7 Describe physical infrastructure connections of WLAN components (AP, WLC,
access/trunk ports, and LAG)
2.8 Describe AP and WLC management access connections (Telnet, SSH, HTTP, HTTPS,
console, and TACACS+/RADIUS)
2.9 Configure the components of a wireless LAN access for client connectivity using GUI
only, such as WLAN creation, security settings, QoS profiles, and advanced WLAN
settings

- Security Fundamentals

5.10 Configure WLAN using WPA2 PSK using the GUI

### Connecting a Cisco AP


#### Autonomus AP 

- its manage everything to itself, also standalone
	- Approval of association requests
	- Transmitter power management
	- Radio Frequency (RF) management
	- Basic Service Set (BSS) management
	-  no central management for these, everything must be done one by one manual
- everything is build in, also just connect with the switch
- common port of the switch must be configured in trunk mode or access
	- in trunk mode, the 802.1Q encapsulation tags each frame according to the VLAN number it came from. The wireless side of a AP inheritely trunks 802.11 frames by assign them to the BSS and WLAN where they belong. 
	
- AP maps each VLAN to a  WLAN and BSS
  
#### Lightweight AP

- has one single Ethernet interface, just with the WLC paired fully functional.
- WLC maps wired VLANs to WLAN    
- even multiple VLANs are mapped to the same AP, via CAPWAP tunnel 
- that means, that LAPs need nur an access link on the network


> **Configuring APs**
> To manage Cisco APs, use serial console cable between PC and AP
> Once the AP has an IP address, Telnet and SSH is also available
> Autonomus APs support browser based management via HTTP and HTTPS


### Accessing Cisco WLC

- via browser, HTTP or HTTPS
- web gui and cli for monitor, configure and troubleshot
- can be reached with SSH session
- both GUI an CLI reqiure management users to log in
- users can be authenticated aganist a local user list or an AAA server like TACACS+ or RADIUS


#### Using a WLC 


##### Service Port

- used for out-of-band management, system management, initial boot functions, always connect to a switch port in access mode
- the switchs interface usually assigned to the management VLAN, so the device access is granted
- service port support only a single VLAN

##### Distribution system ports

- Used for all normal AP and management traffic, usually connects to a switch port in 802.1Q trunk mode
- these ports must be connected to the network, to carry the most of the data coming to and going from the controller
- CAPWAP tunnels (control and data) that extend to  each of a controllers APs pass across the distribution system ports
- client data get through from wireless LANs to wired VLANs over the ports
- all management traffic via web browser, SSH, Simple Network Management Protocol (SNMP), Trivial File Transfer Protocol (TFTP)... reach the controller in-band through the ports
- these ports can be configured redundantly pairs, primary and backup
- to get out the most of these ports, ports can configured to operate a single logical group, like a EtherChannel or port-channel on a switch
- these ports can be configured as a Link Aggregation Group (LAG), such that they are boundled together to act as one larger link
- LAG allowed load balancing and redundancy

> **Note**
> LAG act as a traditional EtherChannel, but Cisco WLCs do not support any link aggregation negotiation protocol, like LACP or PaGP
> Switch ports must be configured as an unconditional or always-on EtherChannel

##### Console Port

- used for out-of-band management, system recovery, and initial boot functions, asynchronous connections to a terminal emulator (9600 baud, 8 data bits, 1 stop bit, by default)

##### Redundancy Port

- Used to connect to a peer controller for high availability, (HA) operation


#### Using WLC Interfaces

- maps VLANs to the switched network
- provides the necessary connectivity through internal logical interfaces, IP address, subnet mask, default gateway, and DHCP server must be configured
- each interface is assigned to a physical port anda VLAN ID.

##### Management Interface 

- management traffic, RADIUS authentication, WLC to WLC, web based and SSH sessions, SNMP, NTP (Network Time Protocol), syslog ...
- also used to terminate CAPWAP tunnels between the controller and its APs

##### Redundancy Management

- the management IP address of a redundant WLC that is part of a high availability (HA) pair of controllers
- active WLC uses the management interface address, the standby WLC uses the redundancy management address

##### Service Port Interface

- bound to service port and used for out-of-band management

##### Dynamic interface 

- used to connect VLAN to a WLAN

> **Check this example**
> [Cisco Catalyst CW9800H1 and CW9800H2 Wireless Controllers Data Sheet]( https://www.cisco.com/c/en/us/products/collateral/networking/wireless/wireless-lan-controllers/cat-cw9800h1-cw9800h2-wireless-controllers-ds.html)

---

- management interface facing to the switched network
- the traffic consist of protocols like HTTPS, SSH, SNMP, NTP, TFTP...
- and consist of CAPWAP packets that carry control and data tunnels to and from the APs
- virtual interfaces used only for certain client-facing  operations
- DHCP request failed, controller can realy the request on to and actual DHCP server, that can provide the appropriate IP address
- from client perspektive, the DHCP appear on the virtual interface's address
- clients see this address but its never to usesd to communicate with other devices on the switched network
- also virtual interface used only for certain client management functions, its need a a unique, non-routable address from the private address range
	- use eg. 10.1.1.1 based on RFC 1918
	- or use reserved address,  192.0.2.0/24 which used documentation purposes, and its never used. (RFC 5737)
- one common virtual address is used to support client mobility
- all controller in the same mobility group used the same address, so the controllers are operate as a cluster, and provide services as the client roam from controller to controller
- these maps WLANs to VLANs, making logical connections between wireless and wired networks
- for each wireless LAN that offered by the controller's APs one dynamic interface is configured, which should be map the interface to the WLAN
- so each dynamic interface must be configured with unique IP address, that is act as a DHCP relay for wireless clients
- to filter traffic, use separate ACLs


### Configuring a WLAN

- AP advertise SSID to wireless clients
- controller connects to the LAN (VLAN) through one of its dynamic interfaces
- to complete the path between SSID and the VLAN, WLAN must be defined

![[Pasted image 20251221231224.png]]




> ** CCNA exam objektive**
> configuring WLAN for client connectivity with WPA2 and a PSK using only the controller GUI

- controller bind WLAN to one of its interfaces and push the configuration out for his APs
- WLAN will be adverteised and available to probe and connect to them
- WLAN can be used to segregate the traffic, such as by VLANs, build logical networks
- with bridging and routing can be WLANs connected
- due of this, **always plan the wireless network carefully, due of wide variety of devices, user  groups and policies**
	- but be care,, the number of WLANs that can be advertised at the same time (16) on the AP, is usually lower then the supported number of WLANs (512) in one contoroller
	- advertising each WLAN uses up valuable airtime
- to advertise all available BSS, APs must broadcast beacon management frames at regular intervals
	- to many WLAN use the whole airtime, no data traffic can be transmitted
	- usually 5 WLAN is maximum, but 3 is the overall best
- controlles has predefined, initial WLANs, before creating new one the following must be have:
	- SSID string
	- controller interface and VLAN number
	- Type of wireless security needed
	- these create a dynamic controller interface to support the new WLAN

### Configuring a RADIUS server

- first RADIUS server must be defined, configured
- use for WPA2-Enterprise and WPA3-Enterprise to AAA users
- on securirty section, choose AAA then RADIUS
- create a new server
	- add IP Address where the server is
	- enter the shared secret key and port number
	- enable the server to use
	- 

















