---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 1 Solutions - Ex1.10"
description: "Computing Euler's Phi Function"
layout: post
headerImage: false
projects: false
date: 2017-12-16 22:49
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 1
exercise: 10
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 1.10

Find all integers n between $$0 ≤ n < m$$ that are relatively prime to $$m$$ for $$m = 4,5,9,26$$. We denote the number of integers $$n$$ which fulfill the condition by $$\phi(m)$$, e.g. $$\phi(3) = 2$$. This function is called “Euler’s phi function”. What is $$\phi(m)$$ for $$m = 4,5,9,26$$?

### Solution

*I haven't yet verified these solutions independently. If you spot any mistakes, please leave a comment in the Disqus box at the bottom of the page.*

$$m = 4$$:

$$ \mathrm{gcd}(0, 4) = 4 $$

$$ \mathrm{gcd}(1, 4) = 1 $$

$$ \mathrm{gcd}(2, 4) = 2 $$

$$ \mathrm{gcd}(3, 4) = 1 $$

$$\phi(4) = 2$$

$$m = 5$$:

$$ \mathrm{gcd}(0, 5) = 5 $$

$$ \mathrm{gcd}(1, 5) = 1 $$

$$ \mathrm{gcd}(2, 5) = 1 $$

$$ \mathrm{gcd}(3, 5) = 1 $$

$$ \mathrm{gcd}(4, 5) = 1 $$

$$\phi(5) = 4$$

$$m = 9$$:

$$ \mathrm{gcd}(0, 9) = 9 $$

$$ \mathrm{gcd}(1, 9) = 1 $$

$$ \mathrm{gcd}(2, 9) = 1 $$

$$ \mathrm{gcd}(3, 9) = 3 $$

$$ \mathrm{gcd}(4, 9) = 1 $$

$$ \mathrm{gcd}(5, 9) = 1 $$

$$ \mathrm{gcd}(6, 9) = 3 $$

$$ \mathrm{gcd}(7, 9) = 1 $$

$$ \mathrm{gcd}(8, 9) = 1 $$

$$\phi(9) = 6$$

$$m = 26$$:

$$ \mathrm{gcd}(0, 26) = 26 $$

$$ \mathrm{gcd}(1, 26) = 1 $$

$$ \mathrm{gcd}(2, 26) = 2 $$

$$ \mathrm{gcd}(3, 26) = 3 $$

$$ \mathrm{gcd}(4, 26) = 2 $$

$$ \mathrm{gcd}(5, 26) = 1 $$

$$ \mathrm{gcd}(6, 26) = 2 $$

$$ \mathrm{gcd}(7, 26) = 1 $$

$$ \mathrm{gcd}(8, 26) = 2 $$

$$ \mathrm{gcd}(9, 26) = 1 $$

$$ \mathrm{gcd}(10, 26) = 2 $$

$$ \mathrm{gcd}(11, 26) = 1 $$

$$ \mathrm{gcd}(12, 26) = 2 $$

$$ \mathrm{gcd}(13, 26) = 13 $$

$$ \mathrm{gcd}(14, 26) = 2 $$

$$ \mathrm{gcd}(15, 26) = 1 $$

$$ \mathrm{gcd}(16, 26) = 2 $$

$$ \mathrm{gcd}(17, 26) = 1 $$

$$ \mathrm{gcd}(18, 26) = 2 $$

$$ \mathrm{gcd}(19, 26) = 1 $$

$$ \mathrm{gcd}(20, 26) = 2 $$

$$ \mathrm{gcd}(21, 26) = 1 $$

$$ \mathrm{gcd}(22, 26) = 2 $$

$$ \mathrm{gcd}(23, 26) = 1 $$

$$ \mathrm{gcd}(24, 26) = 2 $$

$$ \mathrm{gcd}(25, 26) = 1 $$

$$\phi(26) = 12$$

{% include _understanding-crypto/previous-and-next-exercise.html %}
