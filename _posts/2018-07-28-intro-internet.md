---
layout: post
title: Introduction to Computer Network
tag: fundamental
---

Machines are connected, through network, and as I mentioned in my previous post, it is a very common way for one to exploit machines now and in the coming days. Learn fundamentals about the computer network is essential for security researher.

## Reference Models

Reference models normally indicates the overall architecture of the network, two well-known ones are: **OSI model**, **TCP/IP model**, and we would meet a hybrid model combines both of them.

### OSI model

Proposed by ISO, it comes out as a seven layered model so that each layer should has its own task without being oversized.

Seven layers are:

- Physical Layer: concerned with transmitting raw bits over a communication channel.

- Data-Link Layer: transform a "raw transmission facility"(physical line) into a "line"(wire) that appears free of undetected transmission error.

- Network Layer: Network Layer: controls the operation of subnet(routing).

- Transport Layer: accept data above it, split it up if necessary, pass to the network layer, make sure pieces arrive correctly on the other side.

- Session Layer: allow peers to estabish sessions between them, including dialog control(turns), token management, synchronization.

- Presentation Layer: concern with syntax and semantics of the information transmitted.

- Application Layer: contains a variety of protocols that commonly needed by users(HTTP).

### TCP/IP model

This model has four layers, but it is famous for its protocol stack, it provides TCP, UDP, and IP protocols.

### Hybrid model

Hybrid model shrunk unnecessary layers of OSI model and bring in TCP/IP protocol stacks. Roles of the layers are important.

## Layers

### Physical Layer

Physical layer is the connection media used by the network, two major differences are if the connection is guided.

Ethernet cable, power cable network are guided, which means is connected by wires.

Electromegnatic waves are unguided.

Signals could be intercepted, distorted during transmission.

### Data Link Layer

Data Link layer is the layer immediate above the Physical layer, it in charges of the encoding of the signal. Between 2 peers of connection, it can encode, decode, checksum signals.

Consider it encodes signal into the magnitude of the signal and put it onto the media.

### MAC sublayer

MAC layer assigns unique MAC address which uniquely identify a **Network Interface Card (NIC)**. MAC address could only be known by the directly connected peers.

### Network Layer

Network layer is in charge of routing and congestion control, but what we need to know is routing. It helps determine how to get the packet from the sender to the receiver.

Through this layer, ISP provides evaluation on the Quality of Service.

It also provides an important protocol namely IP protocol. 

### Transport Layer

Transport layer provides efficient, reliable connection for the application layer. Simple speaking, it is Data-Link Layer on the scale of whole network.

**Transmission Control Protocol TCP** is a connection oriented protocol, ** User Datagram Protocol UDP** is a connection less protocol.

### Application Layer

Application layer is the software through which user manage to achieve a function.

Most fundamental Application layer software is DNS, then browser, email.

---

\* Scale of network

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

> For further information, <a href="https://github.com/happyren/Reading-notes/blob/master/Internet.md"> my notes on Internet Technology</a> would be helpful, and also the book Computer Network Pearson.2011. 