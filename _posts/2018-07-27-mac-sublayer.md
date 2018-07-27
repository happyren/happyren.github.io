---
layout: post
title: MAC sublayer[Introduction to Internet]
tag: knowledge
---

- [Introduction to Internet Technology](https://homelabdefense.com/2018/07/27/intro-internet/)

- [Data Link layer](https://homelabdefense.com/2018/07/27/datalink-layer/)

---

MAC sublayer is important in broadcasting network, important in LAN especially for wireless network.

## Static Channel Allocation

Frequency Division Multiplexing: for N users, channel is divided into N frequency bands, hence each user has its own band, without interfering

## Dynamic Channel Allocation

### ALOHA and Slotted ALOHA

ALOHA allows different stations sends the frames whenever they need to, a collision would leads to retransmission, it has the best output at expected frame = 0.5 with successful rate at 0.18.

Slotted ALOHA requires stations send frames at the beginning of a slot, it has the best performance at expected frame = 1 with successful rate at almost 0.37.

### Carrier Sense Multiple Access Protocol

- 1-persistent CSMA: when a station finds out the channel is busy, it will wait, when it sees it is idle, it will 100% sure start transmission.
> Propagation delay would influence on it, 100% certainty of transmission is bad, if propagation delay is small, then collision possiblity is small.

- nonpersistent CSMA: same as 1-persistent CSMA, only it will wait a random period of time till it start transmitting, but it adds delay.

- p-persistent CSMA: used on slotted channel, when channel is idle, it has __p__ percent chance of transmitting, otherwise wait till the next slot.

- CSMA with Collision Detection: when the collision happens, the stations detect it and stop transmission immediately, and it will wait 2 times propagation delay for the sender to ack the collision, hence the frame length would better be 2t.

- CSMA with Collision Detection: when the collision happens, the stations detect it and stop transmission immediately, and it will wait 2 times propagation delay for the sender to ack the collision, hence the frame length would better be 2t.

### Collision-Free Protocol

- Bit-map protocol: for channel of N contention, it will has N slots, for j station which has frame to send, it will transmit a 1 on j slot, and then by the slots, frames are transmitted. When the channel load are growing higher, the performance ioncreases.

- Token Passing: only with the token, a station is allowed to send a frame. Also known as token bus.
> There has been FDDI(Fiber Distributed Data Interface) and RPR(Resilient Packet Ring)

- Binary Countdown: Since Bit-map and token passing both need 1 bit overhead, binary addressing could save a lot while there exist a lot stations. In this protocol, every bit time, if a station has frame to send, it will send its address, if sent 0 saw 1, the station will quit contention. The one with the final binary seq address will send the frame. A high numbered station has high priority.

### Limited Contention Protocol

Use contention protocol at low load to save delay and collision-free at high load to increase efficiency.

- Adaptive Tree Walk protocol: using depth-first tree to permit the transmission.

1. Slot 0(first Slot after a successful transmission), all stations are allowed to transfer.

2. Slot 1, if contention happens in the Slot 0, then all stations under node 2 are allowed to compete in this slot.

3. Slot 2, if contention happens in the Slot 1, then all stations under node 4 are allowed to compete in this slot.

...

### Wireless LAN Protocol

> Exposed and Hidden Terminal: A Hidden terminal is that a transmitting station that is beyond the range of the current station; a Exposed terminal is that a two transmitting station has no collision on receiver but can see each other transmitting.

- MACA(Multiple Access with Collision Avoidance): Sender sends RTS(Request To Send) to stimulate the receiver to send a CTS(Clear To Send) to pause receiver nearby stations to stop transmitting, then the actual data frame is sent.

## Ethernet

Classic and switched ethernet: Classic Ethernet solves multiple access as above, switched network uses a switch device.

### Ethernet MAC sublayer protocol analysis

![MAC sublayer structure](https://upload.wikimedia.org/wikipedia/commons/c/cb/SERCOS_III_Control_Interface_Telegram_Structure_Diagram.svg)

1. first 8 bytes, 10101010(last byte of the 8 bytes is different, it is 10101011, which is _Start of the Frame_ delimiter), by the Manchester encoding, this sequence would generates a 10MHz square wave for 6.4 usec.

2. the next 12 bytes contains 2 address, first is the destination address and the second is the source, the first bit of the address is **0** for ordinary address and **1** for Group address, which allows multiple stations to listen to a single address, which is multicasting, and if the address is all **1**, it is broadcasting. Source address is globally unique, assigned centrally by **IEEE**, first three bytes indicates the manufacturer, last three bytes are by manufacturer.

3. Type of Length field, for Ethernet, type field tells the OS which protocol should the packet be handed in to, 0x0800 means IPv4; for IEEE 802.3, this field carrys the legth of the frame, which means a layering violation would be existed, and the extra header from LLC(Logical Link Control) has to be appended to use 8 bytes convey a 2 bytes protocol information for receiver to handle. After 97, IEEE says that all this field with a value greater than 1500(1536 = 0x600) is type, and smaller is length.

4. Data, up to 1500 bytes, mostly constrained by the RAM price at that time, in addition to the MAX size of the frame, it also has a MIN size of a frame, first reason is that it a station may truncate the frame while detect collision, hene a garbage frame may be on the cable, Ethernet requires a MIN of 64 bytes frame to distinguish message from garbage, it should be from destination address to checksum, including both, contains 64 bytes, both address are 6 bytes, type field of 2 bytes, checksum of 4 bytes, leaves at most 46bytes padding while 0 message is transmitted, the second reason to have MIN length is while a message is too short, collision at the end of the transmission line does not have enough time to come back, and the sender might mistaken that transmission is successful.

After this we are going to - [Network layer](https://homelabdefense.com/2018/07/27/network-layer/).
