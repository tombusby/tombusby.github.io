---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 4 Solutions - Ex4.4"
description: "Performing Multiplication and Reduction in GF(2⁴)"
layout: post
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

The influence of the reduction polynonomial choice is that different primitive polynomials will produce different results for any multiplications that require modulo reduction to bring them back into the field.

1\. Since a naive multiplication would produce an order 5 polynomial, this must be reduced using the reduction polynomial. The naive multiplication produces this output:

$$ (x^2 +1)(x^3 +x^2 +1) = x^5 + x^4 + x^3 + 1 $$

In order to finish, we must calculate $$ x^5 + x^4 + x^3 + 1 \,\mathrm{mod}\, x^4 +x +1 $$:

$$
\require{enclose}
\begin{array}{r}
                x + 1  \\[-3pt]
x^4 +x +1 \enclose{longdiv}{x^5 + x^4 + x^3 + 0 + 0 + 1} \\
\oplus\,\,\underline{x^5 +\,\,0\,\, + 0 + x^2 + x + 0 } \\
x^4 + x^3 + x^2 + x + 1 \\
\oplus\,\,\underline{x^4 + 0\,\,+ 0\,\,+ x + 1 } \\
x^3\, + x^2\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,
\end{array}
$$

The remainder of this division is $$ x^3 + x^2 $$. This answer can be verified as correct by placing the following python code in the \__main__ section of the script written for Ex4.3:

{% highlight python %}

    mod = Mod2Polynomial([1, 1, 0, 0, 1])
    a = Mod2Polynomial([1, 0, 1])
    b = Mod2Polynomial([1, 0, 1, 1])

    print a * b % mod

{% endhighlight %}

2\. No modulo reduction is required to calculate this multiplication (since the output is still in the field):

$$ (x^2 +1)(x+1) = x^3 + x^2 + x + 1 $$

{% include _understanding-crypto/previous-and-next-exercise.html %}
