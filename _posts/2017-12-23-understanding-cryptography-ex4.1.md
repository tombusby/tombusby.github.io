---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 4 Solutions - Ex4.1"
description: "The AES Development Process and How that Differed from DES"
layout: post
headerImage: false
projects: false
date: 2017-12-23 18:35
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 4
exercise: 1
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 4.1

Since May 26, 2002, the AES (Advanced Encryption Standard) describes the official standard of the US government.

1. The evolutionary history of AES differs from that of DES. Briefly describe the differences of the AES history in comparison to DES.
2. Outline the fundamental events of the developing process.
3. What is the name of the algorithm that is known as AES?
4. Who developed this algorithm?
5. Which block sizes and key lengths are supported by this algorithm?

### Solution

*This solution is verified as correct by the official [Solutions for Odd-Numbered Questions](http://wiki.crypto.rub.de/Buch/en/download/Understanding_Cryptography_Odd_Solutions.pdf) manual.*

1\. Unlike DES, AES was now developed in secret. It was developed via a public competition by NIST. Some of the design criteria of DES (the S-boxes in particlar) were kept secret from the public to defend against cryptographic attacks which were as yet unpublished and only known to the NSA. AES was developed by allowing a public acadamic evaluation of the algorithm candidates.

This appears to be the beginning of the modern consensus that an open design process produces the most secure algorithms.

2\. The important dates in the development of AES were:

* 2nd January 1997 - Public announcement of the need for a algorithm candidates by NIST for a new "Advanced Encryption Standard".
* 12th September 1997 - Formal request for candidate algorithms by NIST.
* 20th August 1998 - 15 candidate algorithms were selected by NIST.
* 9th August 1999 - 5 fianlist algorithms were announced.
* 2nd October 2000 - NIST announced that Rijndael had been selected as the AES algorithm.
* 26th November 2001 - AES was formally approved as a US federal standard.

3\. The Rijndael algorithm designed by a Belgian team was selected.

4\. The algorithm was designed by Dr. Vincent Rijmen and Dr. Joam Daemen, hence the name.

5\. Key sizes of 128, 192 and 256 are supported.

{% include _understanding-crypto/previous-and-next-exercise.html %}
