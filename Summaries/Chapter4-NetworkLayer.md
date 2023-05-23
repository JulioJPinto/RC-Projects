# Chapter 4 Network Layer

1. **Introduction**
2. **Virtual Circuit and Datagram Networks**
3. **IP: Internet Protocol**
4. **Routing Algorithms**

## Network layer

- Transposrt the segment from the src to the dest host
- The side that send encapsulates the segments into the datagrams
- The dest delivers segments to transport layer
- Network layer protocols in *every* host, router
- router examines header fiels in all IP datagrams passing through it

#### Two key network-layer functions

- **Forwarding**: move packets from router's input to router output
- **Routing**: Determine route taken by packets from src to dest

## Virtual Circuit

Network provides network-layer *connection* service

"source-to-dest path behaves much like telephone circuit"
- performance wise
- network actions along source-to-dest path

#### VC implementation
Consist of:

1. *path* from src to dest
2. *VC numbers*, one number for each link along path
3. *entries in forwarding tables* in routers along path

Packet belonging to VC carrier VC number (not dest address)
The VC number can be changer on each link.

#### Signaling protocols

Are used to setup, maintaind and teardown VC. They are used in ATM or frame-relay networks. This is not used in today's Internet.

#### Datgram networks

These do not need any setup call at network layer. The routers have no state to doubt end-to-end connections (No network level concept of "connection"). The packets are forwarded using dest host adress.

### Datagram or VC network: why?

|  Internet Datagram| ATM (VC) |
|:---:|:---:|
|  Data exchange among computers | Evolved from telephony  |
|  Many link types | Human conversation, strict timing, reliability requiremnts |
|  "smart" end systems (computers) | "dubm" end systems  |

## IP: Internet Protocol

#### The Internet network layer
Host, router network layer functions:

| Names | Functions |
| :---: | :---: |
| Transport Layer | TCP, UDP|
| Network Layer| *routing protocols*, *IP Protocol* & *ICMP protocol*|
| Link Layer | --- |
| Physical Layer | --- |

- ***Routing Protocols:*** 
    - path selection (RIP, OSPF, BGP) &harr; Forwarding table

- ***IP Protocol:***
    - Addressing conventions
    - Datagram format
    - Packet Handling conventions

- ***ICMP Protocol:***
    - error reporting
    - router "signaling"

### ICPM: Internet Controll Message Protocol

Used by hosts & routers to communicate network-level information like error reporting and echo request/reply (used by ping).

In the nertwork-layer "above" IP: ICMP messages carried in IP datagrams

*ICMP message:* type, code, checksum, description.\
ICMPv4 - RFC792; ICMPv6 - RFC4443

#### Traceroute and ICMP

Source sends serires of UDP segments to dest
- first set has TTL = 1, etc.
- unlikely port number

When the n set of datagrams arrives to n router, the router discards the datagrams and sends to source ICMP messages (type 11, code 0). ICMP messages includes name of router & IP address

When the ICMP messages arrive to the src records RTTs

__Stopping Criteria:__
UDP segment eventually arrives at dest host. The dest returns ICMP "port unreachable" message (type 3, code 3). The src stops

#### IP datagram format
How muuch overhead?
- 20 bytes of TCP
- 20 bytes of IP
$0 bytes + app layer overhead

#### IP fragmentation, reassembly

Network links have MTU (max transfer unit) size - the largest possible link level frame - different links has differents MTUs

Large IP datagram divided within net. One datagram becomes several datagrams and its "reassembled" only at final dest. IP header bits used identify order of related fragments.

__Manipulated fiels in IPv4 fragmentation:__
- Identification - identifies fragments of the same original datagram
- More fragments - Flag that determines the number of fragments or if the fragment is the last
- May fragment - Identifies if there is or not fragmented by the network
- Fragment offset - Offset of the data fragmented relative to original datagram

IPv6 by default doesnt use fragmentation! 

### IP Addressing
The *IP adress* is a 32-bit identifier for host, router interface (connection between host/router and physical link). IP addresses associated with each interface.

IPv4: *32-bit* unsigned binary value __(xxxxxxxx.xxxxxxxx.xxxxxxxx.xxxxxxxx)__. A part identifies the network/subnet and the other the host interface in that network. In the Internet each adress must be unique. Theses addresses are distribuited by 5 class (A to E), these classes are given by IANA(Internet Assigned Number Authority)

| Class | Identifier | Network Address | Destinatary Address |
| :---: | :---: | :---: | :---: |
| A | 0 (1 bit) | 7 bits | 24 bits |
| B | 10 (2 bits) | 14 bits | 16 bits |
| C | 110 (3 bits) | 14 bits | 16 bits |
| D | 1110 (4 bits) |Multicast Address between 224.0.0.0 - 239.255.255.255  </td> | - |
| E | 11110 | Reserved for the future | - |

#### Classful vs. Classless
Addressing with classes (__Classful__):
- based in RFC 791
- uses the first bits has an identifier for the class

Addressing without classes (__Classless__):
- doesn't conside the bits for class and uses a 32 bit mask to determine the network address
- Uses CIDR (Classless Internet Domain Routing)
- Uses ISPs routing tables

#### CIDR
Pattern that with the IP address returns the subnet part of the address


| Class | Binary | Decimal | CIDR |
| :---: | :---: | :---: | :---: |
| A | 11111111.00000000.00000000.00000000 | 255.0.0.0 | /8 |
| B | 11111111.11111111.00000000.00000000 | 255.255.0.0 | /16 |
| C | 11111111.11111111.11111111.00000000 | 255.255.0.0 | /24 |

When not using classes the mask can take any value, this allows the criation of subnets or supernets.

Take int account the IP 130.1.5.1:
- If its class B the network has a default mask of /16. So the addres of the station is 5.1 da net 130.1.0.0

Take into account the IP 130.1.5.1/24:
- Therefore de station as the address 1 and the subnet is 130.1.5.0
- the subnetting is definied in the host ID (__\<network id>\<subnet id>\<host id>__)

#### Subnets

A subnet is a device interfaces with the same subnet part of an IP address. It can physically reach each other without contacting with the router.

__IP Address__: Subent part (high order bits) and the host part (low order bits)

__Recipe__: to determine the subnets, we detach each interface from its host or router, creating islands of isolated networks (subnets)

| Advantages | Disadvantages |
| :---: | :---: |
| Better organization and managment of addresses | Reduces address spacing (many addresses become obsolete) |
| Modularity to add more more levels of hierachy | Managment requires more work |

### IP Routing

The router and the stations both have a routing table.

#### Forwarding Algorithm

Its easier due to addressing hierarchy. IP address: __a.b.c.d/m = X.Y__ (Network Station)

1. Uses the mask to extract the network address __X__
2. Looks for the entry that best adjuste to __X__ If __X__ is local it deliver to the interface __X.Y__ (Direct delivery). If it doesn't uses __X__ it determines the next hop
3. By default the entry is (0.0.0.0/0 or default) adjusts to all __X__

#### Static vs. Dynamic 

__Static__ - Based in the default routes
- the routes stay static
- reduces traffic on the network
- simple scheme but not flexible

__Dynamic__ - Routes are updated 
- the router trade routing information
- this dynamic route are updated via specific routing protocols
    - RIP (Routing Information Protocol)
    - OSPF (Open Shortest Path First)
    - BGP (Border Gateway Protocol)
    - etc
- Great flexibility and adaptation to mistakes or the changes are configured in the network
- the update traffic may lead to an overload of the network

#### Default Route 

The Default route is a route thats used when the specified route does not exist in the routing table for the dest network. Its a specific static route. It has the least priority when compared to other routes and its identified by the term default or 0.0.0.0

The routing protocols work has a graph and calculate the best routing for a certain dest.

#### Dynamic route computation

__Dynamic computing of the routes__:
- centralized - each router determines the best route to a determined dest of that topology
- distributed - each router send information to the best route that knows its neighbours 

__Utilized Principle__:
- Vector Distance - RIP, IGRP
- Link State - Open Shortest Path First (OSPF)(uses Dijkstra's algorithm)

#### Route computation

