---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 1 Solutions - Ex1.6"
description: "Division in Finite Sets"
layout: post
headerImage: false
projects: false
date: 2017-12-16 22:45
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 1
exercise: 6
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 1.6

Compute without a calculator:

1. 1/5 mod 13
2. 1/5 mod 7
3. 3 · 2/5 mod 7

### Solution

*I haven't yet verified these solutions independently. If you spot any mistakes, please leave a comment in the Disqus box at the bottom of the page.*

In order to perform a division by $$x$$, we must find the multiplicative inverse $$x^{-1}$$ and multiply by it.

1\.

$$
1 \div 5\,\mathrm{mod}\,13 \equiv 1 \times 5^{-1}\,\mathrm{mod}\,13 \\
\mathsf{where}\,5 \times 5^{-1} \,\mathrm{mod}\,13 \equiv 1\,\mathrm{mod}\,13
$$

$$ 5 \times 8\,\mathrm{mod}\,13 \equiv 1\,\mathrm{mod}\,13 \\ 5^{-1}\,\mathrm{mod}\,13 \equiv 8\,\mathrm{mod}\,13$$

$$ 1 \div 5\,\mathrm{mod}\,13 \equiv 1 \times 8\,\mathrm{mod}\,13 \equiv 8\,\mathrm{mod}\,13 $$

2\.

$$
1 \div 5\,\mathrm{mod}\,7 \equiv 1 \times 5^{-1}\,\mathrm{mod}\,7 \\
\mathsf{where}\,5 \times 5^{-1} \,\mathrm{mod}\,7 \equiv 1\,\mathrm{mod}\,7
$$

$$ 5 \times 3\,\mathrm{mod}\,7 \equiv 1\,\mathrm{mod}\,7 \\ 5^{-1}\,\mathrm{mod}\,7 \equiv 3\,\mathrm{mod}\,7$$

$$ 1 \div 5\,\mathrm{mod}\,7 \equiv 1 \times 3\,\mathrm{mod}\,7 \equiv 3\,\mathrm{mod}\,7 $$

3\.

$$
3 \times 2 \div 5\,\mathrm{mod}\,7 \equiv 3 \times 2 \times 5^{-1}\,\mathrm{mod}\,7 \\
\mathsf{where}\,2 \times 5^{-1} \,\mathrm{mod}\,7 \equiv 1\,\mathrm{mod}\,7
$$

$$ 5 \times 3\,\mathrm{mod}\,7 \equiv 1\,\mathrm{mod}\,7 \\ 5^{-1}\,\mathrm{mod}\,7 \equiv 3\,\mathrm{mod}\,7$$

$$
3 \times 2 \div 5\,\mathrm{mod}\,7 \equiv 3 \times 2 \times 3\,\mathrm{mod}\,7 \equiv 4\,\mathrm{mod}\,7 \\
\mathsf{because}\,3 \times 2 \times 3\,\mathrm{mod}\,7 \equiv 18\,\mathrm{mod}\,7 \equiv 4\,\mathrm{mod}\,7
$$

{% include _understanding-crypto/previous-and-next-exercise.html %}
