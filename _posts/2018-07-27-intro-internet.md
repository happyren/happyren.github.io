---
layout: post
title: Introduction to Internet[Introduction to Internet]
tag: knowledge
---

> Internet Technology Note sheet, based on _Computer Network Pearson.2011_

- [Internet Usage](#internet-usage)
- [Hardware Technology](#hardware-technology)
- [Software Technology](#software-technology)
- [Reference Model](#reference-model)

## Internet Usage

### For business

Sharing data amongst employees are necessary. At a point of resource sharing, the management would like to connect all the private employee computers together. It normally deployed in **client-server model**.

> Client-server model: client send a message to server, then server process and send message back to client. ![Client-Server](https://upload.wikimedia.org/wikipedia/commons/thumb/c/c9/Client-server-model.svg/1200px-Client-server-model.svg.png)

Setting up computer network in business normally has goals:

1. Connect employee and make easy for resource sharing.

2. Providing powerful tools for employee to communicate.

3. Doing business electronically, especially with customers.

### Home Application

The biggest reason for home to buy personal computer is to connect to the Internet. And the connection is often processed in peer-to-peer model.

> Peer-to-peer model: individuals form a loose group and make communication within the group.![Peer-to-peer](https://upload.wikimedia.org/wikipedia/commons/thumb/3/3f/P2P-network.svg/2000px-P2P-network.svg.png)

Home internet is often setup due to:

1. Let home user connect to other computers.

2. Person-to-person communication, using social media or create content jointly like wikipedia.

3. Electronic commerce, online shopping, amazon, e-flea market.

4. Entertainment like online games.

5. Ubiquitous computing in Internet of Things, RFID.

### Mobile users

Cellular, WiFi, GPS, etc.

## Harware Technology

Generally, there are three type transmission technology in daily usage.

1. Broadcast link: Packet coubld be sent by any machine and recieved by all the others. Machine only process packets intended to be received but drop those do not.

2. Multicasting: packets are transmitted to subset of machines.

3. Point-to-point link: Packet is sent by exactly one sender and received by exactly one receiver.

And the technology could also be categorized by scale:

| Distance | Density | Example |
| ------------------ |:-------:|:-------:|
| 1 m | Square Meter | Personal Area Network|
| 10 m | Room | Local Area Network |
| 100m | Building | LAN |
| 1 km | Campus | LAN |
| 10 km | City | Metropolitan Network |
| 100 km | Country | Wide Area Network |
| 1000 km | Continent | WAN |
| 10,000 km | Planet | Internet |

### PAN

Let devices communicates over a range of a person. For example, computer with monitor, keyboard, and mice, cable or bluetooth.

### LAN

Privately owned network operates within and nearby an office, home, or building , or factory. It requires Access Point, or wireless router, or base station. Protocol _IEEE 802.11_ is widely known as **WiFi**, while _IEEE 802.3_ is widely known as **Ethernet**. Logical divided LAN is also possible, which is Virtual LAN or VLAN.

In LAN, since the size is restricted, we could know the worst case scenario in advance. And for wired LAN conncetion using copper wires and optical fiber, the connection speed often at 100Mbps to 1Gbps, the latest reach 10Gbps. **Wired is overrun wireless in every way**.

- Static allocation: it divide channel into time slot, a machine can only broadcast during its slot, which caused channel capacity waste when a machine has nothing to say.

- Dynamic allocation: Centralized allocation would have a central entity which determines using an algorithm to decide which packet goes next, where the decentralized allocation doesn't have that central entity but using each machine to decide when to send the packet.

### MAN

MAN is firstly using by television channels, to distribute TV to subs, then it is found the cable could carry more channels, so Internet is also included.

### WAN

WAN spans large geo area, it consists of hosts and communication subnet, which requires transmission cable and switch elements. Compares to LAN, it separates communication and application for easy deployment. It utilizes multiple network technologies. And its subnet could connect to not only individual computers but also LANs.

Subnets could also be or not to be a Internet Subnet, and the service providers could vary for a **Network Service Provider** to **Internet Service Provider**.

> The method IPS choosing path called routing, choosing destination called forwarding.

### Network Devices

| Devices | Usage |
| --- | :---: |
| Repeater | Regenerates the signal to keep transmission strength|
| Hub | A multiport repeater |
| Bridge | On Data-Link Layer, filtering contents by reading MAC, connect 2 devices|
| Switch | Multiport Bridge with buffer |
| Router | Network Layer device, like switch, but can read IP address |
| Gateway | a passage to connect two networks together with both using different models |
| Access Point | Providing Wireless Connection for devices to Ethernet network |

## Software Technology

- [Protocol Hierarchy](#protocol-hierarchy)
- [Design Issues](#design-issues)
- [CO vs CL](#co-vs-cl-interms-of-services)
- [Service Primitives](#service-primitives)
- [Services to Protocols](#services-to-protocols)

### Protocol Hierarchy

**Protocol** is an agreement between peers on how communication is proceed.

Actual data transmission is proceed on Physical Layer.

Between each pair of adjacent layers, there exist a interface, where lower layer provides premitive operations and services to upper layer.

Interface and layers should be properly designed, meet the _PoLP_, any change to the lower layer should not be noticed by the upper layer.

Protocol stack is the list of protocols used in different layers, one per layer.

### Design Issues

1. Reliability: error detection, error correction, routing(path finding).

2. Network evolution: protocol layering, addressing, internetworking, model scalability.

3. Resource allocation: Statistical multiplexing, flow control, congestion, QoS.

4. Security: confidentiality, authentication, integrity.

### CO vs CL (Interms of services)

CO is modeled after telephone system, CL is modeled after postal system.

- Store-and-forward switching: intermediate node receive the whole message, then send it.

- Cut-through switching: onward transmission starts before the message is fully received.

| CO or CL | Services | Example |
| --- | :---: | :---: |
| CO | Reliable message stream | Sequence of pages |
| CO | Reliable byte stream | Movie download |
| CO | Unreliable connection | Voice of IP |
| CL | Unreliable datagram | Electronic junk mail |
| CL | Acknowledged datagram | Text messaging |
| CL | Request-reply | Database query |

### Service Primitives

| Primitive | Meaning |
| --- | :---: |
| LISTEN | Block waiting for an incoming connection |
| CONNECT | Establish a connection with a waiting peer |
| ACCEPT | Accept an incoming connection from a peer |
| RECEIVE | Block waiting for an incoming message |
| SEND | Send a message to the peer |
| DISCONNECT | Terminate the connection |

LISTEN, CONNECT, ACCEPT could be explained by 3-way handshake.

RECEIVE and SEND could be explained by HTTP transmission.

DISCONNECT could be explained by 2 pairs of DISCONNECT and ACK.

### Services to Protocols

Service is not protocol, **service** is premitive operations a lower layer provides to its upper layer; **protocol** is the agreement between two communication peer on how communication is proceed.

## Reference Model

- [OSI Model](#osi-model)
- [TCP/IP Model](#tcpip-model)
- [Hybrid Model](#hybrid-model)
- [OSI vs TCP/IP](#osi-vs-tcpip)

**Reference Models** are essentially architecture of networks, OSI(Open Systems Interconnection) is a good model, very general and still valid; TCP/IP its model is not of much use, but its protocols are still widely used.

### OSI Model

Based on proposal developed by ISO, as the first step to international standardization of the protocols used in the various layers.

Design Principles:

1. A layer should be created where a different abstraction is needed.

2. Each layer should perform a well-defined function.

3. The function of each layer should be chosen, with an eye toward "defining internationally standardized protocols".

4. The layer boundaries should be chosen to minimize the information flow across the interfaces.(PoLP)

5. The number of layers should be: large enough that distict functions need not to be thrown together in the same layer out of necessity; small enough that the architecture does not become unwieldy(oversized).

7 Layers:

- Physical Layer: concerned with transmitting raw bits over a communication channel.
- Data-Link Layer: transform a "raw transmission facility"(physical line) into a "line"(wire) that appears free of undetected transmission error.
- Network Layer: controls the operation of subnet(routing).
- Transport Layer: accept data above it, split it up if necessary, pass to the network layer, make sure pieces arrive correctly on the other side.
- Session Layer: allow peers to estabish sessions between them, including dialog control(turns), token management, synchronization.
- Presentation Layer: concern with syntax and semantics of the information transmitted.
- Application Layer: contains a variety of protocols that commonly needed by users(HTTP).

### TCP/IP Model

A goal is to connect multiple networks in a seamless way, and another goal is the network could survive loss of subnet hardware(connection should be good as long as the source and destination machines are functioning.)

4 Layers:

- Link Layer: describes what links must do to meet the needs of the connectionless internet layer.
- Internet Layer: permit hosts to inject packets into any networks and have themn travel independently to the destination(IP and ICMP).
- Transport Layer: allow peers on source and destination hosts to carry on a conversation(TCP for CO, UDP for CL).
- Application Layer: includes session and presentation layers, contains all the high level protocols(session layer and presentation layer has little use to most applications, proven by the experience of OSI model.)

### Hybrid Model

This one is what discussed in the book and I will conclude in the following pages:

- [Application layer](https://homelabdefense.com/2018/07/27/application-layer/)

- [Transport layer](https://homelabdefense.com/2018/07/27/transport-layer/)

- [Network layer](https://homelabdefense.com/2018/07/27/network-layer/)

- [Media Access Control Sublayer](https://homelabdefense.com/2018/07/27/mac-sublayer/)

- [Data Link layer](https://homelabdefense.com/2018/07/27/datalink-layer/)

- [Physical Layer](https://homelabdefense.com/2018/07/27/physical-layer/)
