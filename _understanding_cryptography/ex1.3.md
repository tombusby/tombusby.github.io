---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 1 Solutions - Ex1.3"
description: "Calculating the Length of Brute-Force Attacks against AES"
layout: post
redirect_from: /understanding-cryptography-ex1.3/
headerImage: false
projects: false
date: 2017-12-16 22:41
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 1
exercise: 3
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 1.3

We consider the long-term security of the Advanced Encryption Standard
(AES) with a key length of 128-bit with respect to exhaustive key-search attacks.

AES is perhaps the most widely used symmetric cipher at this time.

1. Assume that an attacker has a special purpose application specific integrated circuit (ASIC) which checks $$5 \times 10^8$$ keys per second, and she has a budget of $1 million. One ASIC costs $50, and we assume 100% overhead for integrating the ASIC (manufacturing the printed circuit boards, power supply, cooling, etc.). How many ASICs can we run in parallel with the given budget? How long does an average key search take? Relate this time to the age of the Universe, which is about $$10^{10}$$ years.
2. We try now to take advances in computer technology into account. Predicting the future tends to be tricky but the estimate usually applied is Moore’s Law, which states that the computer power doubles every 18 months while the costs of integrated circuits stay constant. How many years do we have to wait until a key-search machine can be built for breaking AES with 128 bit with an average search time of 24 hours? Again, assume a budget of $1 million (do not take inflation into account).

### Solution

*This solution is verified as correct by the official [Solutions for Odd-Numbered Questions](http://wiki.crypto.rub.de/Buch/en/download/Understanding_Cryptography_Odd_Solutions.pdf) manual.*

1\. By the numbers specified, each machine costs \$100.

<div style="text-align: center;">
$$ $1,000,000 \div $100 = 10,000\,\mathsf{machines} $$.
</div>

Each of our 10,000 machines checks $5 \times 10^8$ keys per second, so we can calculate the speed of our parallelised machines as:

<div style="text-align: center;">
$$ 5 \times 10^8 \times 10^4 = 5 \times 10^{12}\,\mathsf{keys/second} $$
</div>

On average, we'll find the correct key halfway through our search of the keyspace ($$2^{128}$$), so the average case will be $$2^{127}$$ checks.

To calculate the length of time to find a key in the average case:

<div style="text-align: center;">
$$ \frac{2^{127}\,\mathsf{keys}}{5 \times 10^{12}\,\mathsf{keys/second}} \approx 3.4 \times 10^{25}\,\mathsf{seconds}$$
</div>

This comes out to:

<div style="text-align: center;">
$$ 1.08 \times 10^{18}\,\mathsf{years} $$
</div>

This is approximately $$10^8$$ times longer than the elapsed age of the universe.

2\. If we represent the number of Moore's Law iterations to bring the using $$i$$ then we can express the equation as:

<div style="text-align: center;">
$$ 1.08 \times 10^{18}\,\mathsf{years} \times \frac{365}{2^i} = 1\,\mathsf{day} $$
</div>

We can rearrange this equation to make $$2^i$$ the only item on the left:

<div style="text-align: center;">
$$ 2^i = 1.08 \times 10^{18}\,\mathsf{years} \times 365\,\mathsf{days} $$
</div>

The reveals a value of $$i$$ which is roughly $$68.42$$.

Rounding the number of iterations to 69 allows us to calculate the number of years:

<div style="text-align: center;">
$$ 1.5\,\mathsf{years} \times 69\,\mathsf{iterations} = 103.5\,\mathsf{years} $$
</div>

{% include _understanding-crypto/previous-and-next-exercise.html %}
