---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 5 Solutions - Ex5.3"
description: "Discussion of Deriving the IV of a Cipher in CBC Mode where Key is Known"
layout: post
headerImage: false
projects: false
date: 2018-05-11 15:48
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 5
exercise: 3
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 5.3

In a company, all files which are sent on the network are automatically encrypted by using AES-128 in CBC mode. A fixed key is used, and the IV is changed once per day. The network encryption is file-based, so that the IV is used at the beginning of every file.

You managed to spy out the fixed AES-128 key, but do not know the recent IV. Today, you were able to eavesdrop two different files, one with unidentified content and one which is known to be an automatically generated temporary file and only contains the value 0xFF. Briefly describe how it is possible to obtain the unknown initialization vector and how you are able to determine the content of the unknown file.

### Solution

*This solution is verified as correct by the official [Solutions for Odd-Numbered Questions](http://wiki.crypto.rub.de/Buch/en/download/Understanding_Cryptography_Odd_Solutions.pdf) manual.*

To derive the IV, all that needs to be done is to decrypt the first block of the temporary file using your pilfered key. Given that you know the true plaintext of the file only contains 0xFF values, you can XOR the known plaintext (a string of 0xFF values) with the value produced by this decryption. In practise, XOR with all 1s produces a bitwise flip of every bit. This will yield the IV currently in use.

Expressed in mathematical notation, this is as follows:

$$ IV = d_k(y_0) \oplus x_0 $$

Now that you have the IV and the key, you're free to decrypt the unknown file.

{% include _understanding-crypto/previous-and-next-exercise.html %}
