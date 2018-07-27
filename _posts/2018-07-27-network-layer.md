---
layout: post
title: Network Layer[Introduction to Internet]
tag: knowledge
---

- [Introduction to Internet Technology](https://homelabdefense.com/2018/07/27/intro-internet/)

- [Media Access Control Sublayer](https://homelabdefense.com/2018/07/27/mac-sublayer/)

---

Network Layer concerns how to get the packet to the destination, is the lowest layer of end to end transmission.

- [Network Layer Design Issues](#network-layer-design-issues)

- [Routing Algorithms](#routing-algorithms)

- [Congestion Control Algorithms](#congestion-control-algorithms)

- [Quality of Service](#quality-of-service)

- [Internetworking](#internetworking)

---

## Network Layer Design Issues

### Store-and-Forward Packet Switching

The major components are the ISP(Internet Service Provider) devices and customer devices. These devices are used as the packet is stored until it has fully arrived and checksum finished, then it will be forward to the next router.

### Service Provided

1. The services should be independent of the router technology.

2. The transport layer shold be shielded from the number, type, and topology of the router.

3. The network address made available to the transport layer should use a uniform numbering plan, even across LAN and WAN.

These leads to the argue that whether the network layer should provide connection-oriented or connectionless services.

For one side, routers' job is moving the packets around, hence connectionless, and for the other side, it is said that a connection ensures quality of services.

### Connectionless

The ISP router decides which is the next stop by a routing table, the routing table gives the next stop by the final destination, and the routing table is managed by the routing algorithm.

### Connection-oriented

Label switching is the machanism used in this service, when a ISP router sees a packet from Host 1 with label 1, it will label it with identifier 1 and route it to the next router, any other connection from other host or head different destination will be assign different identifier.

### Datagram vs Virtual-Circuit

Trade-offes:

1. Setup time vs Address parsing time: Virtual circuit takes longer to setup, but less time on actual address parsing.

2. Destination address used in datagram are longer than circuit numbers used in VC because they have a global meaning.

3. Amount required in router memory.

4. Scenario based connection.

5. Router crashes leads to the loss of connection has significant effect on the connection-oriented service comparing to connectionless service.

---

## Routing Algorithms

There are two processes within a router, one is determine when a packet arrives, what to do with it, this named forwarding; routing is the process that router setup a routing table, update its value, and find the path to transmit the packet!

- [The Optimality Principle](#the-optimality-principle)
- [Shortest Path Algorithm](#shortest-path-algorithm)
- [Flooding](#flooding)
- [Distance Vector Routing](#distance-vector-routing)
- [Link State Routing](#link-state-routing)
- [Hierachical Routing](#hierachical-routing)
- [Broadcast Routing](#broadcast-routing)
- [Multicast Routing](#multicast-routing)
- [Anycast Routing](#anycast-routing)
- [Routing for Mobile Hosts](#routing-for-mobile-hosts)
- [Routing in Ad Hoc Networks](#routing-in-ad-hoc-networks)

Datagram needs routing for every packet, but the VC needs one for setup session, hence it is also named as session routing.

Two kinds of routing algorithms: Adaptive and non-adaptive.

### The Optimality Principle

If J is on the optimal route from I to K, then the optimal path from J to K is also on this path.

### Shortest Path Algorithm

Examine adjcent nodes of current working nodes, assign distance, then, after examine all the adjcent nodes, if the path is the shortest to this node, then made the label permenant, and using the one node with the smallest label as the new working node.

### Flooding

Every packet is sent out on every path except for the one it arrived on.

> Hop counter: it is used to countdown for a fixed length of the packet can be hopped around, it the counter counts down to 0, the packet is discarded. This avoids the infinite hop of a packet in the network.

Another method is to avoid flooding a packet has been flooded by using a packet seq with source machine.

It is good to be used on the broadcast network and very robust.

### Distance Vector Routing

For current working router, its neighbors will report their delay to destination as X, and the router itself holds the delay to the neighbors as m, by X+m, a minimum value will be the shortest route.

> Convergence: Good news propagates quickly, connection lost propagates take a very long time.

### Link State Routing

1. Learning about the neighbours: a router send a special **HELLO** packet on each point-to-point line, its neighbour is expected to send back a reply given its global unique name.

2. Setting Link Costs: cost to neighbour is set automatically or by network operator. \* For largely expanded network, geo-distance could factor in delay, delay calculated by round trip time divided by 2.

3. Building Link State Packets: after information collected, link-state packet is built with **sender**, **sequence**, **age**, and **neighbour and cost**. \* These packets can either be built periodically or when significant event happens.

4. Distributing the LSP: **flodding** can be used, _seq_ is used to discard duplicated packets, _age_ is used to time out down router and prevent packets get lost and live too long. Refinements are, not send out received packets immediately, wait and see if there could be higher _seq_ packet arrives, if so, send out the new one; adding _SEND_ and _ACK_ field to pass the packets to where it needs to go, and acknowledge the sender avoid duplicates.

5. Computing the New Routes: once the full set of LSP is accumulated, the route is computed. From the same routers, there may exists different paths, like from A to B and B to A may be on different path.

Link State Routing needs more computing and space comparing to Distance Vector Routing, however, it does not suffer from convergence problem.

### Hierachical Routing

Hierachical routing divided the routers into regions, each router know its own region but has no idea of other regions. By Kamoun and Kleinrock, the optimal level for an N routers network is lnN.

### Broadcast Routing

Multidestinational routing, to let each packet contains either a list of destinations or a bit map indicating desired destinations.

Another prefered method called reverse path forward, which means if a packet is mean to destined to a router, it will pick on the path the packets normally come from that router.

### Multicast Routing

Multicasting use same scheme as broadcasting, sending packets from a spanning tree, and by refining it, prune the spanning tree with **MOSPF (Multicast OSPF)** and **DVMRP (Distance Vector Multicast Routing Protocol)**. Or it could be using **core based tree** with **PIM (Protocol Independent Multicast)**

### Anycast Routing

A packet is delivered to the nearest member of the group. It is useful when sometime the node want information can be get from anyone not just its connected node.

### Routing for Mobile Hosts

Mobile hosts will tell home agent where it is then home agent would route the packet to it. The local address of the mobile host is called **care of address**, and the technology for home agent to routing the packet with an extra encapsulation is called tunneling.

### Routing in Ad Hoc  Networks

In extrem cases, the router themselves are mobile, they act as both hosts and routers, then the network of routers happens to be near each other is **Ad Hoc Networks**, or **MANETs (Mobile Ad hoc Networks)**. A popular protocol would be **AODV (Ad hoc On-demand Distance Vector)** go through Route discovery and route maintenence.

## Congestion Control Algorithms

Congestion means that the network goes slow, packets delay and loss, or degraded due to the overwhelm amount of packets being transmitted. Two way congestion would happen, one is that the router memory is fulled, but adding more memory would only lead to worse result; another one is the network being too slow, the counter measure is to build up a faster network.

Congestion control is not equal to flow control, one is on the network level, another is about a specific sender and receiver, despite the fact that they can both be handled by slow down the sender.

- [Traffic-aware Routing](#traffic-aware-routing)
- [Admission Control](#admission-control)
- [Traffic Throuttling](#traffic-throuttling)
- [Load Shedding](#load-shedding)

### Traffic-aware Routing

Shift traffic to lightly loaded path, this could be done by including delay and queue into path weight, however, it might lead to oscillate routing table, refinement could be **multipath routing**, where multiple path could be chosen from a source to a destination; another one is shift traffic across the routes slowly enough that it is able to converge.

However, it is not normally used right now to shift the load, but rather slowly changing input, which is **traffic engineering**.

### Admission Control

This is normally used in Virtual-circuit Network, by not allowing VC to setup while the network cannot hold it and its load. By this way, a network needs to be described in terms of its average load and burst load, normally using **leaky bucket** or **token bucket**.

Two common ways reserve enough capacity and how many VC will fit within the network without congestion.

It can be used with traffic-aware routing.

### Traffic Throttling

When detect a incoming congestion, feedback must be given to the sender to throttle the traffic output, handling to problems, one is how to distinguish a congestion is going to happen, the other one is that the router must deliver the timely feedback to the sender may cause the congestion.

#### Chock Packet

Notify the sender of congested packet with a chock packet destinated to the address found in the packet.

#### Explicit Congestion Notification

Tag a forwarding packet with a signal that it is congested, then the receiver will notice and send reply to the sender to let it know it should throttle the output.

#### Hop-by-Hop Backpressure

Affect every hop with **chock packet** hence the load on the congested router can be immediately relife, however, **increase the upstream pressure**.

### Load Shedding

Routers throw away the packets they cannot handle while none of above method does not work.

The problem is to decide which packet to drop, for file transmission, use **wine** because dropping the old one may cause the receiver increase the buffer usage to store the currently unusable new packets; for stream or any real-time application, use **milk** because old and delayed packets are useless.

Application can also mark the packets they send about how important they are.

#### Random Early Detection

Routers maintain a queue length, when itreach the threshold, it will randomly drop packets, and those senders will slow down, and router do not need to explicitly inform the senders.

---

## Quality of Service

Quality of service mechanism maintains the service quality promised as well as keep a low price, and the following four issues must be addressed to ensure the quality of service. There are two versions of QoS, **[Integrated Services](#integrated-services)** and **[Differentiated Services](#differentiated-services)**

- [Application requirements about the network.](#application-requirements)

- [How to regulates the traffic on entering the network.](#traffic-shaping)

- How to reserve resources to increase perfomance.

- Whether the network can behold more traffic.

### Application Requirements

4 parameters defines the QoS flow requirements: **bandwidth**, **delay**, **jitter**, and **loss**.

> The network requirements are less demanding than application requirements when the application can imporve the service provided by the network.

| Application | Bandwidth | Delay | Jitter | Loss |
| :---: | --- | --- | --- | --- |
| Email | Low | Low | Low | Medium |
| File Sharing | High | Low | Low | Medium |
| Web Access | Medium | Medium | Low | Medium |
| Remote login | Low | Medium | Medium | Medium |
| Audio on demand | Low | Low | High | Low |
| Video on demand | High | Low | High | Low |
| Telephony | Low | High | High | Low |
| Videoconferencing | High | High | High | Low |

Network can be divided to different categories of QoS for different purposes:

1. Constant bit rate.

2. Realtime variable bit rate.

3. Non-realtime variable bit rate.

4. Available bit rate.

### Traffic Shaping

**Traffic shaping** is to regulating the average rate and the burst rate of data flow entering into the network. User and ISP would make **SLA(Service Lever Agreement)** to tell the network about the flow pattern of the user. And by monitoring the traffic using **traffic policing**, network would decide whether the user excess the agreement, and its exceeded packets would be marked as low priority or dropped.

### Leaky and Token Bucket

Leaky bucket is like a bucket with a hole in the bottom, if there exist packet in the bucket, it will go out at a constant rate of **R**, otherwise nothing happens; if the queue of the bucket is full, any extra packets are lost. This bucket is like an interface connect the host to the network, exceeded packet either waited until it could be fitted into the bucket or get discarded; the former solution is when a host shaping its flow, the later one happens at hardware of provider on policing the traffic.

Token bucket works like a tab input water/token at rate **R** into the bucket with capacity of **B**, and to send the packets means taking the water out of the bucket, no more than **B** packets can stay in the bucket.

### Packet Scheduling

Allocate router resources for packets of a flow or amoung competing flows, mostly focus on three major component:

1. Bandwidth: not oversubscribing any output line.

2. Buffer size: up to a maximum value, there is always a buffer available when a flow need it.

3. CPU cycles(speed): CPU should not be overloaded so it could process packets timely(quickly).

A easy method is **FIFO**, but it would let one flow eaisly affect the other flows.

A refinement could be **fair queueing** where each flow has a independent queue, however, it provides hosts using large packets more bandwidth. A refinement is to using the transmission finishing time to assign actual byte for a flow in a queue, and send out the packet following the byte queue, but it gives all the flow the same priority.

This is handled by giving the flow more than one byte to weight each flow, hence it is a **Weighted Fair Queueing**. This algoirthem is farther simplified using **deficit round robin**, reduce the complexity to constant from logarithm.

Other algorithms like priority queueing, it creates priority queue for different level, within each level it follows **FIFO**, but it may failed when high priority burst.

Finally, a packet could be carring a timestamp, and it would be sent by the timestamp.

### Admission Control(QoS)

QoS guarantee is established through admission control.

When a user want to send a data flow, the requirement sent along with, hence the ISP could decide whether accept or not based on the current QoS situation. **QoS Routing** is used when the best route to the destination is congested, then a backup route must be picked to meet the QoS.

Because the acceptance is not simply determined by the resources, some application can tolerate occational missed deadlines, and some application could be negotiable, we would need the flow be detailed descripted using **flow specifications**.

Delay would not be exceed a maximum value because a burst could cause a delay in  the first router, but it will also be smooth by the delay, hence it would not affect the whole path.

---

## Internetworking

Different types of network is always needed and necessary, and connect them together will always be a case, hence internetworking is important. Hence we need to address the difference of the networks:

| Item | Some Possibilities |
| :---: | :---: |
| Service Offered | Connectionless vs Connection Oriented|
| Addressing | Different Size, Flat and Hierarchical |
| Broadcasting | Present or Absent |
| Packet Size | Depends on network |
| Ordering | Ordered or Unordered Delivery |
| QoS | Many different kinds |
| Reliability | Different Level |
| Security | Privacy Rules |
| Parameters | Different Timeout |
| Accounting | By connect time, packet |

2 ways could be considered in order to connect different networks: using a device to translate packets from different networks; adding an additional common layer to encapsulate the packets.

- protocol way: TCP/IP protocol, in this way, the data is essentially encapsulated within a IP packet, and then the lower layer packets, when acrossing the boundary, it would be decapsulate and re-encapsulated using a new lower layer protocol.

- device way: routers and gateways. Switches do not need to understand the protocols used by packets, but the routers do, hence the router knows the IP but the switch knows only the MAC.

Tunneling is a good implementation of protocol encapsulation.

### internetworking routing

Routing through different networks is complicated.

Network operators holds different ideas about good route, and they wouldn't share it, and last, the internet is larger than any of the network, hence the routing algorithm need to be in scale.

Within each network, a intradomain protocol is used, and among the networks, interdomain protocol is used.

And since each network rely on its on intradomain protocol, it is called **Autonomous System**.

### Packet Fragmentation

Packet size or **Path Maximum Trasmission Unit** differes for each network. One solution is letting the source know the Path MTU, hence packet size would meet the need in the first place; another one is to break them up when transmitting.

Two fragmentation method, one is transparent fragmentation, the other is non-transpartent fragmentation. Deviated at whether to reassemble packet at intermediate routers.

Then Path MTU discovery is used to find the MTU rather than fragmentation, the disadvantage is it may leads to slow start up time.

### Internet Network Layer

Network layer plays essential roles in internet, top 10 design principles are:

1. Make sure it works.

2. Keep it simple.

3. Make clear choices.

4. Exploit modularity.

5. Expect heterogeneity.

6. Avoid static options and parameters.

7. Look for a good design; it need not to be perfect.

8. Be strict when sending and tolerate when receiving.

9. Think about the scalability.

10. Consider performance and cost.

IP protocol is the one to hold the networks together to form the Internet.

### IPv4

![IPv4 Header](https://ciscocciers.files.wordpress.com/2013/10/5-535.jpg)

- Version field indicates it is IPv4 header, 4 bits.

- IHL indicates the header length, minimum is 5, maximum is 15.

- Type of Service could be integrated service(high QoS) or differentiated service(low QoS).

- Total length use 2 bytes measure the length of the header + payload.

- Identification tells which packet the current frame it belongs.

- DF stands for don't fragment.

- MF stands for more fragments, only the last one of the fragments doesn't have it set.

- Fragment offset tells the sequence of the fragment.

- Time to live limit packet lifetime.

- Protocol indicates transport layer protocol(TCP, UDP, ICMP).

- Header checksum ensures header contains correct address information and so on.

- Source address is the sender's address.

- Destination address is the receiver's address.

- Option field contains additional information for: security, strict source routing, loose source routing, record route, and timestamp.

### Subnet

> Subnet all 0s and all 1s: subnet all 0s are reserved for network, and all 1s are reserved for broadcasting, hence if three bits subnet, total subnet usable are 2^3-2 = 6. [Subnet all 0s and all 1s convention by CISCO](https://www.cisco.com/c/en/us/support/docs/ip/dynamic-address-allocation-resolution/13711-40.html)

**CIDR(Classless InterDomain Routing)** uses IP address aggregation, it aggregate the IP address to a supernet, hence the intermediate routers do not need to know every detailed destinations.

**NAT(Network Address Translation)** indicates that since the IP address is not enough for all the devices, the idea of using private IP address is utilized. For each home a unique public IP address is assigned, then, within each subnet of that home, a preserved private IP address is applied. Packet would be transfered to private IP inbound and public IP outbound. (*192.168.0.0* for example.) The way the transfer is done using the port idea. Using ports to handle the identification of each process that currently using the network.

NAT also has issues and objections from IP community:

1. It violates the architecture model of IP that IP must be unique.

2. It breaks the end-to-end connectivity model of the Internet makes anytime connection seemingly less possible.

3. It makes connectionless network somewhat connection oriented.

4. It makes Network layer assume protocols in Transport layer.

5. It limit the protocol to TCP, UDP, etc.

6. Some application use more ports than one.

7. Ports number is limited.

### ARP & RARP

**NIC** in data-link layer has no idea of IP address. The sender using **ARP(Address Resolution Protocol)** making broadcast to request receiver with the IP, only the receiver with that IP would respond, hence the IP is connected to the Ethernet interface. And this is the vulnerability to be exploited in ARP spoofing.

**RARP(Reverse Address Resolution Protocol)** is the reverse way of providing MAC address and mapping to IP address.

After this, we are going to [Transport layer](https://homelabdefense.com/2018/07/27/transport-layer/).
