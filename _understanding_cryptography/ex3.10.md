---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 3 Solutions - Ex3.10"
description: "Discussion of Clock Frequency Requirements for DES Hardware Implementations"
layout: post
headerImage: false
projects: false
date: 2017-12-21 22:33
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 3
exercise: 10
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 3.10

In this problem we want to study the clock frequency requirements for a hardware implementation of DES in real-world applications. The speed of a DES implementation is mainly determined by the time required to do one core iteration. This hardware kernel is then used 16 consecutive times in order to generate the encrypted output. (An alternative approach would be to build a hardware pipeline with 16 stages, resulting in 16-fold increased hardware costs.)

1. Let’s assume that one core iteration can be performed in one clock cycle. Develop an expression for the required clock frequency for encrypting a stream of data with a data rate r [bit/sec]. Ignore the time needed for the initial and final permutation.
2. What clock frequency is required for encrypting a fast network link running at a speed of 1 Gb/sec? What is the clock frequency if we want to support a speed of 8 Gb/sec?

### Solution

*I haven't yet verified this solution independently. If you spot any mistakes, please leave a comment in the Disqus box at the bottom of the page.*

1\. If one round of encryption is performed each clock cycle, then we can say that 64 bits of data are encrypted in 16 clock cycles. As such, we can calculate the number of bits encrypted per clock cycle:

$$ \frac{64\text{ bits}}{16\text{ cycles}} = 4 \text{ bits/cycle} $$

As such, the throughput in bits for a given clock speed can be calculated as follows:

$$ c\text{ cycles/second} \times 4 \text{ bits/cycle} = b \text{ bits/second} $$

2\. In order to calculate the clock speed required for a given bitrate, we need to rearrange the above equation:

$$ c\text{ cycles/second} = \frac{b \text{ bits/second}}{4 \text{ bits/cycle}} $$

For 1 Gb of throughput per second, we need a clock speed of:

$$ 2.5 \times 10^8 \text{ cycles/second} = \frac{1 \times 10^9 \text{ bits/second}}{4 \text{ bits/cycle}} $$

For 8 Gb of throughput per second, we need a clock speed of:

$$ 2 \times 10^9 \text{ cycles/second} = \frac{8 \times 10^9 \text{ bits/second}}{4 \text{ bits/cycle}} $$

{% include _understanding-crypto/previous-and-next-exercise.html %}
