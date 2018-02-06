---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 4 Solutions - Ex4.13"
description: "Derive RC Constants in AES Key Schedule"
layout: post
headerImage: false
projects: false
date: 2018-02-06 15:18
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 4
exercise: 13
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 4.13

*Derive* the bit representation for the following round constants within the key schedule:

* RC[8]
* RC[9]
* RC[10]

### Solution

*This solution is verified as correct by the official [Solutions for Odd-Numbered Questions](http://wiki.crypto.rub.de/Buch/en/download/Understanding_Cryptography_Odd_Solutions.pdf) manual.*

Starting from $$RC[1] = 01$$, $$RC[i] = 02 \times RC[i - 1]\,\mathrm{mod}\,P(x)$$ where $$P(x)$$ is the AES polynomial.

As such, the first 10 RC values are as follows:

$$R[1] = 1 = 00000001_2 = 01_{16} $$

$$R[2] = x = 00000010_2 = 02_{16} $$

$$R[3] = x^2 = 00000100_2 = 04_{16} $$

$$R[4] = x^3 = 00001000_2 = 08_{16} $$

$$R[5] = x^4 = 00010000_2 = 10_{16} $$

$$R[6] = x^5 = 00100000_2 = 20_{16} $$

$$R[7] = x^6 = 01000000_2 = 40_{16} $$

$$R[8] = x^7 = 10000000_2 = 80_{16} $$

$$R[9] = x^4 + x^3 + x + 1 = 00011011_2 = 1B_{16} $$

$$R[10] = x^5 + x^4 + x^2 + x = 00110110_2 = 36_{16} $$

After 8 is where the reduction polynomial comes into play to bring the result back into the field.

I wrote a [python script](https://github.com/tombusby/understanding-cryptography-exercises/blob/master/Chapter-04/ex4.15.py) which can calculate any number of RC constants:

{% highlight python %}
# coding: utf-8
from gf import Mod2Polynomial

if __name__ == "__main__":
    template = u"R[{}] = {} = {}₂ = {}₁₆"
    reduction_polynomial = Mod2Polynomial([1, 1, 0, 1, 1, 0, 0, 0, 1])
    poly = Mod2Polynomial([1])
    p02 = Mod2Polynomial([0, 1])
    print template.format(1, poly, poly.get_binary_representation(), "01")
    for i in range(2, 11):
        previous = poly
        poly = (previous * p02) % reduction_polynomial
        bin_rep = poly.get_binary_representation()
        print template.format(i, poly, bin_rep, hex(int(bin_rep, 2)).lstrip("-0x").zfill(2).upper())
{% endhighlight %}

{% include _understanding-crypto/previous-and-next-exercise.html %}
