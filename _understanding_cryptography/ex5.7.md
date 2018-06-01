---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 5 Solutions - Ex5.7"
description: "Weakening an OFB Mode Scheme by Truncating the Feedback Bytes"
layout: post
redirect_from: /understanding-cryptography-ex5.7/
headerImage: false
projects: false
date: 2018-05-12 20:40
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 5
exercise: 7
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 5.7

As is so often true in cryptography, it is easy to weaken a seemingly strong scheme by small modifications. Assume a variant of the OFB mode by which we only feed back the 8 most significant bits of the cipher output. We use AES and fill the remaining 120 input bits to the cipher with 0s. 146 5 More About Block Ciphers

1. Draw a block diagram of the scheme.
2. Why is this scheme weak if we encrypt moderately large blocks of plaintext, say 100 kByte? What is the maximum number of known plaintexts an attacker needs to completely break the scheme?
3. Let the feedback byte be denoted by FB. Does the scheme become cryptographically stronger if we feedback the 128-bit value FB,FB,...,FB to the input (i.e., we copy the feedback byte 16 times and use it as AES input)?

*Note:* "[T]he OFB mode" referred to above, upon consulting the solution manual, appears to refer to the standard OFB mode, not the one we defined in the last exercise.

### Solution

*This solution is verified as correct by the official [Solutions for Odd-Numbered Questions](http://wiki.crypto.rub.de/Buch/en/download/Understanding_Cryptography_Odd_Solutions.pdf) manual.*

1\. Below is an OFB scheme which matches the requirements:

![OFB Scheme]({{ site.url }}/assets/images/understanding-crypto/ex5-7-broken-OFB-mode-scheme.png){: .image-center}

*Note*: In places where 128 bits are truncated to 8 bits, this is done by taking the first 8 bits (and where necessary, right-padding with 0s prior to input into the cryptographic primitive).

2\. The scheme is so weak because there are now only $$ 2^8 = 256 $$ possible outputs from the cryptographic primitive. This also means that the maximum sequence length of keystream blocks is also 256. In reality, it's likely that the cycle is even shorter since it may encounter a previous state long before producing 256 outputs. Each of these 256 possibilities will encrypt 128 bits of plaintext.

As such, the theoretical maximum number of bytes which can be encrypted before encountering a cycle is as follows:

$$ 128\,\mathsf{bits} \times 256\,\mathsf{possibilities} = 32,768\,\mathsf{bits} $$

This is exactly 4 KiB. Thus, an attacker has to know at most 4 KiB of plaintext before he can decrypt the entire message (since the keystream will repeat, and he knows what it will be).

*Note:* I believe that the solutions manual contains an error here ($$ 128 \times 16 $$ should be $$ 256 \times 16 $$) and would be very grateful if someone else could either confirm that or explain my mistake.

3\. This will not increase security, since there will still only be 256 possible input values (and therefore output values) to the cryptographic primitive.

{% include _understanding-crypto/previous-and-next-exercise.html %}
