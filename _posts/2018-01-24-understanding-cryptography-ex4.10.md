---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 4 Solutions - Ex4.10"
description: "Calculating the State After One Round for AES"
layout: post
headerImage: false
projects: false
date: 2018-01-24 23:28
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 4
exercise: 10
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 4.10

In the following, we check the diffusion properties of AES after a single round. Let

$$W = (w_0,w_1,w_2,w_3) =$$

$$
01000000_{16} \\
00000000_{16} \\
00000000_{16} \\
00000000_{16}
$$

be the input in 32-bit chunks to a 128-bit AES. The subkeys for the computation of the result of the first round of AES are $$W_0,...,W_7$$ with 32 bits each are given by

$$
W_0 = 2B7E1516_{16} \\
W_1 = 28AED2A6_{16} \\
W_2 = ABF71588_{16} \\
W_3 = 09CF4F3C_{16} \\
W_4 = A0FAFE17_{16} \\
W_5 = 88542CB1_{16} \\
W_6 = 23A33939_{16} \\
W_7 = 2A6C7605_{16}
$$

Use this book to figure out how the input is processed in the first round (e.g., SBoxes). For the solution, you might also want to write a short computer program or use an existing one. In any case, indicate all intermediate steps for the computation of _ShiftRows_, _SubBytes_ and _MixColumns_!

1. Compute the output of the first round of AES to the input W and the subkeys $$W_0,...,W_7$$.
2. Compute the output of the first round of AES for the case that _all_ input bits are zero.
3. How many output bits have changed? Remark that we only consider a single round — after every further round, more output bits will be affected (_avalanche effect_).

### Solution

*I haven't yet verified this solution independently. If you spot any mistakes, please leave a comment in the Disqus box at the bottom of the page.*

*Note*: From now on the state is represented as a 4x4 grid of bytes expressed in hexadecimal. This makes other transformations easier to represent also. The ordering of the bytes within the grid is as follows:

<div style="text-align: center;">
<script type="math/tex">
\begin{array}{|c|c|}
\hline
A_0 & A_4 & A_8 & A_{12} \\ \hline
A_1 & A_5 & A_9 & A_{13} \\ \hline
A_2 & A_6 & A_{10} & A_{14} \\ \hline
A_3 & A_7 & A_{11} & A_{15} \\ \hline
\end{array}
</script>
</div>

1\. This question is slightly confusingly expressed and it took me a while to figure out what is meant. The keys $$k_0$$ and $$k_1$$ are given as a single array ($$W_0,...,W_7$$). In fact, $$k_0 = W_0,...,W_3$$ and $$k_1 = W_4,...,W_7$$. As such the first step is the initial $$k_0$$ key adition which occurs before we get into applying the rounds. The first word of the input is the only one that isn't all-zeroes. As such, the state after $$k_0$$ addition is:

*Note*: From now on the state is represented as a 4x4 grid of bytes expressed in hexadecimal. This makes other transformations easier to represent also.

<div style="text-align: center;">
<script type="math/tex">
\begin{array}{|c|c|}
\hline
2A & 28 & AB & 09 \\ \hline
7E & AE & F7 & CF \\ \hline
15 & D2 & 15 & 4F \\ \hline
16 & A6 & 88 & 3C \\ \hline
\end{array}
</script>
</div>

It's worth noting that the four columns correspond exactly to the $$k_0 = (W_0,...,W_3)$$ subkey bytes with the exception of the first cell, which was slightly altered by the XOR operation.

After applying the ByteSubstitution (S-box) layer, the state is as follows:

<div style="text-align: center;">
<script type="math/tex">
\begin{array}{|c|c|}
\hline
E5 & 34 & 62 & 01 \\ \hline
F3 & E4 & 68 & 8A \\ \hline
59 & B5 & 59 & 84 \\ \hline
47 & 24 & C4 & EB \\ \hline
\end{array}
</script>
</div>

The ShiftRows layer is a very simple transformation. The first row remains unchainged. The other rows are rotated right by a number of positions; the second by 3, the third by 2 and the fourth by 1. This gives the following:

<div style="text-align: center;">
<script type="math/tex">
\begin{array}{|c|c|}
\hline
E5 & 34 & 62 & 01 \\ \hline
E4 & 68 & 8A & F3 \\ \hline
59 & 84 & 59 & B5 \\ \hline
EB & 47 & 24 & C4 \\ \hline
\end{array}
</script>
</div>

The final transformation (other than the $$k_1$$ addition) is the MixColumn layer. This involves a Galois Extension Field matrix multiplication with the following description:

<div style="text-align: center;">
<script type="math/tex">
\left(
\begin{array}{c}
C_0 \\
C_1 \\
C_2 \\
C_3 \\
\end{array}
\right)
=
\left(
\begin{array}{c}
02 & 03 & 01 & 01 \\
01 & 02 & 03 & 01 \\
01 & 01 & 02 & 03 \\
03 & 01 & 01 & 02 \\
\end{array}
\right)
\left(
\begin{array}{c}
B_0 \\
B_5 \\
B_{10} \\
B_{15} \\
\end{array}
\right)
</script>
</div>

The C values refer to the outputted column. The B values refer to the input columns which were the output of the ShiftRows layer. The indexes here are preserved from prior to the shift. Each new column can be calculated left-to-right using this procedure.

it should also be noted that $$03$$ here would refer to $$ x + 1 $$ in $$GF(2^8)$$. The same logic is used to turn a hex byte into a Galois Extension Field polynomial. The reduction polynomial is the AES primitive polynomial:

$$ P(x) = x^8 + x^4 + x^3 + x + 1 $$

As such the calculation to be performed is as follows for each of the columns:

<div style="text-align: center; margin-bottom: 20px">
<script type="math/tex">
\left(
\begin{array}{c}
02 \times E5 + 03 \times E4 + 01 \times 59 + 01 \times EB \\
01 \times E5 + 02 \times E4 + 03 \times 59 + 01 \times EB \\
01 \times E5 + 01 \times E4 + 02 \times 59 + 03 \times EB \\
03 \times E5 + 01 \times E4 + 01 \times 59 + 02 \times EB \\
\end{array}
\right)
</script>
</div>

<div style="text-align: center; margin-bottom: 20px">
<script type="math/tex">
\left(
\begin{array}{c}
02 \times 34 + 03 \times 68 + 01 \times 84 + 01 \times 47 \\
01 \times 34 + 02 \times 68 + 03 \times 84 + 01 \times 47 \\
01 \times 34 + 01 \times 68 + 02 \times 84 + 03 \times 47 \\
03 \times 34 + 01 \times 68 + 01 \times 84 + 02 \times 47 \\
\end{array}
\right)
</script>
</div>

<div style="text-align: center; margin-bottom: 20px">
<script type="math/tex">
\left(
\begin{array}{c}
02 \times 62 + 03 \times 8A + 01 \times 59 + 01 \times 24 \\
01 \times 62 + 02 \times 8A + 03 \times 59 + 01 \times 24 \\
01 \times 62 + 01 \times 8A + 02 \times 59 + 03 \times 24 \\
03 \times 62 + 01 \times 8A + 01 \times 59 + 02 \times 24 \\
\end{array}
\right)
</script>
</div>

<div style="text-align: center; margin-bottom: 20px">
<script type="math/tex">
\left(
\begin{array}{c}
02 \times 01 + 03 \times F3 + 01 \times B5 + 01 \times C4 \\
01 \times 01 + 02 \times F3 + 03 \times B5 + 01 \times C4 \\
01 \times 01 + 01 \times F3 + 02 \times B5 + 03 \times C4 \\
03 \times 01 + 01 \times F3 + 01 \times B5 + 02 \times C4 \\
\end{array}
\right)
</script>
</div>

This produces the following state:

<div style="text-align: center;">
<script type="math/tex">
\begin{array}{|c|c|}
\hline
54 & 13 & 3C & 7D \\ \hline
36 & 34 & A2 & FC \\ \hline
95 & 86 & 36 & D4 \\ \hline
44 & 3E & 3D & D6 \\ \hline
\end{array}
</script>
</div>

All that's left to do after this is the KeyAddition layer for $$k_1 = W_4,...,W_7$$:

<div style="text-align: center;">
<script type="math/tex">
\begin{array}{|c|c|}
\hline
F4 & E9 & C2 & 6A \\ \hline
BE & 60 & 8E & 4D \\ \hline
B6 & 25 & 0F & ED \\ \hline
6E & 52 & 4B & D3 \\ \hline
\end{array}
</script>
</div>

Therefore, the output of the first round of encryption is as follows:

$$ F4BEB66EE9602552C28E0F4B6A4DEDD3_{16} $$

2\. For the case that the input is all-zeroes, the state after the $$k_0$$ key addition will be as follows (only the first byte is different from part 1):

<div style="text-align: center;">
<script type="math/tex">
\begin{array}{|c|c|}
\hline
2B & 28 & AB & 09 \\ \hline
7E & AE & F7 & CF \\ \hline
15 & D2 & 15 & 4F \\ \hline
16 & A6 & 88 & 3C \\ \hline
\end{array}
</script>
</div>

After applying the ByteSubstitution (S-box) layer, the state is as follows:

<div style="text-align: center;">
<script type="math/tex">
\begin{array}{|c|c|}
\hline
F1 & 34 & 62 & 01 \\ \hline
F3 & E4 & 68 & 8A \\ \hline
59 & B5 & 59 & 84 \\ \hline
47 & 24 & C4 & EB \\ \hline
\end{array}
</script>
</div>

After applying the ShiftRows layer, the state is as follows:

<div style="text-align: center;">
<script type="math/tex">
\begin{array}{|c|c|}
\hline
F1 & 34 & 62 & 01 \\ \hline
E4 & 68 & 8A & F3 \\ \hline
59 & 84 & 59 & B5 \\ \hline
EB & 47 & 24 & C4 \\ \hline
\end{array}
</script>
</div>

Up until this point, only one byte (in fact only one bit) has been altered in compared to part 1. The MixColumn introduces some diffusion, and after this layer is applied, the state is as follows:

<div style="text-align: center;">
<script type="math/tex">
\begin{array}{|c|c|}
\hline
7C & 13 & 3C & 7D \\ \hline
22 & 34 & A2 & FC \\ \hline
81 & 86 & 36 & D4 \\ \hline
78 & 3E & 3D & D6 \\ \hline
\end{array}
</script>
</div>

The first column of the state now has a different set of values than in part 1, the others are unaltered. All that's left to do after this is the KeyAddition layer for $$k_1 = W_4,...,W_7$$:

<div style="text-align: center;">
<script type="math/tex">
\begin{array}{|c|c|}
\hline
DC & E9 & C2 & 6A \\ \hline
AA & 60 & 8E & 4D \\ \hline
A2 & 25 & 0F & ED \\ \hline
52 & 52 & 4B & D3 \\ \hline
\end{array}
</script>
</div>

Therefore, the output of the first round of encryption is as follows:

$$ DCAAA252E9602552C28E0F4B6A4DEDD3_{16} $$

3\. We can see how many output bits have been altered by XORing the two output values together. This produces:

$$ 2814143C000000000000000000000000_{16} $$

In this form, we can clearly see that only the first column is altered after the first round.

$$ 2814143C_{16} = 101000000101000001010000111100_2 $$

The 1s in the binary above correspond to output bits which have changed. There are ten of them, so therefore the number of output bits which have changed due to a 1 bit change in input, in this instance, is 10 after the first round.

{% include _understanding-crypto/previous-and-next-exercise.html %}
