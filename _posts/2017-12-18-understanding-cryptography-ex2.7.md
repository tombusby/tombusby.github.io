---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 1 Solutions - Ex2.7"
description: "Computing the First Two Bytes of Output from an LSFR with an 8th Degree Polynomial"
layout: post
headerImage: false
projects: false
date: 2017-12-18 21:42
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 2
exercise: 7
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 2.7

Compute the first two output bytes of the LFSR of degree 8 and the feedback polynomial from Table 2.3 where the initialization vector has the value FF in hexadecimal notation.

*Note*: The polynomial referred to is $$x^8 + x^4 + x^3 + x + 1$$.

### Solution

*This solution is verified as correct by the official [Solutions for Odd-Numbered Questions](http://wiki.crypto.rub.de/Buch/en/download/Understanding_Cryptography_Odd_Solutions.pdf) manual.*

This LFSR derived from this polynomial can be visualised as such:

![LFSR]({{ site.url }}/assets/images/understanding-crypto/ex2-7-lfsr.png){: .image-center}

The sequence generated by $$FF_{16}$$, which is $$ 11111111_2 $$, is as follows:

<div style="text-align: center;">
<script type="math/tex">
\begin{array}{c c c c c c c c|c}
s_7 & s_6 & s_5 & s_4 & s_3 & s_2 & s_1 & s_0 & \text{Output} \\ \hline
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
0 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
0 & 0 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
0 & 0 & 0 & 1 & 1 & 1 & 1 & 1 & 1 \\
0 & 0 & 0 & 0 & 1 & 1 & 1 & 1 & 1 \\
1 & 0 & 0 & 0 & 0 & 1 & 1 & 1 & 1 \\
0 & 1 & 0 & 0 & 0 & 0 & 1 & 1 & 1 \\
0 & 0 & 1 & 0 & 0 & 0 & 0 & 1 & 1 \\
1 & 0 & 0 & 1 & 0 & 0 & 0 & 0 & 0 \\
1 & 1 & 0 & 0 & 1 & 0 & 0 & 0 & 0 \\
1 & 1 & 1 & 0 & 0 & 1 & 0 & 0 & 0 \\
0 & 1 & 1 & 1 & 0 & 0 & 1 & 0 & 0 \\
0 & 0 & 1 & 1 & 1 & 0 & 0 & 1 & 1 \\
1 & 0 & 0 & 1 & 1 & 1 & 0 & 0 & 0 \\
0 & 1 & 0 & 0 & 1 & 1 & 1 & 0 & 0 \\
0 & 0 & 1 & 0 & 0 & 1 & 1 & 1 & 1
\end{array}
</script>
</div>

The resulting first two output bytes are $$ 1001000011111111_2 = {90FF}_{16} $$.

{% include _understanding-crypto/previous-and-next-exercise.html %}