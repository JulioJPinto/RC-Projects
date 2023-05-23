# Chapter 1: Introduction

 1. __What is the internet__
 2. __Network Edge__
 3. __Network Core__
 4. __Delay, Loss, Throughput in Networks__
 5. __Protocol Layers, service models__
 6. __Network under attack: Security__
 7. __History__


 ## What is the internet

 ### How to view the internet

 #### "Nuts and Bolts" View

 - Internet: "network of networks"
 - Protocol control sending, receiving of messages
 - Internet Standards
    - RFC: Request for comments
    - IETF: Internet Engineering Task Force

#### A Service View

- Infrastructure that provides services of applications
- Provides programming interface to apps
    - hooks that allow sendind and receiving app programs to "connect" to the Internet
    - provides service options, analogous to postal service

### What's a protocol?

#### Network protocols:
- machines rather than humans 
- all communication activity in Internet governed by protocols

__Protocols__ define format, order of messages sent and received among network entities, and actions taken on a message transmission, receipt

## Network edge

### Network structure

#### Network Edge
- Hosts:
    - clients 
    - servers

#### Acess Networks
- wired
- wireless 

#### Network Core
- interconnected routers
- netwrks of networks

### Acess network

#### Digital Subscriber line (DSL)

Use exisiting telephone line to DSLAM. The data over DSL line goes to the Internet. Meanwhile the voice over DSL line goes to the telephone network.

#### Cable Network
Frequency division multiplexing: different channels transmitted in different frequency bands

- HFC: hybrid fiber coax
    - Up to 500Mbps-1Gbps downstream transmission rate, up to 100Mbps upstream
    - Network of cable, fiber attaches homes to ISP router. Homes share acess network to cable headend, unlike DSL, which has dedicated acess to central office

#### Wireless Acess Network

__wireless LANs:__
- within building (100 ft.)
- 802.11 b/g/n/ac (Wifi): 11, 54, 450Mbps, 1-2Gbps transmission rate

__wide-area wireless acess:__
- provided by telco (cellular) operator, 10s km
- between 1 and 100 Mbps (avg)
- 3G, 4G, 5G

#### Host: sends pockets of data

Takes the messages and breaks it into smaller chunks, known as packets of lenght L (bits). Transmits these packets at a transmission rate R.
This can also be called link transmission rate or link capacity aka link bandwidth

$$
\begin{align}
    \frac{L (bits)}{R (bits/sec)} = PTD 
\end{align}
$$

PTD - Packet Transmission Delay

#### Physical media
- bit: propagates between transmitter/receiver pairs
- physical link: what lies between transmitter & receiver
- guided media: signals propagate in solid media (copper, fiber, coax)
- unguided media: signals propagate freely (radio)

## Network Core

Packet Switching: hosts break application-layer messages into packets
    Forward packets from one router to the next across links on path from src to dest. Packet transmitted at full link capacity.

### Packet Switching
#### Store and Forward
Entire packet must arrive at router before it can be transmitted on next link.

#### Queueing delay, loss
Packets will queue, wait to be transmitted on link. They can be dropped (lost) if memory (buffer) fills up

#### Two keys network-core functions
__Routing__: determines source-destination route taken by packets
__Forwarding__: move packets from router's input to appropriate route output

#### Packet switching vs Circuit Switching
__Packet Switching:__

Great for bursty data
- resource sharing
- simpler, no call setup

Excessive Congestion possible: packet delay and loss
- protocols needed for reliable data transfer congestion control

## Delay, loss, throughput in networks

Packet queue in router buffers - Packet arrival rate to link (temporarily) exceeds output link capacity, packets queue and wait for the turn

## Protocol Layers, service models

#### Network pieces
- hosts
- routers
- links of various media
- applications
- protocols
- hardware, software

#### Why layering?

Dealing with complex systems is easier when explic structure allows identification and relationship of complex system's pieces. Modularization also makes the maintenance and updating of the system easier.

#### Internor Protocol Stack
- __Apllication__: FTP, SMTP, HTTP
- __Transport__: process-process data transfer (TCP, UDP)
- __Network__: routing of datagrams from src to dest (IP, routing protocols)
- __Link__: data transfer between networks (Ethernet, 802.11 (Wi-fi), PPP)
- __Physical__: bits "on the wire"