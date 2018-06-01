---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 4 Solutions - Ex4.7"
description: "Finding Multiplicative Inverses in GF(2⁴)"
layout: post
redirect_from: /understanding-cryptography-ex4.7/
headerImage: false
projects: false
date: 2018-01-07 23:34
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 4
exercise: 7
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 4.7

We consider the field $$GF(2^4)$$, with $$P(x) = x^4 + x + 1$$ being the irreducible polynomial. Find the inverses of $$A(x) = x$$ and $$B(x) = x^2 + x$$. You can find the inverses either by trial and error, i.e., brute-force search, or by applying the Euclidean algorithm for polynomials. (However, the Euclidean algorithm is only sketched in this chapter.) Verify your answer by multiplying the inverses you determined by A and B, respectively.

### Solution

*This solution is verified as correct by the official [Solutions for Odd-Numbered Questions](http://wiki.crypto.rub.de/Buch/en/download/Understanding_Cryptography_Odd_Solutions.pdf) manual.*

Rather than implementing the Extended Euclidian Algorithm, I will instead just be finding them via trial and error using the code I wrote for Ex4.3. The number of possiblities to check is only 16 ($$2^4$$) for each of the two subquestions, so it's more time-efficient to check manually:

$$ (x)(x^3 + 1) \,\mathrm{mod}\, x^4 + x + 1 \equiv 1 \,\mathrm{mod}\, x^4 + x + 1 $$

The multiplicative inverse is therefore $$x^3 + 1$$. This answer can be verified as correct (assuming my code is correct) by placing the following python code in the \__main__ section of the script written for Ex4.3:

{% highlight python %}

    mod = Mod2Polynomial([1, 1, 0, 0, 1])
    a = Mod2Polynomial([0, 1])
    b = Mod2Polynomial([1, 0, 0, 1])

    print a, "\t", b
    print a * b % mod

{% endhighlight %}

$$ (x^2 + x)(x^2 + x + 1) \,\mathrm{mod}\, x^4 + x + 1 \equiv 1 \,\mathrm{mod}\, x^4 + x + 1 $$

The multiplicative inverse is therefore $$x^2 + x + 1$$. This answer can be verified as correct (assuming my code is correct) by placing the following python code in the \__main__ section of the script written for Ex4.3:

{% highlight python %}

    mod = Mod2Polynomial([1, 1, 0, 0, 1])
    a = Mod2Polynomial([0, 1, 1])
    b = Mod2Polynomial([1, 1, 1, 0])

    print a, "\t", b
    print a * b % mod

{% endhighlight %}

{% include _understanding-crypto/previous-and-next-exercise.html %}
