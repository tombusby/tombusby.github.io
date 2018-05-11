---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 5 Solutions - Ex5.5"
description: "The Problems of IV Reuse with Ciphers in OFB Mode"
layout: post
headerImage: false
projects: false
date: 2018-05-11 16:37
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 5
exercise: 5
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 5.5

Describe how the OFB mode can be attacked if the IV is not different for each execution of the encryption operation.

### Solution

*This solution is verified as correct by the official [Solutions for Odd-Numbered Questions](http://wiki.crypto.rub.de/Buch/en/download/Understanding_Cryptography_Odd_Solutions.pdf) manual.*

Assuming that the key remains the same, encrypting with the same IV will produce the exact same keystream as previous encryptions.

If no plaintext/ciphertext pairs are known, then there's no way to use this fact to attack the cipher. However, if you have a chosen plaintext for a given block $$b_i$$ in message $$m_1$$, this can be XORed with the known ciphertext to derive the keystream for that block. The keystream can then be used to decrypt block $$b_i^\prime$$ in message $$m_2$$ (which was encrypted using the same IV and so produced the same keystream).

{% include _understanding-crypto/previous-and-next-exercise.html %}
