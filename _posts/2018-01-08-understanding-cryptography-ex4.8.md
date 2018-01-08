---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 4 Solutions - Ex4.8"
description: "Finding Irreducible Polynomials in GF(2)"
layout: post
headerImage: false
projects: false
date: 2018-01-08 23:08
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 4
exercise: 8
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 4.8

Find all irreducible polynomials

1. of degree 3 over $$GF(2)$$,
2. of degree 4 over $$GF(2)$$.

The best approach for doing this is to consider all polynomials of lower degree and check whether they are factors. Please note that we only consider monic irreducible polynomials, i.e., polynomials with the highest coefficient equal to one.

### Solution

*I haven't yet verified this solution independently. If you spot any mistakes, please leave a comment in the Disqus box at the bottom of the page.*

1. There are two irreducible polynomials in $$GF(2)$$ of degree 3:

    + &nbsp; $$ 1+x+x^3 $$

    + &nbsp; $$ 1+x^2+x^3 $$

2. There are three irreducible polynomials in $$GF(2)$$ of degree 4:

    + &nbsp; $$ 1+x+x^4 $$

    + &nbsp; $$ 1+x+x^2+x^3+x^4 $$

    + &nbsp; $$ 1+x^3+x^4 $$

{% include _understanding-crypto/previous-and-next-exercise.html %}
