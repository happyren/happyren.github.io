---
layout: post
title: Asymetrical Key Cryptography
tag: fundamental
---

Aside from symmetric cipher, modern cryptography has a method to both encrypt a message, and sign a message. The technique used here is called asymetrical key cryptography, or public key cryptography. This article would introduces some basic ideas about the asymetrical key cryptography.

## From number theory

Just like most computer science theroems, public key cryptography has a strong mathematical background. Rooted in number theory, it utilizes the knowledge of Prime number, Fermat's and Euler's theorems.

> Fermat Theorem: $a^{p-1} \equiv 1 \pmod{p}$

This theroem is heavily used in the public key cryptography as it is the foundation of **RSA**.

---

> Euler's Totient Function: $\phi(p) = p-1$

This functino is also used in RSA algorithm, it gives how many numbers co-prime to *prime* p meanwhile under p.

---

> [Miller-Rabin Algorithm](https://www.geeksforgeeks.org/primality-test-set-3-miller-rabin/)

This is a algorithm to test the primality of a given *probable prime* number. Personally I would like to use openssl to generate a huge prime number.

```
	openssl prime -generate -bits 1024
```

---

Learning and be familiar with these mathematical theorems would greatly helps out when learning public key cryptography, they are very basic, I would suggest reading Hardy's *An introduction to the theory of numbers*.

## Public key cryptography

Public key cryptography contains two related key, one public key and one private key.

The cycle starts with chosing keys:

1. Chosing two very large prime number $p,q$ and get the product $N$

2. Picking the public key $e$ so that $GCD(e, \phi(N)) = 1$.

3. Calculate the private key $d$ as the modular inverse of $e$ against $\phi(N)$.

---

After finish the above cycle, you would get the public key and private key, so the encryption could begin:

1. Encryption: $C \pmod N = M^e$

2. Decryption: $M \pmod N = C^d$

---

Above is the public key cryptography encryption scheme work flow, and it could be reversed so works as signature to ensure the message sender's identity:

1. Signing: $S \pmod N = M^d$

2. De-Signing: $M \pmod N =S^e$

However, plain public key encryption or signature is not secure, homomorphism property of the operation exposes severe vulnerability of Chosen Cipher Attack (CCA), Chosen Plaintext Attack (CPA), and faulty key generation may also leads to vulnerability.

---

Here I would like to prove the RSA correctiveness:

### Proof of RSA

First, by *[Fermat's Little Theorem](https://en.wikipedia.org/wiki/Fermat%27s_little_theorem)*, we would have $a^p \equiv a \pmod p$, divide by $a$ on both side would give $a^{p-1} \equiv 1\pmod p$. The target is $(M^e)^d = M\pmod N$ where $N=pq$.

By having $p$, $q$ as two prime number(usually need to be large enough), we would have a number $N$, we store this one, then we need to find two integer $e$ and $d$ such that $ed \equiv 1 \pmod{\phi(pq)}$:
$$
   \because \phi(pq) = lcm(p-1,q-1) = h(p-1) = k(q-1)\\
    \therefore ed - 1 = h(p-1) = k(q-1)
$$

Now, since $p$ and $q$ are both prime number, if we could prove $M^{ed} \equiv M \pmod p$ and $M^{ed} \equiv M \pmod q$, we could prove the target:
$$
    M^{ed} = M^{ed-1}M = M^{h(p-1)}M = 1^hM \pmod p
$$
and:
$$
    M^{ed} = M^{ed-1}M = M^{k(q-1)}M = 1^kM \pmod q
$$

Hence we could prove:
$$
    M^{ed} = M \pmod{pq} = M\pmod N
$$

With this proof, you can see that RSA is mathematically correct, and even given the original infromation that is not co-prime to the **N**, we could still finish the calculation for $M^{ed} = M \pmod N$