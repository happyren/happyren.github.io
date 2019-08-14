---
layout: post
title: How lazy mistake cause cryptographical failure(ECDSA)
---

In many cases, the deployment of cryptographic algorithms fail on their purposes, some may blame to the algorithms, but some comes from misconfiguration and lazy mistakes(where you don't crack the algorithm but still crack the cryptography). In this article, we would briefly introduce how a "should be random" variable leads to failure of ECDSA.

## What is ECDSA

**ECDSA** is short for Elliptic Curve Digital Signature Algorithm. Put the DSA aside, Elliptic Curve is a brilliant finite group where DLP is super hard. In fact, its way more efficient than RSA where 256 bits ECDSA has the same computational complexity compares to RSA with 3072 bits.

![secp256k1](/assets/img/2019-03-09-lazy-mistake-ecdsa/Secp256k1.png)

This is the figure of a elliptic curve group which is used by bitcoin. ( Secp256k1 :  y <sup>2</sup> = x <sup>3</sup> + 7 )

ECC uses EC as a finite group. After choosing a generator, and 2g, 4g, ... are finding as shown in the below figure. (tan is tangent)

![secp256k1](/assets/img/2019-03-09-lazy-mistake-ecdsa/ecc.png)

When selecting a random private key d<sub>a</sub>, public key is a point Q<sub>a</sub> = d<sub>a</sub>g, from which is computationally infeasible to regain the private key.

[Digital Signature is well explained in wikipedia](https://en.wikipedia.org/wiki/Elliptic_Curve_Digital_Signature_Algorithm)

One thing to be noticed, whenever selecting a random number, make sure it is really random and cryptographically secure.

**Random! Random! Random!**

## The lazy mistake

When people get lazy and not selecting **the "should be random" variable** really random, two signatures could expose the private key.

That variable is **k**, which in turn generates temper key (x, y) = kg. If 2 signature shares the same k, it indicates they have the same value of x in turn r(part of the signature).

So given two signature (s1, r1) and (s2, r2), if r1 = r2:

k = ((H(m1) - H(m2)) mod n / (s1 - s2) mod n) mod n

d<sub>a</sub> = ((sk - H(m1)) mod n / r1 mod n) mod n

**Let's put aside the arithmatics and bring on the code.**

```ruby
s1 = some Integer
r1 = some Integer
m1 = some Message
z1 = Digest::SHA2.hexdigest(m1).to_i(16)

s2 = some Integer
r2 = some Integer
m2 = some Message
z2 = Digest::SHA2.hexdigest(m2).to_i(16)

if r1 == r2

    puts "the signature is vulnerable"
end

k = ((z1 - z2) % n * modinv(s1 - s2, n)) % n

da = ((((s1 * k) % n) - z1) * modinv(r1, n)) % n
```

## Wrap up

This is not the only issue caused by lazy mistake when applying cryptography, POODLE is another one which failed on using HMAC properly to check message integrity.

So the thing we learn today is, don't be lazy, **Random! Random! Random!**
