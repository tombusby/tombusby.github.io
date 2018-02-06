---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 4 Solutions - Ex4.16"
description: "Calculating the Length of Brute-Force Attacks against AES"
layout: post
headerImage: false
projects: false
date: 2018-02-06 16:41
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 4
exercise: 16
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 4.16

For the following, we assume AES with 192-bit key length. Furthermore, let us assume an ASIC which can check $$3 \times 10^7$$ keys per second.

1. If we use 100,000 such ICs in parallel, how long does an average key search take? Compare this period of time with the age of the universe (approx. $$10^{10}$$ years).
2. Assume Moore’s Law will still be valid for the next few years, how many years do we have to wait until we can build a key search machine to perform an average key search of AES-192 in 24 hours? Again, assume that we use 100,000 ICs in parallel.

### Solution

*I haven't yet verified this solution independently. If you spot any mistakes, please leave a comment in the Disqus box at the bottom of the page.*

1\. With 100,000 ICs in parallel, the amount of keys we can check per second is as follows:

$$ 10^5 \,\mathsf{ICs} \times 3 \times 10^7\,\mathsf{keys/second} = 3 \times 10^{12}\,\mathsf{keys/second} $$

The size of the keyspace in this instance is $$ 2^{196} $$, meaning the average key search will need to do $$ 2^{196} \div 2 = 2^{195} $$ checks. As such, the time required for an average key search is as follows:

$$ 2^{195} \,\mathsf{keys} \div 3 \times 10^{12}\,\mathsf{keys/second} \approx 1.674 \times 10^{46} \,\mathsf{seconds} $$

We can calculate what this value is in years:

$$ \frac{1.674 \times 10^{46} \,\mathsf{seconds}}{60 \times 60 \times 24 \times 365.25} \approx 5.304 \times 10^{38} \,\mathsf{years} $$

This is approximately $$10^{28}$$ times the current elapsed age of the universe.

2\. If we represent the number of Moore's Law iterations to bring the using $$i$$ then we can express the equation as:

$$ 5.304 \times 10^{38}\,\mathsf{years} \times \frac{365.25}{2^i} = 1\,\mathsf{day} $$

We can rearrange this equation to make $$2^i$$ the only item on the left:

$$ 2^i = 5.304 \times 10^{38}\,\mathsf{years} \times 365.25\,\mathsf{days} $$

The reveals a value of $$i$$ which is roughly $$137.153$$.

Rounding the number of iterations to 137 allows us to calculate the number of years:

$$ 1.5\,\mathsf{years} \times 137\,\mathsf{iterations} = 205.5\,\mathsf{years} $$

{% include _understanding-crypto/previous-and-next-exercise.html %}
