---
layout: post
title: [Internet Background Knowledge] Transport Layer
tag: knowledge
---

- [Introduction to Internet Technology](https://homelabdefense.com/2018/07/27/intro-internet/)

- [Network layer](https://homelabdefense.com/2018/07/27/network-layer/)

---

Transport layer in order to provide efficient, reliable, and cost-effective data transmission service to user throught application layer.

Transport layer software and hardware are called transport entity.

Connection-oriented and connectionless transport layer protocols are very like the counterpart in the network layer, only the connectionless transport layer is inefficient when setting upon a connection-oriented network layer.

> The difference between the **Transport Layer** and **Network Layer** is the former one entirely run on the user's machine. It reassure the QoS on top of the Network Layer when it doesn't.

The other reason is network layer has significantly different calls from one to others, so hiding it behind the transport layer provides easiness for application layer.

Transport layer intends to hide all the inperfection of the network so that the user could assume the network transfers error free bits.

- [Transport Layer Services](#transport-layer-services)

- [Transport Layer Protocols Design](#transport-layer-protocols)

- [Congestion control](#congestion-control)

- [User Datagram Protocol](#udp)

- [Transmission Control Protocol](#tcp)

## Transport layer Services

| Primitive | Packet Sent | Meaning |
| :---: | :---: | :---: |
| LISTEN | (none) | Block until some process want to connect |
| CONNECT | CONNECTION REQ | Actively attempt to establish a connection |
| SEND | DATA | Send information |
| RECEIVE | (none) | Block until DATA units arrive |
| DISCONNECT | DISCONNECTION REQ | Request a release of the connection |

> Symmetric connection: connection could be fully disconnect only when both side DISCONNECT.

### Socket Primitives

This is the service primitives for TCP connection.

| Primitive | Meaning |
| :---: | :---: |
| SOCKET | Create a new communication endpoint |
| BIND | Associate a local address with a socket |
| LISTEN | Announce willingness to accept connections; give queue size |
| ACCEPT | Passively establish an incoming connection |
| CONNECT | Actively attempt to establish a connection |
| SEND | Send some data over the connection |
| RECEIVE | Receive some data from the connection |
| CLOSE | Release the connection |

> 1. Connection release of socket is also symmetric.
> 2. Reliable byte stream: socket API combined with TCP to provide connection-oriented service.

## Transport layer protocols

It handles similar problems as to data-link layer in different ways:

1. They operates on different level, for data-link layer connects two machine end-to-end, the transport layer handles over the whole network.

2. The connection is simple to establish in the data-link layer since both ends always there.

3. Network has storage in it while the physical layer does not.

4. Issue is more complicated in the transport layer than in data-link layer.

### Addressing

In transport layer, the access point is **port**, it is called **TSAP(Transport Service Access Point)**.

To get the port for application, a **portmapper** is normally used. User would connect to the **portmapper** and then it would request a port, and connect the application to that port.

**Initial connection protocol** is when the server service may not listen to the port, a process server *inetd* would be listen to it and when no service respond to connection, connection would be pass to it.

### Connection Establishment

The problem could happen when the network can lose, delay, corrupt, and duplicate packet.

One solution is to setup new connection for every transaction, so the duplicated packet cannot find the destination. But it is wasteful and complicated.

The other one is to setup a sequence number, with obsolete sequence number, the packet cannot be accepted. But it would require the transport to hold the list of the sequence number.

In real world, the packet life time is used to kill the packet, it could be restricted by:

1. Restricted network design.

2. Putting a hop counter in each packet.

3. Timestamping each packet.

In practice of Internet, we will kill all the packets and all of the ACKs, hence we use a **T** period, which in Internet case, is 120s. Then the SEQ of each packet should be unique within this **T**.

However, what if the host crashes. Then it needs a global clock, and let it represent the SEQ. It is a bit counter with bits equals or exceeds the bits of SEQ, and it would continue running even if the host shuts down. To utilize this, the sender must send following the clock, that is to reduce data rate. And with clock rate **C** and sequence number space **S**, **S/C>T** must holds.

With the clock method, we can distinguish a new segment from a recent duplicated segment, but we still cannot tell a CONNECT REQ from a recent duplicated CONNECT REQ. Hence introduce **three-way handshake**.

> Three-way handshake: the establishment of the connection needs one peer to check with the other whether the REQ is current.

Host 1 CONNECTION REQ with its own SEQ *seq1*, HOST 2 ACK *seq1* and attach its own SEQ *seq2*, and HOST 1 ACK *seq2* with the first data segment.

**TCP** uses this improvement with adding timestamp to the SEQ so that it will never wrap up, due to the fact **TCP** is used on very fast networks, its name is **PAWS(Protection Against Wrappped Sequence numbers)**.

### Connection Release

Asymmetric release could cause data loss.

Symmetric release could solve the problem, however, the **two army problem** is difficult to be solved.

It means if neither sides is convinced that the disconnect will happen for the other side, the disconnect will never happen.

Buffer is considered an important part of the transmission for it is needed to store the data temporarily. Three approaches are:

1. Static sized buffer, good when segments sizes are uniform.

2. Dynamic sized buffer, utilize memory better, but request complicated memory manager.

3. Single large circular buffer per connection, makes good use of memory only when the connections are heavily loaded.

Buffer allocation problem is normally arised with datagram since the connectionless network causes more packet loss.

### Multiplexing

Multiplexing is sharing several conversations over connections, virtual circuits, and physical links.

Only one network address is available, multiple operation needs to have cvonversations through that address, this is called **multiplexing**.

Another example would be the user needs extra bandwidth, so one operation holds conversation through multiple network address, it is **inverse multiplexing**, a protocol called **SCTP(Stream Transmission Control Protocol)** utilizes inverse multiplexing.

### Crash recovery

If routers crashes, and the connection need to be long lived, then issue happens.

Crash recovery is hard because each layer has no way to know what status it in, for **N** layer crash, only **N+1** layer could help do the recovery.

And a truely end-to-end acknowledgement is almost impossible to achieve.

## Congestion Control

The Internet relies heavily on the Transport layer to take congestion control.

A good congestion control can not only avoid congestion, but also properly allocate the bandwidth so that the performance could be improved, and bandwidth could be fairly utilized.

If transport layer protocol is poorly designed, and the retransmitted packet is delayed but not lost, the network could be congestion collapse.

**Power** is introduced since as the load increase and approach to the capacity, the delay would increase dramatically, hence the **power = load / delay**, which the max power gives the most efficient load.

### Max-min fairness

This is a discussion about how to divide the bandwidth among the different senders.

This happens when in order to increase the bandwidth of one flow, the bandwidth of others must be decreased since no extra bandwidth is available.

### Bandwidth allocation convergence

This means that despite the network bandwidth allocation would converge to a static arrangement eventually, the network is actually dynamic. So the algorithm should help the allocation converge at the right point at all circumstances.

### Regulating sending rate

The sending rate is regulated by the transport protocol using **control law** using feedback, such as *explicit precise feedback*. **AIMD(Additive Increase Multiplicative Decrease)** is an appropriate control law.

Additive Increase means 45 degree increase rate, multiplicative decrease means decrease towards the origin.

And since TCP is currently dominant form of congestion control, **TCP-friendly** congestion control is introduced.

## UDP

**UDP(User Datagram Protocol)** as stated in name, is a connectionless protocol.

![UDP header](https://nmap.org/book/images/hdr/MJB-UDP-Header-800x264.png)

It holds the length of both UDP header and the data, minimum 8 bytes header and maximum 65515 bytes wihtin the 16 bits length offset limitation.

Checksum checks the header, data, and pseudoIPheader.

The pseduoIPheader includes both source and destination IP address, UDP protocol code 17, and UDP segment count(with pesudoheader).

The UDP protocol only provides interface to demultiplexing using ports and adding end-to-end error detection.

### Remote Procedure Call

Process 1(caller) on localhost calling process 2(callee) on remote host, after the caller made the call, it would suspended, and then the callee would process the call, and process result would later come back to the caller.

The client and the server would be bounded with the available procedure calls **client stub** and **server stub**. The whole process coming in below:

1. Client calls the client stub procedure.

2. Client stub packing the procedure call in to message, called marshaling.

3. Client OS sending the message to the remote OS.

4. Remote OS passing the message to the server stub.

5. Server stub unmarshal the message and executes the call.

However, there are problems with RPC, one is pointer parameter may not be pointing to the same thing on different machines; the second is that with weakly typed languages, the marshalled message size may not be specified in the procedure calls; the third problem is that it is not always possible to deduce the parameter type, even form a formal specification or code itself; the fourth one is the global variable is not shared.

### Real-Time Transport Protocol

**RTP** is designed for real0time multimedia application.

It works as follow:

1. The multimedia application consists of multimedia streams.

2. These streams are fed into the RTP library.

3. The library multiplex streams and encode them into RTP packet and then stuff them onto socket.

4. UDP segment is created with the media socket, pass down to the IP and eventually the ethernet.

5. Reverse process is done on the receiver and then multimedia application would play out the multimedia data.

Each RTP packet would have a SEQ higher than the previous one to indentify if a packet is missing. How it might be handled is fully depend on application layer.

It would contains the encoding and sample of payload, as well as the time stamping which is essential to the application.

RTP header contains following fields:

- Ver.: **RTP is now in version 2**.

- P: indicates the data is padded.

- X: means including extension header.

- CC: is the number of contributing sources, 0-15.

- M: application-specific marker, mark start media.

- Payload type: encoding.

- Seq: missing detection.

- Timestamp: first frame timestamp.

- Synchronized Souce Id: which stream this packet belongs to.

- Contributing Souce Id: Identified used mixer source.

## TCP

It provides reliable end-to-end byte stream transmission over an unreliable network.

TCP is extended based on RFC 793 plus, clarification and bug fixes in 1122; extension for high-performances in 1323; selective ACK in 2018; congestion control in 2581; repurposing header field for QoS in 2873; improved retransmission timers in 2988; explicit congestion notification in 3168.

Each TCP supported machine must have a TCP entity, either library procedure, user process, or a part of kernel, with the same purpose to manage TCP stream and using network layer interface.

TCP would make sure the transmission quality while lower layer makes no guarantee on the transmission.

### Service Model

TCP is based on sockets, each holds IP address + port number. More than 1 connection could use one socket.

In terms of addressing, ports below 1024 are reserved for [well known port](https://en.wikipedia.org/wiki/List_of_TCP_and_UDP_port_numbers).

Applications may choose their ports within the range 1024-49151.

For connection to be initiated, a process, normally daemon would be needed, normally a general *inetd(Internet Daemon)* would be listen to multiple ports. Which acts as a process server.

- TCP is full duplex point-to-point.

- Each TCP connection is byte stream rather than message stream.

- TCP has no idea what a byte means in a file.

- TCP may choose to buffer or send a message at its on discretion.

- **Urgent Data**, depite rarely used, means the data has highest priority to be immediately process by TCP.

### TCP protocol

Key is that each byte stream in TCP connection has its own SEQ, for it to carry through to receiver and back with ACK.

- TCP has minimum 20bytes length which is only header.

- Each segment must fit in IP packet thats 65,515 bytes, and also fit in link **MTU** normally 1500 bytes.

- TCP uses sliding window with a dynamic window size.

### TCP header

![TCP header](https://intronetworks.cs.luc.edu/current/html/_images/tcp_header.svg)

- Source and Destination ports.

- TCP SEQ for sequence identification.

- **ACK next expected income SEQ**.

- Length of data and header.

- Next 4 bits are reserved, shows how well TCP designed.

- CWR and ECE for congestion signal. **ECN(Explicit Congestion Notification)** is used, and **ECE(ECN-Echo)** would tell sender to slow down, **CWR(Congestion Window Reduced)** notify the receiver that sender slows down and stop sending **ECE**.

- URG is urgent pointer.

- ACK = 1 means ACK number field valid, without it, segment does not carry a ACK.

- PSH is remind receiver to push data received without buffering.

- RST is enforced reset.

- SYN is used to establish connection.

- FIN is used to release connection.

- Window size indicates how many available bytes remain, and could be sent these much data after the last ACK data.

- Checksum is for reliability.

- Urgent pointer indicates how many bytes offset from current position, the urgent data could be found.

- Option field is for extra facilities, a well known one is **MSS(Maximum Segment Size)**, another one is **Timestamp**, **SACK(Selective Acknowledgement)** tells the sender which range of segment is received.

### Timer management

4 Timers are used by TCP:

- Retransmission Timeout: check if retransmission is needed by ACK arrival.

- Persistence Timer: Prevent following deadlock, if receiver's window size non-0 segment lost.

- Keepalive timer: Check if the other side is still on.

- TIME WAIT, make sure all packets from oneself go off after the connection is closed.

### TCP Congestion Control

TCP maintain a congestion window, size equals the bytes sender may have in the network.

Congestion window is managed in addition to the flow control window, hence even the receiver says "Send 64bytes" and the sender knows 32bytes burst would block network, it would send only 32 bytes.

Now idea is TCP assume packet lost is due to congestion.

- **ACK clock** means sender use 4 segments to test the receiver network speed, and sending packet at this rate would max out the receiver network without congesting the router.

- **Slow start** indicates the sender would initialize a small congestion window, then when the receiver returns ACK, the sender know no congestion, and increase the congestion window by 1 MSS, essentially each RTT double the size. Slow start threshold is used to detect the congestion, when it reaches, the window size would be cut half, later, the window size increase by 1 segment.

- **Duplicated ACK** is the receiver obtain a packet beyond the lost packet, it would send back ACK with previous SEQ, so when sender receive duplicated ack, it knows a lost occurs.

- Inreality, TCP would consider three duplicated ack means a lost and the lost one is ack+1 packet, hence the lost packet get sent immediatly, this is **fast retransmission**.

- Duplicated ACK could be used to count the packet in the network. Utilize this feature, **fast recovery** would let the packet in the network drop to the congestion window.

- **ECN** flag allows router tells the receiver when congestion is approaching, receiver tells sender by using **ECN-Echo**, sender ack receiver by **CWR**.

After this, we are going to [Application layer](https://homelabdefense.com/2018/07/27/application-layer/).