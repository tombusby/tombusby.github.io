---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 1 Solutions - Ex1.8"
description: "A full solution-set for the problems at the end of Chapter 1 of Understanding Cryptography"
layout: post
headerImage: false
projects: false
date: 2017-12-16 22:47
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 1
exercise: 8
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 1.8

What is the multiplicative inverse of 5 in $$\mathbb{Z}_{11}$$, $$\mathbb{Z}_{12}$$, and $$\mathbb{Z}_{13}$$? You can do a
trial-and-error search using a calculator or a PC.

With this simple problem we want now to stress the fact that the inverse of an integer in a given ring depends completely on the ring considered. That is, if the modulus changes, the inverse changes. Hence, it doesn’t make sense to talk about an inverse of an element unless it is clear what the modulus is. This fact is crucial for the RSA cryptosystem, which is introduced in Chap. 7. The extended Euclidean algorithm, which can be used for computing inverses efficiently, is introduced in Sect. 6.3.

### Solution

*I haven't yet verified these solutions independently. If you spot any mistakes, please leave a comment in the Disqus box at the bottom of the page.*

These are small enough sets that you can find their inverses ($$5 \times 5^{-1} \equiv 1\,\mathrm{mod}\,n$$) by trial and error:

$$
5^{-1}\,\mathrm{mod}\,11 \equiv 9\,\mathrm{mod}\,11 \\
5^{-1}\,\mathrm{mod}\,12 \equiv 5\,\mathrm{mod}\,12 \\
5^{-1}\,\mathrm{mod}\,13 \equiv 8\,\mathrm{mod}\,13
$$

{% include _understanding-crypto/previous-and-next-exercise.html %}
