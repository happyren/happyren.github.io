---
layout:	post
title:	How we are attacked?[To be continued]
tag: penetration
---

How are we attacked? It is a question too hard to answer, but perfect for the introduction. I will list all that I know, and keep this post updated once I got more knowledge.

## Networks

Although the network is eventually a type of IO, due to its well-known to the public, it deserves a special section.

Very popular way to exploit any machine is through network, virus, DDoS, modern attacks happen very often through the network. A lot of attacks could be executed through a network, but I would now focus on three of them:

* Transmitting Malicious Payloads

* Hijacking connections

* Flooding target

### Malicious Payload

Most common one for the client (most of us) is the virus, we saw trojans, rootkit, adware. They may be in an email, a clickable link, a picture, torrent... Back then, hackers destroy things, now, they might spy, steal, or just control our machines using for a botnet.

There is another kind of payload targeting mostly on servers, beyond the virus and the executables, which is code that would be handled by the server. SQL injection, buffer overflow, these attacks happen with the payload are not actually malicious software, but malicious code that could benefit the hacker.

These malicious payloads could be targeted at the most commonly exploited vulnerability, and that's why [Injection continuous to be the most common cyber attack method according to OWASP top 10 2017](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project).

### Hijacking connections

In terms of hijacking connection, it would most likely be Man-In-The-Middle attacks. Either spying on someone and some connection or modify the information being transmit between peers.

This is why we need a VPN, or a Tor to somehow hide our connection to the network, turn as to invisible. Well, it could protect you from scripts, but it doesn't mean you can fully wipe out the trace. So if you need to transmit something really important, don't bother.

### Flooding target

In this one, I would give you the very famous Dos and DDoS attack, which makes it in this article because it comes from the very fundamentals of the network. 

Machines do get overflowed when they cannot handle what they are requested and stop providing service. I will post a research article of mine about DDoS later so you can glance at it. For most of us, the thing we need to worry is that do not become one of the bots.

## No input or output functions?

### No input

Israeli research found out headphone could be reverse engineered and become a microphone. So technically it solves this problem. I am not an expert on this, but the theory could be working. So I would do further research on this and post it.

### No output

Have you ever wondered how to get a file out of vault without bringing it out? **Side-channel attack** is what solves this.

One simple example: using radiation (or heat signature) to passing out information of the secret file in plaintext. This is the example my lecturer used back in undergraduate. Assuming high temp is 1 and low temp is 0, now we could implant a virus, dig the secret, and directly send it out through the heat signature of the computer.

So next time if you notice weird fan spinning up and down, audio peaking, or just a light dot sending **Morse code** at the bottom of your screen, be careful :)

## Social Engineering

Have you ever wondered how to get the access code of a highly secured government institute? Look at them, ask them, and they would tell you.

Yeah right, just like how Sherlock Holmes did in the TV shows, despite how secure a system is, how complex a cryptography is, the very weak point of the chain is human.

Setup your pin as your birthday? Not good. Change 0 to O? Not good. Show off a secret file? Definitely not good.

Believe me, in my final exam in Computer Security, I really would like to answer all the questions with "Social Engineering". So human is the weakest part, let us learn, grow, and enhance it :)