---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 1 Solutions - Ex1.5"
description: "A full solution-set for the problems at the end of Chapter 1 of Understanding Cryptography"
layout: post
headerImage: false
projects: false
date: 2017-12-16 22:44
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 1
exercise: 5
externalLink: false
---

{% include _understanding-crypto/chapter1-menu.html %}

## Exercise 1.5

As we learned in this chapter, modular arithmetic is the basis of many cryptosystems. As a consequence, we will address this topic with several problems in this and upcoming chapters.

Compute the result without a calculator:

1. 15 · 29 mod 13
2. 2 · 29 mod 13
3. 2 · 3 mod 13
4. −11 · 3 mod 13

The results should be given in the range from 0,1,..., modulus-1. Briefly describe the relation between the different parts of the problem.

### Solution

*This solution is verified as correct by the official [Solutions for Odd-Numbered Questions](http://wiki.crypto.rub.de/Buch/en/download/Understanding_Cryptography_Odd_Solutions.pdf) manual.*

We can compute these by reducing the individual terms (since all members of an equivalence class behave the same), performing the arithmetic and then reducing the result:

1\. $$ 15 \times 29\,\mathrm{mod}\,13 \equiv 2 \times 3\,\mathrm{mod}\,13 \equiv 6\,\mathrm{mod}\,13 $$

2\. $$ 2 \times 29\,\mathrm{mod}\,13 \equiv 2 \times 3\,\mathrm{mod}\,13 \equiv 6\,\mathrm{mod}\,13 $$

3\. $$ 2 \times 3\,\mathrm{mod}\,13 \equiv 6\,\mathrm{mod}\,13 $$

4\. $$ -11 \times 3\,\mathrm{mod}\,13 \equiv 2 \times 3\,\mathrm{mod}\,13 \equiv 6\,\mathrm{mod}\,13 $$
