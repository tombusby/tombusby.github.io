---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 4 Solutions - Ex4.11"
description: "Deriving Equations for the Constant Multiplications Done in the MixColumn GF Computations"
layout: post
headerImage: false
projects: false
date: 2018-02-05 00:28
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 4
exercise: 11
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 4.11

The MixColumn transformation of AES consists of a matrix–vector multiplication in the field $$GF(2^8)$$ with $$P(x) = x^8 + x^4 + x^3 + x + 1$$. Let $$b = (b_7x^7 + ... + b_0)$$ be one of the (four) input bytes to the vector–matrix multiplication. Each input byte is multiplied with the constants 01, 02 and 03. Your task is to provide exact equations for computing those three constant multiplications. We denote the result by $$d = (d_7x^7 + ... + d_0)$$.

1. Equations for computing the 8 bits of $$d = 01 \times b$$.
2. Equations for computing the 8 bits of $$d = 02 \times b$$.
3. Equations for computing the 8 bits of $$d = 03 \times b$$.

*Note*: The AES specification uses “01” to represent the polynomial $$1$$, “02” to represent the polynomial $$x$$, and “03” to represent $$x+1$$.

### Solution

*This solution is verified as correct by the official [Solutions for Odd-Numbered Questions](http://wiki.crypto.rub.de/Buch/en/download/Understanding_Cryptography_Odd_Solutions.pdf) manual.*

1\. This is a trivial case. A multiplication by 1 does not change the result in a Galois Extension Field, just as in regular multiplication:

$$ d = 01 \times b = b $$ 

2\. 02 represents the polynomial $$x$$. This is essentially a left-shift by one operation when multiplied (ignoring the effects of the reduction). The first thing to do is the naive multiplication by $$x$$:

$$ x(b_7x^7 + b_6x^6 + b_5x^5 + b_4x^4 + b_3x^3 + b_2x^2 + b_1x + b_0) $$

$$ = $$

$$ b_7x^8 + b_6x^7 + b_5x^6 + b_4x^5 + b_3x^4 + b_2x^3 + b_1x^2 + b_0x $$

The next step is do do a modulo reduction using $$P(x) = x^8 + x^4 + x^3 + x + 1$$

$$
\require{enclose}
\begin{array}{r}
                b_7 \\[-3pt]
P(x) \enclose{longdiv}{b_7x^8 + b_6x^7 + b_5x^6 + b_4x^5 + b_3x^4 + b_2x^3 + b_1x^2 + b_0x + \,0\,} \\
\oplus\,\,\underline{b_7x^8 + \,\,\,0\,\,\,\, + \,\,\,0\,\,\,\, + \,\,\,0\,\,\,\, + b_7x^4 + b_7x^3 + \,\,\,0\,\,\,\, + b_7x + b_7\,} \\
... \\
\end{array}
$$

The end result is therefore:

$$ (b_6 \oplus b_7)x^7 + b_5x^6 + b_4x^5 + (b_3 \oplus b_7)x^4 + (b_2 \oplus b_7)x^3 + b_1x^2 + (b_0 \oplus b_7)x + b_7 $$

3\. 03 represents the polynomial $$x + 1$$. As such we can convert any multiplication by 03 into the following form:

$$ (x + 1)b = xb + b $$

As such, we can use our answers from part one:

$$ b_6x^7 + b_5x^6 + b_4x^5 + (b_3 \oplus b_7)x^4 + (b_2 \oplus b_7)x^3 + b_1x^2 + (b_0 \oplus b_7)x + b_7 $$
 
$$ \oplus $$

$$ b_7x^7 + b_6x^6 + b_5x^5 + b_4x^4 + b_3x^3 + b_2x^2 + b_1x + b_0 $$

$$ = $$

$$ (b_6 \oplus b_7)x^7 + (b_5 \oplus b_6)x^6 + (b_4 \oplus b_5)x^5 + (b_3 \oplus b_4 \oplus b_7)x^4 + \\
(b_2 \oplus b_3 \oplus b_7)x^3 + (b_1 \oplus b_2)x^2 + (b_0 \oplus b_1 \oplus b_7)x + (b_0 \oplus b_7) $$

{% include _understanding-crypto/previous-and-next-exercise.html %}
