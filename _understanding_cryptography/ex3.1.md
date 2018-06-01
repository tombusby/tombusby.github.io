---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 3 Solutions - Ex3.1"
description: "Verifying the Non-Linearity of the DES S-boxes"
layout: post
redirect_from: /understanding-cryptography-ex3.1/
headerImage: false
projects: false
date: 2017-12-19 20:47
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 3
exercise: 1
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 3.1

As stated in Sect. 3.5.2, one important property which makes DES secure is that the S-boxes are nonlinear. In this problem we verify this property by computing the output of S1 for several pairs of inputs.

Show that $$ S_1(x_1) \oplus S_1(x_2) \neq S_1(x_1 \oplus x_2) $$, where “$$\oplus$$” denotes bitwise XOR, for:

1. &nbsp; $$ x_1 = 000000_2, x_2 = 000001_2 $$
2. &nbsp; $$ x_1 = 111111_2, x_2 = 100000_2 $$
3. &nbsp; $$ x_1 = 101010_2, x_2 = 010101_2 $$

### Solution

*This solution is verified as correct by the official [Solutions for Odd-Numbered Questions](http://wiki.crypto.rub.de/Buch/en/download/Understanding_Cryptography_Odd_Solutions.pdf) manual.*

1\.

$$ S_1(000000_2) \oplus S_1(000001_2) \neq S_1(000000_2 \oplus 000001_2) $$

$$ 1110_2 \oplus 0000_2 \neq S_1(000001_2) $$

$$ 1110_2 \neq 0000_2 $$

2\.

$$ S_1(111111_2) \oplus S_1(100000_2) \neq S_1(111111_2 \oplus 100000_2) $$

$$ 1101_2 \oplus 0100_2 \neq S_1(011111_2) $$

$$ 1001_2 \neq 1000_2 $$

3\.

$$ S_1(101010_2) \oplus S_1(010101_2) \neq S_1(101010_2 \oplus 010101_2) $$

$$ 0110_2 \oplus 1100_2 \neq S_1(111111_2) $$

$$ 1010_2 \neq 1101_2 $$

{% include _understanding-crypto/previous-and-next-exercise.html %}
