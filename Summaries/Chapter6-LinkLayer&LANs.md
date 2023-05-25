# Chapter 6: Link Layer and LANs

1. __Terminology & Context__
2. __Error detection, correction__
3. __Multiple Acess Protocols__
4. __LANs__

## Terminology
- Host and routers: **nodes**
- Communication Channels that connect adjacent nodes along communication path: **links**
    - wired links
    - wireless links
    - LANs
- layer-2 packet: **frame**, encapsulataes datagram

## Context 
Datagram transferred by differents links protocols, each link protocol provides different services.

### Link layer services

**Framing, link access**:
- encapsulate datagram into frame, adding header and trailer
- MAC addresses used in frame headers to identify source destination\
    MAC (Medium Access Control) addresses: layer-2 addrs

**Reliable delivery between adjacent nodes**
- seldom used on low bit-error link (fibe, some twisted pair)
- wireless links: high error ratios

**Flow control**:
- Pacing between adjacent sending and receiving nodes

**Error detection**:
- errors caused by signal attenuation, noise
- receiver detects presence of errors

**Error Correction**:
- receiver identifies and corrects bit error(s) withouth resorting to restramission

**Half-duplex and Full-duplex**:
- with half duplex, nodes at both ends of link can transmit, but not at same time


## Error detection, correction

EDC = Error Detection and Correction bits (redundancy)
D = Data protected by error checking may include header fields

Take into account error detection not 100% reliable! These protocols may miss some errors, but rarely. Larger EDC field yields better detection adn correction.

### Parity checking 

#### Single bit parity:
Detect single bit errors

| D Data bits | Parity Bit |
| ---: | :--- |
| 0111000110101011|0|

#### Two dimensional bit parity

Detect and correct single bits errors

### Internet checksum 

#### Sender:
Treat segment contents as sequences of 16-bit integers. Checksum: additions of segment contents and the sender puts checksum  value into UDP checksum field.\
Identical process of checksum evaluation in IPv4, over the packet header.

#### Receiver:
Compute checksum of received segment. Check if computed checksum; NO - error detected; YES - no error detected

#### Cyclic redundancy check

Its a more powerful erroer-detection coding. It views the data bits, D, as a binary number and choose r+1 bit pattern (generator), G.\
__Goal__: chose r CRC bits, R\
Its widely used in practice Ethernet, 802.11 Wifi

__Bit Pattern__:
| d bits | r bits |
| --- | ---|
| D: Data bits to be sent | R: CRC bits |

__Mathematical Formula__:
$D*2^r XOR R$

## Multiple Access Protocols

There are two types of "links":
- point to point 
    - PPP for dial-up; HDLC for point to point or multipoint
    - point to point link between Ethernet switch, host
- broadcast (shared wire or medium)
    - old-fashioned ethernet
    - upstream HFC (Hybrid Fiber Coax)
    - 802.11 wireless LAN

Single shared broadcast channel. Two or more simultaneous transmissions by nodes: interference. Collision if node receives two or more signals at the same time

__Multiple Access Protocol__: Distributed algorithm that determines how nodes share channel. Communication about channel sharing must use the channel itself.

#### An ideal multiple access protocol
__Given__: broadcast channel of rate R bps
1. When one node wants to transmit, it can send at rate R
2. When M nodes want to transmit, each can send at average rate R/M
3. Fully decentralized:
    - no special node to coordinate transmissions
    - no synchronization of clocks, slots
4. simple

### MAC Protocols: taxonomy
Three broad classes:
- __channel partitioning__
    - divide channel into smaller pieces
    - allocate piece to node for exclusive use
- __random access__
    - channel not divided, allow collisions
    - "recover" from collisions
- __taking turns__
    - nodes take turns, but nodes with more to send can take longer turns

#### TDMA: Time division multiple access
- channel capacity is divided in time frams and then slots
- access to channel in "rounds"
- each stationg gets fixed length slot (length = pkt transmission time) in each round (each transmitting at R/N)
- unused slots go idle
- __Pros__: no collisions, fair
- __Cons__: time wait, may waste capacity
- Uses: cellular netowk (2G/3G), industrial networks for automation and control and also in satellite, military and aviation communications

#### FDMA: Frequency division multiple access
- channel spectrum divided into frequency bands
- each station assigned fixed frequency band
- unused transmission time in frequency bands goes idle
- same TDMA pros and cons
- Uses: analog radio broadcasting, land mobile radio, digital tv broadcasting

#### TDMA vs FDMA

The choice between __TDMA__ and __FDMA__ depends on the specific application requirements and network conditions

| TDMA | FDMA |
| --- | --- |
| There are strict requirementes for latency and delay | Where simplicity and flexibility are more important |
| The channel is susceptible to interference | There are large number of users that need to be supported |

### Random access protocols

__Random access MAC protocol__ specifies:
- how to detect collisions
- how to recover from collisions

### Examples:

#### Pure (unslotted) ALOHA
- unslotted ALOHA: simples, no synchronization
- when frame first arrive it transmit immediately

#### CSMA (Carrier Sense Multiple Access)
Listen before transmit:
- if channel sensed idle: transmit entire frame
- if channel sensed busy, defer transmission

__Collisions can still occur__: propagation delay means 2 nodes may not each other\
__Collision__: entire packet transmission time wasted

#### CSMA/CD (Collision Detection)
Carrier sensing, deferral as in CSMA
- collisions detected within short time
- colliding transmissions aborted, reducing channel wastage

__CSMA/CD fails in WLANs__ since the collision detection does not work properly

#### Taking turns MAC protocols

__Channels partiitioning MAC protocols__:
- share channel efficienty and failry at high load
- inefficient at low load: delay in channel access. I/N bandwith allocated even if only 1 active node!

__Random Access MAC protocols__:
- efficient at low load: single node can fully utilized channel
- high load: collision overhead

__Polling__:
Master node invites slave nodes to transmit in turn
Concerns:
- polling overhead
- latency
- single point of failure (master)

__Token passing__:
Control __token__ passed from one node to next the token message.

#### Summary of MAC protocols

- __Channel partioning__: by time/frequency
- __Random Access__ (Dynamic)
    - ALOHA, S-ALOHA, CSMA, CSMA/CD
    - CSMA/CD used in Ethernet (IEEE 802.3)
    - CSMA/CA used in WLANs (IEEE 802.11)
- __Taking turns__
    - polling from central site
    - token passing: Blueetooth, FDDI (Fibe Distributed Data Interface), token ring (IEEE 802.5)

## LANs

### MAC addresses and ARP
32 bit IP address: Network-layer address for interface, used for layer 3 (network layer) forwarding

__MAC address__:
Function: used locally to get frame from one interface to another physically-connected interface (same network, in IP-addressing sense)
- 48 bit MAC address (most LANs) burned in NIC ROM also sometimes software settable\
Each adapter on LAN has an unquie address.

MAC address allocation administered by IEEE. The manufacturer buys portion of the address space.

#### ARP: Address resolution protocol

In order to determine the MAC address' inteface knowing its IP address we use the __ARP Table__: each IP node (host, router) on LAN has a table.
- IP/MAC address mappings for some LAN nodes: < IP address; MAC address; TTL (Time to live)>

## Ethernet

The __"dominant"__ wired LAN technology. It was the first widely used LAN technology since it was simple and cheap. It works with a single chip and uses multiple seeds. It also kept up with speed race (10 Mbps - 40 Gbps)

#### Physical topology

__bus__: All nodes in same collision domain, they can collide with each other\
__star__: Active switch center, each "spoke" runs a separate Ethernet protocol this way nodes do not collide with each other

#### Frame structure

__Preamble__ - 7 bytes with patter 10101010 folly by one byte with pattern 10101011, this is used to synchronize receiver and sender clock rates

__Addresses__: 6 byte source, destintion MAC addresses

__Type__: indicates higher layer protocol

__CRC__: cyclic redundancy check at receiver

#### Unreliable, Connectionless

__Connectionless__: No handshaking between sending and receiving NICs
__Unreliable__: receiving NIC doesn't send acks for nacks to sending NIC\
Ethernet's MAC protcol: __CSMA/CD__

## Switches & VLANS

### Ethernet Switch
The ethernet switch is a link-layer devices. It takes an active role in storing and forwarding Ethernet frames. It also examines incoming frame's MAC address, selectively. This switch is also very transparent since the host are unaware of its presence. It follows a plug-and-play and self-learning since it doesn't need to be configured.

#### Multiple simultaneous Transmissions

Host have dedicated direct connection to switch, and there's a switches buffer packets.  The ethernet protocol used on each incoming link allows for no collisions and full duplex. Each link has its own collision domain 

__How does the switch know what interface connects to each host?__
For that its used a __Switch table__ with each entry, its pretty similar to a routing table.

__How are entries created and maintained?__ Due to its self-learning pattern the switch *learns* which hosts can be reached through which interfaces

#### Frame filtering/Forwarding

When the frame is received at switch:
1. it records the incoming link, the MAC address of sending host
2. it indexs the switch table using MAC destination address
3. __if__ the entry is found for dest\
__then__ {\
__if__ dest is on the segment from which frame arrived\
__then__ it drops the frame\
__else__ it forwards the frame on interface indicated by entry\
}\
__else__ flood, basically it forwards the frame on all interfaces except arriving
interface 

#### Interconnecting Switches

Its possibles switches can be connected together, you may be wandering how then can I connect to a host after multiple layers of switches but just like it was mentioned previously due to the self learning nature of switches.

#### Switches vs. Router

In both cases they are store-and-forward. __Routers__ work has a network-layer device meanwhile __switches__ work has link-layer devices. Another thing they have in common is that both have forwarding tables. Whilst __routers__ compute the tables using routing algorithms (IP addresses), __switches__ learn how to populate their forwarding table using flooding, learning and MAC addresses.

### VLANs

VLAN or Virtual Local Area Network can be configured to define multiple virtual LANs over a single physical LAN infrstructure.

#### Port based VLAN
This VLANs basically work with __traffic isolation__, __dynamic membership__ and __forwarding between themselves__.

#### VLANs spanning multiple switches
In this cases it exists a __trunk port__ that carries frames between VLANs defined over multiple physical swithces.
