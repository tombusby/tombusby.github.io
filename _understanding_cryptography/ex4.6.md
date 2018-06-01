---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 4 Solutions - Ex4.6"
description: "Calculating a Division in GF(2⁸) and Reducing via the AES Irreducible Polynomial"
layout: post
redirect_from: /understanding-cryptography-ex4.6/
headerImage: false
projects: false
date: 2017-12-31 14:19
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 4
exercise: 6
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 4.6

Compute in $$GF(2^8)$$:

$$ (x^4 + x + 1) \div (x^7 + x^6 + x^3 + x^2) $$

where the irreducible polynomial is the one used by AES, $$ P(x) = x^8 + x^4 + x^3 + x + 1$$.

Note that Table 4.2 contains a list of all multiplicative inverses for this field.

### Solution

*I haven't yet verified this solution independently. If you spot any mistakes, please leave a comment in the Disqus box at the bottom of the page.*

The multiplicative inverse could be found via the Euclidian Algorithm, though in this instance I have simply looked it up in the table mentioned above:

$$ x^7 + x^6 + x^3 + x^2 = 11001100_2 = CC_{16} $$

$$ CC^{-1}_{16} = 1B_{16} = 00011011_2 = x^4 + x^3 + x + 1 $$

Next we perform a naive multiplication of $$ (x^4 + x + 1) $$ with the inverse we just looked up:

$$ (x^4 + x + 1)(x^4 + x^3 + x + 1) \\ = x^8 + x^7 + x^4 + x^3 + x^2 + 1 $$

All that's left to do now is to reduce it via the reduction polynomial

$$
\require{enclose}
\begin{array}{r}
                1  \\[-3pt]
x^8 + x^4 + x^3 + x + 1 \enclose{longdiv}{x^8 + x^7 + 0 + 0 + x^4 + x^3 + x^2 + 0 + 1} \\
\oplus\,\,\underline{x^8 + \,0\, + 0\, + 0 + x^4 + x^3 + \,0\, +\, x + 1} \\
x^7\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\, + x^2 + \,x\,\,\,\,\,\,\,\,
\end{array}
$$

The result of this calculation is therefore $$x^7 + x^2 + x$$. This answer can be verified as correct (assuming my code is correct) by placing the following python code in the \__main__ section of the script written for Ex4.3:

{% highlight python %}

    mod = Mod2Polynomial([1, 1, 0, 1, 1, 0, 0, 0, 1])
    a = Mod2Polynomial([1, 1, 0, 0, 1])
    b = Mod2Polynomial([1, 1, 0, 1, 1])

    print a * b % mod

{% endhighlight %}

{% include _understanding-crypto/previous-and-next-exercise.html %}
