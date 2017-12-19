---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 1 Solutions - Ex2.9"
description: "Discussion of Attacks Against LFSRs"
layout: post
headerImage: false
projects: false
date: 2017-12-19 00:08
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 2
exercise: 9
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 2.9

Given is a stream cipher which uses a single LFSR as key stream generator. The LFSR has a degree of 256.

1. How many plaintext/ciphertext bit pairs are needed to launch a successful attack?
2. Describe all steps of the attack in detail and develop the formulae that need to be solved.
3. What is the key in this system? Why doesn’t it make sense to use the initial contents of the LFSR as the key or as part of the key?

### Solution

*This solution is verified as correct by the official [Solutions for Odd-Numbered Questions](http://wiki.crypto.rub.de/Buch/en/download/Understanding_Cryptography_Odd_Solutions.pdf) manual.*

1\. The attacker needs 512 consecutive plaintext/ciphertext bit pairs $$(x_i, y_i)$$ for an attack.

2\. In order to calculate the (secret) feedback coefficients $$p_i$$, Oscar generates 256 linearly dependent equations using the relationship between the unknown key bits $$p_i$$ and the keystream output defined by the equation (where $$m = 256$$ in this instance):

$$ s_{i+m} \equiv \sum_{j=0}^{m-1} p_j ⋅ s_{i+j}\,\mathrm{mod}\,2; s_i, p_j \in \{0, 1\}; i \in \mathbb{Z}_{m} $$

After generating this linear equation system, it can be solved using Gauss-Jordan Elimination, revealing the 256 feedback coefficients

This mathematical definition is fairly dense and difficult to understand. The solution to the next exercise demonstrates how one goes about performing this attack in practise.

3\. The key of this system is represented by the 256 feedback coefficients. Since the initial contents of the LFSR are unalteredly shifted out of the LFSR and XORed with the first 256 plaintext bits, it would be easy to calculate them.

{% include _understanding-crypto/previous-and-next-exercise.html %}
