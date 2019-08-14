---
layout: post
title: Steganography 󠁦󠁬󠁡󠁧󠁻󠁨󠀰- 󠁭󠀰󠁧󠁬󠁹󠁰󠁨hide 󠁟󠀱󠁳󠁟󠁡󠁷󠀳the 󠁳󠀰󠁭󠀳󠁽secret
---

The action of consealing a file, a message, a secret has been there for a while, a while so long that dated back to 440 BC as the _steganography_ has first been recorded by Herodotus. While cryptography also hide information, the steganography prevails as it does not draw attentions. This article intends to introduce this keyword so you can have something to look for when necessary.

Representing "consealed writing", steganography has been used in wars since the acient Greek.

People have hidden information in many different ways:

- Invisible ink
- Under the postage stamp
- Microdot photography

They appear in spy movies, detective stories. And they are effective without using any cryptography which generally causing suspecious.

## Digital steganography

In the digital era, the way of steganography just go wider. Data can be conveyed in so many ways, as _invisible encoding_, _bits difference in pixel color space_.

Below are two images, the upper one is the carrier (original image), the bottom one carrys a secret image which is "I will hide this secret!" It is clear these two images are identical to human eyes, hence without causing any suspecious, the secret can be passed around.

![Original Carrier Image](/assets/img/2019-04-10-steganography-hide-the-secret/input.png)

![Stegged Carrier Image](/assets/img/2019-04-10-steganography-hide-the-secret/new.png)

Code for this demo comes with [very detailed explaination by Andrew Scott](https://hackernoon.com/simple-image-steganography-in-python-18c7b534854f).

\*Quest: There is a **flag** hidden in the **title**, can you retrieve it?

By now, you might have already grab this keyword and know what it means, there are plenty of methods under this catergory. Remeber to check **steganography** when you have no idea how to extract a hidden message.

[Demo could be downloaded here with image file and payload file](/assets/download/2019-04-10-steganography-hide-the-secret/steg.tar)