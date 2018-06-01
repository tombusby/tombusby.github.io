---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 2 Solutions - Ex2.2"
description: "Discussion of Key Management Issues with One Time Pads (OTPs)"
layout: post
redirect_from: /understanding-cryptography-ex2.2/
headerImage: false
projects: false
date: 2017-12-18 18:00
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 2
exercise: 2
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 2.2

Assume we store a one-time key on a CD-ROM with a capacity of 1 Gbyte. Discuss the real-life implications of a One-Time-Pad (OTP) system. Address issues such as life cycle of the key, storage of the key during the life cycle/after the life cycle, key distribution, generation of the key, etc.

### Solution

*I haven't yet verified this solution independently. If you spot any mistakes, please leave a comment in the Disqus box at the bottom of the page.*

Some of the main issues are as follows:

* The key must be stored at both the sender and receiver's end. So all of these issues must be dealt with at each end. One can never be sure whether the person/organisation at the other end of the channel followed procedures correctly to secure the keybits.
* The key must be kept secure (both before, during, and long after encryption).
* The key must not ever be reused. Once bit $$n$$ has been read from the CD, it should never be used to encrypt any other data.
* Since the key must be stored at both the sending and receiving end, it must be securely destroyed at both ends after use. If the keybits are leaked at any point in the future, the attacker can decrypt all historical messages.
* Generation of the key must be done via true randomness. Pseudorandomness just means you are basically encrypting with a pseudorandom stream cipher, but caching a lot of keybits before. If pseudorandomness is used to generate the key, then this is no more secure than simply sharing the seed and generating the bits independently at each end of the channel like a normal stream cipher.
* Distribution of the key must handled very securely since, if it's intercepted and copied in transit, then it is worthless. Also, if you send the keybits over a channel via a conventional cipher, then you have just reduced the security of the system to the security of that cipher, since breaking that cipher reveals all the OTP subkeys.

{% include _understanding-crypto/previous-and-next-exercise.html %}
