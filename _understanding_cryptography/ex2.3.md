---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 2 Solutions - Ex2.3"
description: "The Dangers of Key Reuse with One Time Pads (OTPs)"
layout: post
redirect_from: /understanding-cryptography-ex2.3/
headerImage: false
projects: false
date: 2017-12-18 18:15
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 2
exercise: 3
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 2.3

Assume an OTP-like encryption with a short key of 128 bit. This key is then being used periodically to encrypt large volumes of data. Describe how an attack works that breaks this scheme.

### Solution

*This solution is verified as correct by the official [Solutions for Odd-Numbered Questions](http://wiki.crypto.rub.de/Buch/en/download/Understanding_Cryptography_Odd_Solutions.pdf) manual.*

The main issue is that the key repeats every 128 bits. As such, the attacker may be able to exploit patterns in the underlying plaintext to recover the key. If a chosen plaintext attack is available then the attacker may be able to recover the entire key and then decrypt the rest of the message unhindered. In mathematical terms, the attack looks as follows (where $$s_i$$ is a keystream bit, $$x_i$$ is a chosen plaintext bit and $$y_i$$ is a ciphertext bit):

$$ s_i = x_i \oplus y_i; i = 1,2,... ,128 $$

{% include _understanding-crypto/previous-and-next-exercise.html %}
