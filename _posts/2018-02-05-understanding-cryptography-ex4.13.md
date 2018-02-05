---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 4 Solutions - Ex4.13"
description: "Checking Multiplicative Inverses in GF(2⁸)"
layout: post
headerImage: false
projects: false
date: 2018-02-05 19:09
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

We consider the first part of the ByteSub operation, i.e, the Galois field inversion.

1. Using Table 4.2, what is the inverse of the bytes 29, F3 and 01, where each byte is given in hexadecimal notation?
2. Verify your answer by performing a $$GF(2^8)$$ multiplication with your answer and the input byte. Note that you have to represent each byte first as polynomials in $$GF(2^8)$$. The most significant byte of each byte represents the $$x^7$$ coefficient

### Solution

*This solution is verified as correct by the official [Solutions for Odd-Numbered Questions](http://wiki.crypto.rub.de/Buch/en/download/Understanding_Cryptography_Odd_Solutions.pdf) manual.*

1\. As per Table 4.2:

$$ 26_{16}^{-1} = A8_{16} $$

$$ F3_{16}^{-1} = 34_{16} $$

$$ 01_{16}^{-1} = 01_{16} $$

2\. Instead of doing long-hand multiplications and reductions, I will be using the [Mod2Polynomial](https://github.com/tombusby/understanding-cryptography-exercises/blob/master/Chapter-04/gf.py) class I wrote for earlier exercises.

Just as a reminder, the reduction primitive polynomial is the AES polynomial: $$ P(x) = x^8 + x^4 + x^3 + x + 1 $$.

This is expressed as a Mod2Polynomial class as follows (it takes least significant bit first):

{% highlight python %}
mod = Mod2Polynomial([1, 1, 0, 1, 1, 0, 0, 0, 1])
{% endhighlight %}

The first pair, $$ 26_{16}^{-1} = A8_{16} $$, can be checked as follows:

$$ 26_{16} \equiv 00100110_2 \equiv x^5 + x^2 + x $$

$$ A8_{16} \equiv 10101000_2 \equiv x^7 + x^5 + x^3 $$

When multiplied together, these should produce an output of 1:

{% highlight python %}
mod = Mod2Polynomial([1, 1, 0, 1, 1, 0, 0, 0, 1])
a = Mod2Polynomial([0, 1, 1, 0, 0, 1, 0, 0])
b = Mod2Polynomial([0, 0, 0, 1, 0, 1, 0, 1])
print (a * b) % mod
{% endhighlight %}

This does produce an output of 1.

The second pair, $$ F3_{16}^{-1} = 34_{16} $$, can be checked as follows:

$$ F3_{16} \equiv 11110011_2 \equiv x^7 + x^6 + x^5 + x^4 + x + 1 $$

$$ 34_{16} \equiv 00110100_2 \equiv x^5 + x^4 + x^2 $$

When multiplied together, these should produce an output of 1:

{% highlight python %}
mod = Mod2Polynomial([1, 1, 0, 1, 1, 0, 0, 0, 1])
a = Mod2Polynomial([1, 1, 0, 0, 1, 1, 1, 1])
b = Mod2Polynomial([0, 0, 1, 0, 1, 1, 0, 0])
print (a * b) % mod
{% endhighlight %}

This does produce an output of 1.

The 01 case is pretty trivial and doesn't really need to be checked, but we will anyway:

$$ 01_{16} \equiv 00000001_2 \equiv 1 $$

When multiplied together, these should produce an output of 1:

{% highlight python %}
mod = Mod2Polynomial([1, 1, 0, 1, 1, 0, 0, 0, 1])
a = Mod2Polynomial([1, 0, 0, 0, 0, 0, 0, 0])
b = Mod2Polynomial([1, 0, 0, 0, 0, 0, 0, 0])
print (a * b) % mod
{% endhighlight %}

This does produce an output of 1.

{% include _understanding-crypto/previous-and-next-exercise.html %}
