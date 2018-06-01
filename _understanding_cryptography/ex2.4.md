---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 2 Solutions - Ex2.4"
description: "The Impossibility of Brute Force Attacks on One Time Pads (OTPs)"
layout: post
redirect_from: /understanding-cryptography-ex2.4/
headerImage: false
projects: false
date: 2017-12-18 18:32
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 2
exercise: 4
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 2.4

At first glance it seems as though an exhaustive key search is possible against an OTP system. Given is a short message, let’s say 5 ASCII characters represented by 40 bit, which was encrypted using a 40-bit OTP. Explain exactly why an exhaustive key search will not succeed even though sufficient computational resources are available. This is a paradox since we know that the OTP is unconditionally secure. That is, explain why a brute-force attack does not work.

*Note*: You have to resolve the paradox! That means answers such as “The OTP is unconditionally secure and therefore a brute-force attack does not work” are not valid

### Solution

*I haven't yet verified this solution independently. If you spot any mistakes, please leave a comment in the Disqus box at the bottom of the page.*

Even with a relatively small search space such as 40-bits, the plaintext could be any 40-bit number and the key could be any 40-bit number. As such any plaintext can produce any ciphertext, given the right key and any key can produce any ciphertext, given the right plaintext.

The distribution of these inputs and outputs is also completely even (i.e. there are no statistical relationships of any kind). Another way of wording this is that each bit is encrypted with its own 1-bit key. As such all bits in the plaintext (or ciphertext) are completely unrelated to each other in terms of encryption and decryption.

There is simply no way to confirm that the checked key is correct. With a brute-force attack, you need some means of knowing when you've found the correct answer. With a One Time Pad, no such method exists. With OTPs, there are no mathematical "threads" to pull on which unravel the system when found. This is in contrast to other modern crypto-systems in that those "threads" normally do exist, but are meant to be computationally infeasible to find.

With a conventional block cipher, if you can find a key that decrypts all blocks into plaintext that is intelligible (i.e. something which is recognisable as sensible input, and not random bits), you can be pretty sure it's the correct one. The chances of there being a pair of keys that map two distinct, intelligible plaintexts to the same ciphertext blocks is atronomically unlikely.

When trying to brute force a OTP, you are in fact trying to solve a series of totally unrelated 1-bit cryptograms:

* Each 0 could be:
  * a plaintext 1 encrypted with a keybit 1 (50% chance)
  * a plaintext 0 encrypted with a keybit 0 (50% chance)
* Each 1 could be:
  * a plaintext 1 encrypted with a keybit 0 (50% chance)
  * a plaintext 0 encrypted with a keybit 1 (50% chance)

{% include _understanding-crypto/previous-and-next-exercise.html %}
