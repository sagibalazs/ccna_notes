
# CCNA Ex. Vol.1

## Introduction to TCP/IP Networking


#protocol - set of logical rules



#### OSI Model

| Layer            | PDU      | Used Protocols                  | Network Devices                        |
| ---------------- | -------- | ------------------------------- | -------------------------------------- |
| **Application**  | Data     | HTTP, FTP, SMTP, DNS, IMAP      | End-user devices (PCs, mobile devices) |
| **Presentation** | Data     | SSL/TLS, JPEG, GIF, MPEG        | Gateways, Translators                  |
| **Session**      | Data     | NetBIOS, RPC, PPTP              | Gateways, Firewalls                    |
| **Transport**    | Segments | TCP, UDP, SCTP,DCCP             | Firewalls, Load Balancers              |
| **Network**      | Packets  | IP, ICMP, IPsec, OSPF, BGP      | Routers, Layer 3 Switches              |
| **Data Link**    | Frames   | Ethernet, PPP, ARP, Frame Relay | Switches, Bridges                      |
| **Physical**     | Bits     | Ethernet, DSL, ISDN, Wi-Fi      | Hubs, Cables, Network Interface Cards  |

---

#### TCP/IP Model


| Layer           | PDU      | Used Protocols                  | Network Devices                        |
| --------------- | -------- | ------------------------------- | -------------------------------------- |
| **Application** | Data     | HTTP, FTP, SMTP, DNS, IMAP      | End-user devices (PCs, mobile devices) |
| **Transport**   | Segments | TCP, UDP, SCTP                  | Firewalls, Load Balancers              |
| **Internet**    | Packets  | IP, ICMP, IPsec, OSPF, BGP      | Routers, Layer 3 Switches              |
| **Link**        | Frames   | Ethernet, PPP, ARP, Frame Relay | Switches, Bridges, NICs                |


#### Protocols


#ip

**