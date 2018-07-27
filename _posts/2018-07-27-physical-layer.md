---
layout: post
title: Physical Layer[Introduction to Internet]
tag: knowledge
---

- [Introduction to Internet Technology](https://homelabdefense.com/2018/07/27/intro-internet/)

---

- [Nyquist Theorem](#nyquist-theorem-maximum-data-rate-of-a-channel)
- [Guided Media](#guided-media)
- [Unguided Media](#unguided-media)

Firstly, using electronics cannot generates actual digital signal directly, because it exists break point, which violates the natural law of electricity. So instead, digital signal is decomposed into different possible signals, which is harmonic signal (sin and cos) with different frequency.

From the actual transmission, some frequency of signal may be attenuated, so there exist a cutoff frequency that defines how many frequencies are actually useful while transmitting, and from the lowest frequency to the highest frequency of the signal components are called bandwidth.

There could be baseband and passband, however, it is irrelevant with bandwidth, using baseband and passband, signals with different bandwidth could actually share the same channel by multiplexing.

And with higher bandwidth, a more accurate frequency could be reproduced by receiver, hence more bits could be transfered at once, hence higher bandwidth means higher bit rate.

> The first harmonic is portraited as a 8bits chunk transmit on a b bits/sec data rate, hence the first harmonic freq is 1/8/b Hz = b/8 Hz.

Bandwidth has different definition in terms of electrical (Hz) and CS (bits/sec). Bandwidth in CS means the maximum data rate let b/8 < Bandwidth (Hz).

## Nyquist Theorem -- Maximum Data Rate of a channel

For bandwidth B, the sampling rate should be at least 2B to reconstruct the original signal and more than 2B is pointless due to the fact that the higher frequency is filtered out by the low pass filter using to limit the bandwidth.

> Nyquist theorem gives noiseless channel: Max(Data Rate) = 2BlogV; Shannon theorem gives noisy channel: Max(Data Rate) = BlogS/N, or if the S/N is in dB, Max(Data Rate) = B\*dB/3.

## Guided media

### Magnetic media

Write data into magnetic media, aka DVD, physically transmitted them to the destination.

> This method is very cost effective! Used for applications requires high bandwidth and cost per bit, and don't request low delay.

### Twisted Pairs

Two copper wires twisted into one, forming an attena, cancelling out wire radiates, transmitting signal using the voltage difference between two wire, hence less affected by noise, examples could be telephone and ADSL.

> It could achieve several megabits/sec, it has low cost and can go online.

There are some topologies:

1. Links can be used in both direction at the same time is called **full-duplex**.

2. Links can be used in bi-direction but only one at a time is called **half-duplex**.

3. Links can only be used in one direction is called **simplex**.

### Coaxial cable

![Coaxial Cable](https://upload.wikimedia.org/wikipedia/commons/thumb/f/f4/Coaxial_cable_cutaway.svg/2000px-Coaxial_cable_cutaway.svg.png)

It is better sheilded hence run on higher bandwidth with longer range.

50ohm one is normally used for digital transmission, while 75ohm one is normally used for analog transmission and TV signals(historical reason: early antennas had 300ohm impedance with 4:1 impedance-matching transformers.)

> It has high bandwidth and good noise immunity, but gradually replaced by optic fiber and long-haul routes.

### Power lines

Initially used by power company for remote metering, now reused both in home as LAN and outside home as broadband Internet connection.(Mostly in home)

Difficulties:

1. Wiring normally setup for 50-60Hz electricity and will attenuate high freq signals.

2. Without careful twisting the twisted pairs, the wire will act like an antenna.

> Despite its disadvantage on transmitting high freq signal and bad noise immunity, it is still possible to transmit over 100Mbps signal over power lines, but requires better standards.

### Optic fiber

Optic fiber is capable transmitting at extremly high speed, tens Tbps, however, it is limited by converting a electrical signal into optic signal.

Optic fiber transmission requires: light source, transmission medium, detector.

> optics with light bouncing inside is multi-mode, without that bouncing is single-mode.

Optics need either connectors, mechanically spliced, or melted together.

> optic fiber has higher bandwidth, lower attenuate, better noise immunity, lower weight, lower installation cost, better security; but it also requires better engineering, more protection, and extra bi-directional design.

## Unguided media

The modern wireless digital communication began in the Hawaiian Island - -

The basic of unguider media is electromegnatic wave, caused by the movement of a electron. It has a oscillation freq and wavelength where their product shall be light speed in vacuum.

![Electromegnatic Wave Spectrum](http://www.shieldingsystems.com/images/electromagnetic_spectrum%20.png)

Wireless communication normally transmitting in a narrow band of frequency, however, it can also use wide bandwidth:

1. Frequency hopping spread spectrum: transmitter jump from freq to freq, usually by military for harder detection.

2. Direct sequence spread spectrum: use a code sequence to spread the data to a wide bandwidth, for example, CDMA (Code Division Multiple Access).

3. Ultra Wide-Band: sends a series of rapid pulses, varying their position to communicate information.

### Radio Transmission

Omnidirectional, transmitter and receiver do not need to align perfectly. And comparing to guided media, RF can travel longer distance, but interference between users is a problem.

### Microwave Transmission

Microwave travel in straight line, so repeater is needed to avoid earth. And signal may be delayed due to passing lower atmosphere, called **multipath fading**

> It has advantages over fibers, because it does not take up a lot of place to embed the cable, only towers are required, and it is relatively inexpensive.

### Infrared Transmission

For short range communication.

### Light Transmission

LANs from two buildings could be connected via the laser on the top of both building.

> High bandwidth, low cost, and secure, although it is eaisly distorted, it could be used in vacuum in space programs.

### Geo-stationary Satellite

There could be maximum 180 Geo-stationary satellite. Through development, footprint of satellite is shrank, from 1/3 sphere, to spot beam. And require station to transmit signal to local, from large one to small ones called VSATs.

VSATs requires ground hub to transmit signal using high power antenna.

> very useful when connecting rural area using VSATs, signal transmit faster because the medium is air, but not very secret, however, cost does not grow as distance, and error rate is low.

### Medium-Earth Orbit Satellites

GPS.

### Low-Earth Orbit Satellites

Gound stations don't need much power, delay is small, only ms around the earth, and price is relatively low.

### Overall

Mostly, fibers are winning, but satellites have pros:

1. deploy could be very fast.

2. connection at rural area is achieved.

3. more suitable for broadcasting.

After this we are going to [Data Link Layer](https://homelabdefense.com/2018/07/27/datalink-layer/).
