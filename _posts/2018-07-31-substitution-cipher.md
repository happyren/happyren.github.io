---
layout: post
title: Substitution Cipher
---

This article would be talking about the a classic encryption technique, substitution cipher. It substitute plain text with other characters so that the cipher text is generated. 

- [Caesar Cipher](#caesar-cipher)
- [Monoalphabetic Ciphers](#monoalphabetic-ciphers)
- [Playfair Cipher](#playfair-cipher)
- [Hill Cipher](#hill-cipher)
- [Polyalphabetic Ciphers](#polyalphabetic-ciphers)
- [One-Time Pad](#one-time-pad)

## Caesar Cipher

**Caesar Cipher** is the earliest known encryption system.

It shifts the character in a rotating fashion, so that each character in the plaintext is shifted the same position in the alphabet.

![Caesar Cipher Example](/assets/img/2018-07-31-substitution-cipher/caesar_cipher.png)

In the example, the plaintext is shifted by 10 using **Caesar Cipher**

## Monoalphabetic Ciphers

**Monoalphabetic Ciphers** uses permutation of the alphabet, creating the key space of 26\! ( factorial(26) ).

![Monoalphabetic Cipher Example](/assets/img/2018-07-31-substitution-cipher/monoalphabetic_cipher.png)

## Playfair Cipher

**Playfair cipher** is a minor upgrade from monoalphabetic ciphers, it uses a 5\*5 table, choosing a keyword, ignore repeat letter, put the keyword from -right to left-, -top to bottom-, then fill the table with the remaining letters in alphabet, with -I and J- in the same cell.

**Four rules** apply to it:

1. Repeating letter in plaintext is filled with filler, eg, balloon -> ba lx lo on.
2. Two letters in the same row replaced by right next letter, circulated left.
3. Two letters in the same column replaced by down next letter, circulated top.
4. Otherwise each letter replaced by letter in its own row and the other letter's column one.

It uses 26 character but has 676 diagrams. Although it is quite complicated, but still a few hundred letters given could help break the cipher.

For example, let's choose the keyword as "Playfair", and plaintext as "cipher"

| 1 | 2 | 3 | 4 | 5 |
|:-:|:-:|:-:|:-:|:-:|
| P | L | A | Y | F |
| I/J | R | B | C | D |
| E | G | H | K | M |
| N | O | Q | S | T |
| U | V | W | X | Z |

plaintext should be divided into pairs: "CI", "PH", "ER". And after encryption: "CI" -> "DR", "PH" -> "AE", "ER" -> "GI"/"GJ"

## Hill Cipher

**Hill cipher** utilizes the property of Matrix and inverse Matrix.

M(M<sup>-1</sup>) = I

for example, generating a key of 3 by 3 matrix M with inverse matrix M <sup>-1</sup>, divide the plaintext into 3 letter block P:

P\*M -> C; C\*M <sup>-1</sup> -> P

and it is because P \* M(M <sup>-1</sup>) = P \* I = P

## Polyalphabetic Ciphers

**Polyalphabetic ciphers** uses different monoalphabetic ciphers throughout the whole plaintext, it has:

1. A set of monoalphabetic ciphers.
2. A key to determine which cipher to be used.

One of the most simple and well known example would be -Vigenere Cipher-:

P contains n letters, K contains m letters

C = E(K, P)

C <sub>i</sub> = (P <sub>i</sub> + K <sub>i mod m</sub>) mod 26

P <sub>i</sub> = (C <sub>i</sub> - K <sub>i mod m</sub>) mod 26

> due to the property of modular calculation

## One-Time Pad

**One-Time Pad** is promised to be unbreakable. Because each key is used only once, so that even if the key is acquired, it only apply to one ciphertext. An example is [Cloudflare's LavaRand](https://blog.cloudflare.com/lavarand-in-production-the-nitty-gritty-technical-details/)

> <a href="/assets/download/2017-07-31-substitution-cipher/substitution-ciphers.zip">Code available for download(including hill cipher)</a> and <a href="/assets/download/2017-07-31-substitution-cipher/substitution-ciphers.zip.gpg">Signed</a>