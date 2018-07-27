---
layout: post
title: [Internet Background Knowledge] Application Layer
tag: knowledge
---

- [Introduction to Internet Technology](https://homelabdefense.com/2018/07/27/intro-internet/)

- [Transport layer](https://homelabdefense.com/2018/07/27/transport-layer/)

---

Layers below the application layer provides network transport services, but they do not do the real job interacting with users.

Application layer is the layer where the user get to access the network, below are the essential gadgets for the users to use the network easily.

- [DNS](#dns)

- [Email](#email)

- [World Wide Web](#world-wide-web)

- [Streaming](#streaming)

## DNS

**Domain Name Service** interpretes the IP address to the human readable domain name.

It firstly come out when APRNET, back then, all hosts are list in a hosts.txt file, for hundred machines, this works fine, but people realize that for millions of machines, it wouldn't work as good. Host name conflict would happen unless all hosts name are centrally managed. Hence the DNS is introduced.

The Library procedure called is **resolver**.

**ICANN(Internet Corporation for Assigned Names and Numbers)** manages the DNS, it is divided into more than 250 top level domain names.

Second level domain name need to be applied from the **registrars** appointed by the ICANN.

- Cybersquatting is an investment.

- Domain name can be either absolute or relative.

- Domain names are case-insensitive.

### Domain name resource record

Domain name whether is a single host or a top-level domain, can have a set of **resource records**, which constructs DNS database.

- *Domain_name* tells domain to which this record applies, many records exists for each domain.

- *Time_to_live* indicates how stable the record is, highly stable is 86400.

- *Class* indicates the application method, **IN** is for Internet, others may exist but rarely in use.

- *Type* tells what kind of record this is. **SOA** provides the name of primary source of information about the name server's zone.

- *Value* valid value corresponding to the type.

### DNS resolve

- Iterative and recursive resolution.

- Cache is quick but out of date.

- 13 root servers.

- UDP.DNS is used for query.

- root server of a zone is authority.

## Email

Two entity, **user agent** and **message transfer agent(mail server)**.

\* **Mail servers are assumed always on, but user agents are not**.

- Sending new email is **mail submission**.

- Protocol used is **SMTP(Simple Mail Transfer Protocol)**.

- Mailing list is copies of identical mails sends to different receivers.

- Mail would have envelope for basic information which is public.

- Header and body are private, header contains sender info, body is message.

### User agent

User interface. After message be read, user could choose **message disposition**, which are following operation on mail.

**Auto responder** and **Vacation agent** help automatically reply at certain period of time.

**Signature block** is extra feature to append digital signature to ensure the message is valid.

**MIME(Multipurpose Internet Mail Extensions)** defines encoding of non-ASCII message.

Email sending and receiving is in the following order:

- Mail submission. SMTP used with AUTH extension to submit the mail, over TCP channel.

- Message transfer. Using SMTP over TCP channel, relay mail to the final mail server to the receiver's mail box.

- Final delivery. Final delivery is used **IMAP(Internet Message Access Protocol)** with mails remain on the server. It is an updated version of **POP3(Post Office Protocol, version 3)** which would download the mail from server.

## World Wide Web

Webpages uses **Hypertext**, the transfer protocol is **HTTP(Hypertext Transfer Protocol)**. It is viewd in a browser.

If a page shows the same content everytime, it is a **static page**, on the other hand, if a page is generated on demand with data, it is a **dynamic page**.

Each page has a **URL(Uniform Resource Locator)**, it comsists of its protocol, DNS, and a path to the specific page.

The browser gets the web page in the following order:

1. Browser determines the URL.

2. Browser ask DNS for IP address.

3. Browser gets the IP address.

4. Browser make connection through protocol by TCP.

5. Browser send request based on the protocol for the file indicated in path.

6. Server send back file by protocol.

7. Embedded URL are also fetched.

8. Browser display the webpage.

9. TCP connection released if no other requests.

On the server side:

1. Sever accept the connection.

2. Get the path to the page.

3. Get the page.

4. Send the page.

5. Release connection.

Using MIME could allow the browser to open multiple type of files, such as open .pdf in the browser.

For the one supporting MIME server:

1. Resolve webpage name.

2. Perform Access Contorl on the page.

3. Check cache.

4. Fetch page or build it.

5. Determine the rest of reponses.

6. Return response to the client.

7. Making an entry to the server log.

- Coockie is used to identify user, no expire field is non-persistent, expires is persistent.

- CSS is style sheet.

- PHP on server side generates the HTTP.

- Javascript shows dynamic page on client side.

- HTML is HyperText Markup Language.

- For HTTP, GET and HEAD both get data from server.

## Streaming

Including realtime video and realtime audio.

### Audio

**dB(decibels)** is 10lg(A/B)

Audio go through **ADC(Analogue to Digital Converter)** to bits for transmission, and back using **DAC(Digtal to Analogue Converter)**.

### Video

Video is measured using pixels.

- Interlacing is a tech that split picture into two fields, odd and even, each time update half, so the frame rate up to 50. But it is not necessary on computer for the video stream is progressive, and graphics card can buffer.

### Stored Videos

**VoD(Video on Demand)** is the form of streaming.

**RSTP(Real Time Streaming Protocol)** would need to handle:

1. Manage UI.

2. Transmission Error.

3. Decompression.

4. Eliminate jitter.

Low water mark is RTT \* BitRate.

### Real-Time Conferencing

**VoIP(voice over IP)** is the most disruptive.

VoIP has advantages:

- Financial savings.

- Consolidate infrastructure.

- Flexible infrastructure.

- Standard based voice and data.