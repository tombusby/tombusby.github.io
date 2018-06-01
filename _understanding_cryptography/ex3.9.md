---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 3 Solutions - Ex3.9"
description: "Discussion of Number of Key Checks Required to Brute-Force a DES Key from a Chosen Plaintext"
layout: post
headerImage: false
projects: false
date: 2017-12-21 21:48
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 3
exercise: 9
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 3.9

Assume we perform a known-plaintext attack against DES with one pair of plaintext and ciphertext. How many keys do we have to test in a worst-case scenario if we apply an exhaustive key search in a straightforward way? How many on average?

### Solution

*This solution is verified as correct by the official [Solutions for Odd-Numbered Questions](http://wiki.crypto.rub.de/Buch/en/download/Understanding_Cryptography_Odd_Solutions.pdf) manual.*

Assuming that no two keys can produce the same ciphertext for the same plaintext (possibly not a realistic assumption) then the worst case is the one where you have to check all $$2^{56}$$ possible keys. I.e. the correct key is the very last possible key that you check.

In the average case you will find the key exactly halfway through your search, meaning $$2^{56} \div 2 = 2^{55}$$ key checks.

{% include _understanding-crypto/previous-and-next-exercise.html %}
