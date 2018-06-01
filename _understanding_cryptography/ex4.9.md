---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 4 Solutions - Ex4.9"
description: "Calculating the Output of the First Round of AES Encryption with All 1s"
layout: post
redirect_from: /understanding-cryptography-ex4.9/
headerImage: false
projects: false
date: 2018-01-08 23:47
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 4
exercise: 9
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 4.9

We consider AES with 128-bit block length and 128-bit key length. What is the output of the first round of AES if the plaintext consists of 128 ones, and the first subkey also consists of 128 ones? You can write your final results in a rectangular array format if you wish.

*Note: It seems that the official solution for this question discounts the initial Key Addition that would have XORed the all-1s plaintext with the all-1s zeroth subkey. This would have given all-0s input to the S-boxes and completely changed the outcome. To stay in line with the solutions manual I'll also be ignoring the inital Key Addition of $$k_0$$.*

### Solution

*This solution is verified as correct by the official [Solutions for Odd-Numbered Questions](http://wiki.crypto.rub.de/Buch/en/download/Understanding_Cryptography_Odd_Solutions.pdf) manual.*

We know that all the S-boxes are the same function, and that the input will the same in each case ($$FF_{16}$$). As such, using Table 4.3 from the book, the state array ($$A_0 - A_{15}$$) produced by Byte Substitution is as follows:

<div style="text-align: center;">
<script type="math/tex">
\left[
\begin{array}{cccc}
16_{16} & 16_{16} & 16_{16} & 16_{16} \\
16_{16} & 16_{16} & 16_{16} & 16_{16} \\
16_{16} & 16_{16} & 16_{16} & 16_{16} \\
16_{16} & 16_{16} & 16_{16} & 16_{16}
\end{array}
\right]
</script>
</div>

ShiftRows can be discounted since all cells in the array are the same and therefore permutating them does not change anything.

Given that all the cells are the same, the MixColumn matrix multiplication can be expressed as follows for each column, since $$ax + bx + cx + dx = x(a + b + c + d)$$:

<div style="text-align: center; margin-bottom: 25px">
<script type="math/tex">
\left[
\begin{array}{cccc}
(02 + 03 + 01 + 01) \times 16_{16} \\
(01 + 02 + 03 + 01) \times 16_{16} \\
(01 + 01 + 02 + 03) \times 16_{16} \\
(03 + 01 + 01 + 02) \times 16_{16}
\end{array}
\right]
</script>
</div>

It's worth noting that in $$GF(2^8)$$, $$01 + 01 + 02 + 03 = 01$$. As such, in this special case, the MixColumn operation will also not affect the state (since $$01 \times 16 = 16$$).

All that remains now is to bitwise XOR the round key onto the state:

<div style="text-align: center;">
<script type="math/tex">
\left[
\begin{array}{cccc}
16_{16} & 16_{16} & 16_{16} & 16_{16} \\
16_{16} & 16_{16} & 16_{16} & 16_{16} \\
16_{16} & 16_{16} & 16_{16} & 16_{16} \\
16_{16} & 16_{16} & 16_{16} & 16_{16}
\end{array}
\right]
</script>
</div>

$$ \oplus $$

<div style="text-align: center;">
<script type="math/tex">
\left[
\begin{array}{cccc}
FF_{16} & FF_{16} & FF_{16} & FF_{16} \\
FF_{16} & FF_{16} & FF_{16} & FF_{16} \\
FF_{16} & FF_{16} & FF_{16} & FF_{16} \\
FF_{16} & FF_{16} & FF_{16} & FF_{16}
\end{array}
\right]
</script>
</div>

$$ = $$

<div style="text-align: center;">
<script type="math/tex">
\left[
\begin{array}{cccc}
E9_{16} & E9_{16} & E9_{16} & E9_{16} \\
E9_{16} & E9_{16} & E9_{16} & E9_{16} \\
E9_{16} & E9_{16} & E9_{16} & E9_{16} \\
E9_{16} & E9_{16} & E9_{16} & E9_{16}
\end{array}
\right]
</script>
</div>

This is the state after the first round of encryption.

{% include _understanding-crypto/previous-and-next-exercise.html %}
