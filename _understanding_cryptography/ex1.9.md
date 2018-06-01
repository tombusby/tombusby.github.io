---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 1 Solutions - Ex1.9"
description: "Computing Large Exponents in Finite Sets"
layout: post
redirect_from: /understanding-cryptography-ex1.9/
headerImage: false
projects: false
date: 2017-12-16 22:48
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 1
exercise: 9
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 1.9

Compute $$x$$ as far as possible without a calculator. Where appropriate, make use of a smart decomposition of the exponent as shown in the example in Sect. 1.4.1:

1. x = 3<sup>2</sup> mod 13
2. x = 7<sup>2</sup> mod 13
3. x = 3<sup>10</sup> mod 13
4. x = 7<sup>100</sup> mod 13
5. 7<sup>x</sup> = 11, mod 13

The last problem is called a discrete logarithm and points to a hard problem which we discuss in Chap. 8. The security of many public-key schemes is based on the hardness of solving the discrete logarithm for large numbers, e.g., with more than 1000 bits.

### Solution

*This solution is verified as correct by the official [Solutions for Odd-Numbered Questions](http://wiki.crypto.rub.de/Buch/en/download/Understanding_Cryptography_Odd_Solutions.pdf) manual.*

These can be performed by performing a smaller exponentiation and reducing:

1\.

$$ 3^2\,\mathrm{mod}\,13 \equiv 9\,\mathrm{mod}\,13$$

2\.

$$ 7^2\,\mathrm{mod}\,13 \equiv 49\,\mathrm{mod}\,13 \equiv 10\,\mathrm{mod}\,13 $$

3\.

$$
3^{10}\,\mathrm{mod}\,13 \equiv 9^5\,\mathrm{mod}\,13 \equiv 81^2 \times 9\,\mathrm{mod}\,13 \\
\equiv\,3^2 \times 9\,\mathrm{mod}\,13\equiv81\,\mathrm{mod}\,13\equiv3\,\mathrm{mod}\,13
$$

4\.

$$ 7^{100}\,\mathrm{mod}\,13 $$

$$ \equiv {7^2}^{50}\,\mathrm{mod}\,13 $$

$$ \equiv 49^{50}\,\mathrm{mod}\,13 $$

$$ \equiv 10^{50}\,\mathrm{mod}\,13 $$

$$ \equiv {10^2}^{25}\,\mathrm{mod}\,13 $$

$$ \equiv 100^{25}\,\mathrm{mod}\,13 $$

$$ \equiv 9^{25}\,\mathrm{mod}\,13 $$

$$ \equiv {9^2}^{12} \times 9\,\mathrm{mod}\,13 $$

$$ \equiv 81^{12} \times 9\,\mathrm{mod}\,13 $$

$$ \equiv 3^{12} \times 9\,\mathrm{mod}\,13 $$

$$ \equiv {3^3}^{4} \times 9\,\mathrm{mod}\,13 $$

$$ \equiv 27^{4} \times 9\,\mathrm{mod}\,13 $$

$$ \equiv 1^{4} \times 9\,\mathrm{mod}\,13 $$

$$ \equiv 9\,\mathrm{mod}\,13 $$

5\. Through trial and error, we can discover the value of $$x$$:

$$ 7^5\,\mathrm{mod}\,13 \equiv 11\,\mathrm{mod}\,13 $$

{% include _understanding-crypto/previous-and-next-exercise.html %}
