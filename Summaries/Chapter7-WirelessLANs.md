# Chapter 7 - Wireless LANs

1. Wireless links, characteristics
2. IEEE 802.11 wireless LANs ("Wi-Fi")

#### Elements of a wireless network

- Wireless Hosts
- Base Station
- Wireless Link
- Infrastructure Node
- Ad hoc mde

## Wireless Links, Characteristics

There are some __important__ differences from wired links when it comes to wireless links. In wireless links:
- **decreased signal strenght**
- **interference from other sources**
- **multipath progration**

ALl of this make communication across a wireless link much more "difficult" when compared to a wired link.

In wireless links theres also signal-to-noise ratio or SNR. The larger the SNR is the easier it its to extract signal from noise (actually good).\
When compared to BER (Bit error rate) there can be found a couple of tradeoffs. Given physical layer makes it so there is an increase in power and SNR but a decrease in BER. Given SNR makes it choose physical layer that meet BER requirement, giving highest throughput. The SNR may change with mobility: dynamically adapt physical layer.

Some characteristics about a wireless network is that multiple wireless senders and receiver can create additional problems like a __hidden node/ fading__ or an __exposed node__.

For example (refer to the image in the powerpoints Chapter 7 - Wireless and Mobile Network) in a __hidden node__ problem due to obstacles for examples N1 and N3 cannot touch each other, due to that their packages colide in N2. Meanwhile in an __exposed node__ problem N1 and N4 are able to simultaneously receive but N2 and N3 are both in a reachable zone. (This problem is less severe than fading)

## IEEE 802.11 Wireless LAN 
#### Architecture

On the basis of this architecture there is a wireless host that communicates with the base station (access point AP). Other then that there is also a Basic Service Set (BSS or "cell") in infrastructure mde that contains: wireless hosts, an AP and ad hoc mode (hosts only). Last but not least there is an Extended Service Set or ESS that includes one or more BSSs

#### Channels, Association

In IEEE 802.11 there is a spectrum divided into channels at different frequencies. The AP admin chooses frequecny for AP. Keep in mind interference is possible: channels can be the sames as that chosen by the neighboring AP.\
In terms of association, the arriving host must associate with an AP. This way it can scan channels, listen for beacon frames containg AP's name (SSID) and MAC address. The host can also select which AP to associate with. Only then may it perform authentication before assocaiation and run DHCP to get the IP addresses in AP's subnet.

#### Passive/Active Scanning

__Passive Scanning__:
1. Beacon frames sent from APs
2. Association Request sent
3. Association Response frame sent from selected AP

__Active Scanning__:
1. Probe request frame broadcast
2. Probe response frames sent from APs
3. Association Request frame sent to selected AP
4. Association Response frame sent from selected AP

#### Multiple Access
Theres a few thing with multiple access when it comes to IEEE 802.11.
IT can avoid collisions with 2+ nodes transmitting at same time. CSMA (802.11) can sense before transmitting this way it doesn't collied with onging transmission by other node. But there is no collision detection, making it so the goal is to avoid collisions.

#### MAC Protocol: CSMA/CA

__Sender__:
If channel idle for __DIFS__ then sets sets time t and transmits the entire frame. If there is no ACK within t, it increases random backoff interval otherwise the transmission is successful. If it senses the channel is busy then it staart a random backoff time, while the channle is idle the timer counts down and once it expires it senses the channel again.

__Receiver__: If the frame is received OK, return ACK after __SIFS__

DIFS – DCF (Distributed Coordinated Function) Interframe Space (standard-dep, from 28 to 50 μs)\
SIFS – Short Interframe Space (standard-dependent, usually from 10 to 16 μs)

#### Avoiding Collisions

The base _idea_ behind it is to allow the sender to "reserve" channel rather than random acces of data frames, avoiding collision of long data frames. the sender transmist RTS (Request To Send) packets to BS (Base Station) using CSMA. BS broadcast clear to send CTS in response to RTC and CTS is heard by all nodes.

#### Frame Addressing
Within the frame 4 addresses are stored.
1. MAC address of wireless host or AP to receive this frame
2. MAC address of wireless or AP transmitting this frame
3. MAC address of router interface which AP is attached
4. Used in some casses withing ESS or in ad hoc mode

#### Frame types

There are a bunch of different frame types but lets focus on 3.
1. Management frames - used to perform supervisory functions
2. Control frames - used in conjunction with data frames to perform control operation
3. Data frames - used to send data from STA to STA.

#### Advanced Capabilites
Some advanced capabilities could be **Rate Adaptation** and **Power Management**
