---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 4 Solutions - Ex4.4"
description: "Performing Addition and Reduction in GF(2⁴)"
layout: post
redirect_from: /understanding-cryptography-ex4.4/
headerImage: false
projects: false
date: 2017-12-31 00:45
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 4
exercise: 4
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 4.4

Addition in $$GF(2^4)$$: Compute $$A(x) + B(x)\,\mathrm{mod}\,P(x)$$ in $$GF(2^4)$$ using the irreducible polynomial $$P(x) = x^4 +x +1$$. What is the influence of the choice of the reduction polynomial on the computation?

1. &nbsp;$$A(x) = x^2 +1, B(x) = x^3 +x^2 +1$$
2. &nbsp;$$A(x) = x^2 +1, B(x) = x+1$$

### Solution

*I haven't yet verified this solution independently. If you spot any mistakes, please leave a comment in the Disqus box at the bottom of the page.*

The choice of reduction polynomial has no impact on the computation, since it is not possible for the result of addition to be outside the Field and therefore require reduction.

1\.

$$ (x^2 + 1) \oplus (x^3 + x^2 + 1) = x^3 $$

2\.

$$ (x^2 + 1) \oplus (x + 1) = x^2 + x $$

{% include _understanding-crypto/previous-and-next-exercise.html %}
