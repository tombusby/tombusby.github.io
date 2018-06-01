---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 3 Solutions - Ex3.7"
description: "Discussion of What Constitutes a \"Weak Key\" in DES"
layout: post
headerImage: false
projects: false
date: 2017-12-20 02:03
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 3
exercise: 7
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 3.7

A DES key $$K_w$$ is called a weak key if encryption and decryption are identical operations:

$$ \text{DES}_{K_w}(x) = \text{DES}_{K_w}^{-1}(x), \text{for all } x $$

1. Describe the relationship of the subkeys in the encryption and decryption algorithm that is required so that above-defined equality is fulfilled.
2. There are four weak DES keys. What are they?
3. What is the likelihood that a randomly selected key is weak?

### Solution

*This solution is verified as correct by the official [Solutions for Odd-Numbered Questions](http://wiki.crypto.rub.de/Buch/en/download/Understanding_Cryptography_Odd_Solutions.pdf) manual.*

1\. For $$ \text{DES}_{K_w}(x) = \text{DES}_{K_w}^{-1}(x), \text{for all } x $$ to hold true, the generated subkeys must have the following relation to one another:

$$ k_{i+1} = k_{16-i} \text{ for } i = 0, 1, ..., 7 $$

2\. Rotating all 0s or all 1s is the only way to satisfy this criteria, since they always produce the same subkey in each round.

As such, the four weak keys are when:

$$C_0 = FFFFFFF_{16} \text{ or } 0000000_{16}$$

$$\text{and}$$

$$D_0 = FFFFFFF_{16} \text{ or } 0000000_{16}$$

Ignoring the effect of $$PC-1$$ (since it's trivial to reverse), the four possible weak keys can be defined as:

$$ (C_0, D_0) = \mathtt{00000000000000}_{16} $$

$$ (C_0, D_0) = \mathtt{0000000FFFFFFF}_{16} $$

$$ (C_0, D_0) = \mathtt{FFFFFFF0000000}_{16} $$

$$ (C_0, D_0) = \mathtt{FFFFFFFFFFFFFF}_{16} $$

3\. The likelyhood of choosing one of these weak keys at random is as follows:

$$ \frac{4}{2^{56}} = \frac{2^2}{2^{56}} = \frac{1}{2^{54}} $$

{% include _understanding-crypto/previous-and-next-exercise.html %}
