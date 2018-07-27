---
layout: post
title: Data Link Layer[Introduction to Internet]
tag: knowledge
---

- [Introduction to Internet Technology](https://homelabdefense.com/2018/07/27/intro-internet/)

- [Physical Layer](https://homelabdefense.com/2018/07/27/physical-layer/)

---

This layer has three objective:

1. [Providing services to the network layer.](#framing)

2. [Dealing with transmission error.](#error-control)

3. [Flow control](#protocols)

Three acknowledgment scheme:

1. unacknowledged connection-less: Ethernet, because the connection has very low error rate, no need to send ack.

2. acknowledged connection-less: WiFi, because the connection is unreliable, sending ack will reduce the cost to recover message.

3. connection oriented: each frame is received exactly once and all frames are received in the right sequence.

## Framing

Data-link layer is in charge of detecting, or more, correcting the error in the transmitted signal. Normal way is to break the bit sequence and framing them.

A good deisgn is when receiver is easy to find the start of a new frame and the information took up as less bandwidth as possible.

4 possible methods:

1. [Byte count](#byte-count)

2. [Flag byte with byte stuffing](#flag-byte-stuffing)

3. [Flag byte with bit stuffing](#flag-bit-stuffing)

4. [Physical layer coding violations](#physical-layer-coding-violations)

### Byte count

Use a field to count how many byte sending in the packet.

> it is very possible that the bit flipped and the counter gives the wrong bytes, hence leading error.

### FLAG Byte stuffing

FLAG means adding the same byte called **FLAG** to both the beginning and the end of the frame to announce the beginning and ending of a frame.

> it is possible that data contains the bit forms the **FLAG** and **ESC** Whenever sees a **ESC** or **FLAG** add a **ESC** before. PPP is one example

### FLAG bit stuffing

FLAG has the same mining, but bit stuffing inserts bit after 5 \"1\" because \"01111110\" means **FLAG**

both of bit stuffing and byte stuffing would increase the frame size.

### Physical layer coding violation

In physical layer, usually bits are mapped with more signals to safe guard redundency, we could utilizing extra bits to send reserved signals indicates the start and end of a frame, without needs for stuffing.

The popular method is to include a long string called **preamble**, which starts the frame, and then use a field to indicates how long a frame is.

## Error control

1. Ack is normally the way of solving the problem of letting the sender know whether the packet has arrived successfully or not.

2. Timer is used to get rid of the frame lossing trouble, so sender will not wait forever for a ACK of a lossing frame or lossing ACK frame.

3. Assign sequence number to each frame, so that receiver can know whether one frame is received twice.

There are two mean method, adding **error correcting code** and **error detecting code**.

> **error correcting code** or **FEC** is used in noisy channels because resend could likely be errors again.

bit flip is harder than erasure channel.

---

1. [Hamming codes](#hamming-codes)

2. [Binary convolutional codes](#binary-convolutional-codes)

3. [Reed-Solomon codes](#reed-solomon-codes)

4. [Low-Density Parity Check codes](#low-density-parity-check-codes)

**m** reps message length, **r** reps redundant bits, and the encoded code word length is **n = m + r**, code rate is **m/n**.

### Hamming Codes

> Hamming distance is the number of bits positions in which two code words differes.

Firstly, all possible code words should be computed and form a list from which the minimum **Hamming Distance** should be found thus called the **Hamming Distance of whole list**. And it could be called a **Distance x code**

All possible code words should be **2^m/2^n**

With the error of **d**, we are expecting to correct it with a **Distance 2d+1 code** and detect it with a **Distance d+1 code**.

Given **m** bits message, if we wish to correct all single error, we are expecting **n+1** pattern for each of **2^m** pattern, it should be less than **2^n**, henc we have **(N+1) * 2^m <= 2^n**

Message bit **m** is checksumed by the ordering bits, for example, **m3 = p1 + p2**, **m7 = p1 + p2 + p4**

When receiving a message code, it will computes the hamming codes based on the parity, each parity bit is calculated by check the message bit it related to, if comes out odd 1s, then parity bit shall be 1; then, after receiver receives hamming code, it will recompute it, by check the parity including check bits, if all checks give 0, then it is correct, other wise it will pinpoint the error bit.

### Binary Convolutional Codes

No natural message size and encoding boundary as in block codes, output depends on current and previous bits, number of previous bits that output depends on is the constraint length.

A Convolutional codes is specified by its rate and constraint length.

> Widely used in deployed networks, GSM, Satellite, Wi-Fi, NASA [r=1/2, k=7] code is used in Voyager program and reused in Wi-Fi.

For Wi-Fi, there should be 6 memory registers, when 111 input in, 000000 -> 100000 -> 110000 -> 111000, and 7 bits input is required to wipe out all previous input, hence k=7.

| bit 1 | bit 2|
| --- | :---: |
| in | in |
| 2 -> 3 | 1 -> 2 |
| 3 -> 4 | 2 -> 3 |
| 4 -> 5 | 3 -> 4 |
| 5 -> 6 | 4 -> 5 |
| 6 -> out | 6 -> out |

Hence input '111':

1. in = 1, state = 000000, bit 1 = 1, bit 2 = 1;
2. in = 1, state = 100000, bit 1 = 1, bit 2 = 0(1 XOR 1);
3. in = 1, state = 110000, bit 1 = 1(1 XOR 1), bit 2 = 1(1 XOR 1 XOR 1);

> To decode the convolutional code is to find the input sequence that is most likely to generate the output bits. Viterbi Algorithm is used in this codes.

Using Viterbi algorithm error correcting and working with uncertain bits is **soft-decision decoding**, deciding which bits the signal is before error correcting is called **hard-decision decoding**.

### Reed-Solomon Codes

Reed-solomon codes is a linear block code, but it works on byte.

> it works based on the fact tha tany n-degree polynomial uniquely determined by the n+1 points' positions.

This method means, any two signal points sits on a line, we could add two more points on this line as redudant, one signal is error, three are on the line, and the one is not on the line is the error symbol, and we could hence correct the error.

Most popular RS code is (255,223) code, where it could error correct based on byte, 255 = 2^8 - 1, where 223 = 2^8 - 1 - 2 * 16, where 16 is the symbol-error corerecting ability of this code.

> it could be used with convolutional code, and correct both burst and single error.

### Low-Density Parity Check Codes

In this codes, each output is computed with a fraction of input, where matrix rep of a code has low density of 1s.

While decoding, it will use an approximation algorithm to iteratively improves the best fit of the incoming data to a legal codeword.

> Very high error correcting ability, outperform above codes, and widely used in new protocols.

---

1. [Parity](#parity)

2. [Checksums](#checksums)

3. [Cyclic Redundancy Checks](#cyclic-redundancy-checks)

### Parity

Simple idea is that even 1 then add 0, odd 1 then add 1, it can detect single error.

Compare to Hamming Code, for 1,000 bits info block, Hamming Codes requires 10 bits for correction, 1,000,000 bits requires 10,000 bits check. Where with error rate at 10^-6, for every one of the 1000 blocks, 1 will be error, to detect and retransmit it, one extra block of 1001 is required, hence total will cost 2002 bits, is significantly smaller than hamming 10,000 bits.

> We could use interleaving to deal with burst errors.

### Checksums

Calculating a number based on the information and appended to the information, any error will cause the mis-recomputation of the checksum hence leads to error.

### Cyclic Redundancy Checks

CRC is also known as polynomial code, it requires both sender and receiver agree on a generator, then if the receiver receives a code that cannot be fully divide by the generator, it means there is an error.

## Protocols

Data-Link Layer protocols defines how to encapsulate **packet** from network layer to **frame**, and it handles certain amount flow control and error control.

1. [Utopia Simplex Protocol](#utopia-simplex-protocol)

2. [Simplex Stop-and-Wait Protocol for Error-Free Connection](#simplex-stop-and-wait-protocol-for-error-free-connection)

3. [Simplex Stop-and-Wait Protocol for Noisy Connection](#simplex-stop-and-wait-protocol-for-noisy-noisy-connection)

4. [One-bit Sliding Window Protocol](#one-bit-sliding-window-protocol)

5. [Go-Back-N Protocol](#go-back-n-protocol)

6. [Selective Repeat Protocol](#selective-repeat-protocol)

### Utopia Simplex Protocol

This protocol concerns nothing but the actual transfer, is unrealisti scenario.

### Simplex Stop-and-Wait Protocol for Error-Free Connection

Considering flow control to preventing the sender flooding the receiver.

Two major schemes, one is **feedback-based flow control**

> receiver send feedback to allow sender to continue send packets, or tell it how receiver is going on processing packets.

this requires at least a half-duplex channel to be implemented on.

the other one is **rate-based flow control**

> protocol has built-in mechanism to limit the sender's sending rate.

### Simplex Stop-and-Wait Protocol for Noisy Connection

It could be adding timer to the sender, when the ack is not sent to the sender, and the timer is out, the sender will resend the frame. Problem is, scenarios exist that ack is lost so that the frame will be duplicated.

1 bit of seq number is enough for ack this frame and the next frame.

For this protocol, both sender and receiver should remember the next frame's seq, damaged ack is sent the same as the last good ack, and data is sent from the buffer.

### One-bit Sliding Window Protocol

Sliding window protocol with the size of 1, which means all frames received must be in order.

In a duplex connection, if both sides decide to send frame simultaneously, error will occur.

### Go-Back-N Protocol

One-bit will decrease the bandwidth utilization, hence increase the bits so that the sender could (ideally) continuously sending the frames.

Hence we would consider how many frames can fit into the channel as the frames are propagated from sender to the receiver.

This could be calculated by **product the Bandwidth(bits/sec) with the one time propagation delay**, with this value named **BD**, and the max frames to fit inside this channel is **2BD + 1**.

This is the max frames to be coexisted on the channel, due to it ingores the processing time and the ack frame length.

Cumulative Ack means that the incoming of ACK N also ack the N-1, N-2.

### Selective Repeat Protocol

Selective repeat improves the go back N by letting the sender only retransmit the error frame, it will normally send a negative ack to force the sender to retransmit without waiting for the expiry of the timer. And if the NACK is lost, the sender will only send the losing frame again.

After this we are going to [Media Access Control Sublayer](https://homelabdefense.com/2018/07/27/mac-sublayer/).
