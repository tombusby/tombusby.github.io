---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 5 Solutions - Ex5.9"
description: "Discussion of the Number of IV Bits Available in AES CTR (Counter) Mode for 1TB of Plaintext"
layout: post
redirect_from: /understanding-cryptography-ex5.9/
headerImage: false
projects: false
date: 2018-05-13 15:41
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 5
exercise: 9
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 5.9

We are using AES in counter mode for encrypting a hard disk with 1 TB of capacity. What is the maximum length of the IV?

*Note*: TB normally refers to a Terabyte (which is $$10^9$$ bytes). This answer does not coverge with the answer in the solution manual unless you define 1 TB as 1 Tebibyte (TiB), which is $$ 2^{40} $$ bytes.

### Solution

*This solution is verified as correct by the official [Solutions for Odd-Numbered Questions](http://wiki.crypto.rub.de/Buch/en/download/Understanding_Cryptography_Odd_Solutions.pdf) manual.*

Asking what the maximum length of the IV can be is equivalent to asking for the minimum length of the counter value bits. The total number of bits to be input into the cryptographic primitive is 128. As such, we need to calculate how many counter values we need to encrypt 1 TiB of data. This will allow us to calculate the number of bits needed for the counter. Subtracting this number will give us the maximum number of bits available for the IV.

First we work out how many blocks are needed:

$$ 2^{43}\,\mathsf{plaintext bits} \div 128\,\mathsf{bits/block} \\= 68,719,476,736 \,\mathsf{blocks} $$

*Note*: It's very easy to fall into the trap that there are $$ 2^{40} $$ bits of plaintext. There are in fact $$ 2^{40} \times 8 = 2^{43} $$ bits of plaintext, since 1 TiB is $$ 2^{40} $$ __bytes__.

We will need one distinct counter value for each block, so we take this value $$ log_2 $$ to work out the number of bits required for the counter:

$$ log_2 68,719,476,736\,\mathsf{blocks} = 36\,\mathsf{bits} $$

The number of bits required for the counter is therefore 36. These bits are also used to full capacity. One more bit of plaintext to be encrypted and we'd need another counter bit.

As such, the final thing to do is substract this from the total number of input bits available to determine the maximum number of IV bits that allows encryption of 1 TiB without cycles in the keystream.

$$ 128\,\mathsf{bits} - 36\,\mathsf{bits}= 92\,\mathsf{bits} $$

As such, the maximum number of bits available for use as IV is 92.

{% include _understanding-crypto/previous-and-next-exercise.html %}
