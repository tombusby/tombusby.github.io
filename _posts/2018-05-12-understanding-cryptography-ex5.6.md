---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 5 Solutions - Ex5.6"
description: "Designing an OFB Mode Scheme for Byte Encryption"
layout: post
headerImage: false
projects: false
date: 2018-05-12 20:00
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 5
exercise: 6
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 5.6

Propose an OFB mode scheme which encrypts one byte of plaintext at a time, e.g., for encrypting key strokes from a remote keyboard. The block cipher used is AES. Perform one block cipher operation for every new plaintext byte. Draw a block diagram of your scheme and pay particular attention to the bit lengths used in your diagram (cf. the descripton of a byte mode at the end of Sect. 5.1.4).

### Solution

*I haven't yet verified this solution independently. If you spot any mistakes, please leave a comment in the Disqus box at the bottom of the page.*

Below is an OFB scheme which matches the requirements:

![OFB Scheme]({{ site.url }}/assets/images/understanding-crypto/ex5-6-OFB-mode-scheme.png){: .image-center}

*Note*: In places where 128 bits are truncated to 8 bits, this is done by taking the first 8 bits.

{% include _understanding-crypto/previous-and-next-exercise.html %}
