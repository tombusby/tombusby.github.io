---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 4 Solutions - Ex4.10"
description: "Calculating the State After One Round for AES"
layout: post
redirect_from: /understanding-cryptography-ex4.10/
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

*Feedback kindly provided by Lisa Roy in the comment section has allowed me to correct some errors in earlier versions of this answer. Given that we have now converged upon a common answer, I can be reasonably sure of correctness.*

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
F4 & 9B & 1F & 57 \\ \hline
CC & 60 & 01 & 90 \\ \hline
6B & AA & 0F & A2 \\ \hline
53 & 8F & 04 & D3 \\ \hline
\end{array}
</script>
</div>

Therefore, the output of the first round of encryption is as follows (remembering to read it out column-wise):

$$ F4CC6B539B60AA8F1F010F045790A2D3_{16} $$

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
F4 & 9B & 1F & 57 \\ \hline
CC & 60 & 01 & 90 \\ \hline
6B & AA & 0F & A2 \\ \hline
53 & 8F & 04 & D3 \\ \hline
\end{array}
</script>
</div>

Therefore, the output of the first round of encryption is as follows (remembering to read it out column-wise):

$$ DCD87F6F9B60AA8F1F010F045790A2D3_{16} $$

3\. We can see how many output bits have been altered by XORing the two output values together. This produces:

$$ 2814143c000000000000000000000000_{16} $$

In this form, we can clearly see that only the first column is altered after the first round.

$$ 2814143c_{16} = 101000000101000001010000111100_2 $$

The 1s in the binary above correspond to output bits which have changed. There are ten of them, so therefore the number of output bits which have changed due to a 1 bit change in input, in this instance, is 10 after the first round. The ShiftRows operations in further rounds will diffuse this effect to other columns.

I wrote a [python script](https://github.com/tombusby/understanding-cryptography-exercises/blob/master/Chapter-04/ex4.10.py) which can perform the operations involved in computing an AES round:

{% highlight python %}
from pprint import pprint

from gf import Mod2Polynomial

def pretty_print_state(state):
    def format(hex_value):
        return "{0:0{1}x}".format(hex_value, 2).upper()
    pprint([[format(i) for i in row] for row in state])

def ByteSubstitution(state):
    sbox = [
        0x63, 0x7C, 0x77, 0x7B, 0xF2, 0x6B, 0x6F, 0xC5, 0x30, 0x01, 0x67, 0x2B, 0xFE, 0xD7, 0xAB, 0x76,
        0xCA, 0x82, 0xC9, 0x7D, 0xFA, 0x59, 0x47, 0xF0, 0xAD, 0xD4, 0xA2, 0xAF, 0x9C, 0xA4, 0x72, 0xC0,
        0xB7, 0xFD, 0x93, 0x26, 0x36, 0x3F, 0xF7, 0xCC, 0x34, 0xA5, 0xE5, 0xF1, 0x71, 0xD8, 0x31, 0x15,
        0x04, 0xC7, 0x23, 0xC3, 0x18, 0x96, 0x05, 0x9A, 0x07, 0x12, 0x80, 0xE2, 0xEB, 0x27, 0xB2, 0x75,
        0x09, 0x83, 0x2C, 0x1A, 0x1B, 0x6E, 0x5A, 0xA0, 0x52, 0x3B, 0xD6, 0xB3, 0x29, 0xE3, 0x2F, 0x84,
        0x53, 0xD1, 0x00, 0xED, 0x20, 0xFC, 0xB1, 0x5B, 0x6A, 0xCB, 0xBE, 0x39, 0x4A, 0x4C, 0x58, 0xCF,
        0xD0, 0xEF, 0xAA, 0xFB, 0x43, 0x4D, 0x33, 0x85, 0x45, 0xF9, 0x02, 0x7F, 0x50, 0x3C, 0x9F, 0xA8,
        0x51, 0xA3, 0x40, 0x8F, 0x92, 0x9D, 0x38, 0xF5, 0xBC, 0xB6, 0xDA, 0x21, 0x10, 0xFF, 0xF3, 0xD2,
        0xCD, 0x0C, 0x13, 0xEC, 0x5F, 0x97, 0x44, 0x17, 0xC4, 0xA7, 0x7E, 0x3D, 0x64, 0x5D, 0x19, 0x73,
        0x60, 0x81, 0x4F, 0xDC, 0x22, 0x2A, 0x90, 0x88, 0x46, 0xEE, 0xB8, 0x14, 0xDE, 0x5E, 0x0B, 0xDB,
        0xE0, 0x32, 0x3A, 0x0A, 0x49, 0x06, 0x24, 0x5C, 0xC2, 0xD3, 0xAC, 0x62, 0x91, 0x95, 0xE4, 0x79,
        0xE7, 0xC8, 0x37, 0x6D, 0x8D, 0xD5, 0x4E, 0xA9, 0x6C, 0x56, 0xF4, 0xEA, 0x65, 0x7A, 0xAE, 0x08,
        0xBA, 0x78, 0x25, 0x2E, 0x1C, 0xA6, 0xB4, 0xC6, 0xE8, 0xDD, 0x74, 0x1F, 0x4B, 0xBD, 0x8B, 0x8A,
        0x70, 0x3E, 0xB5, 0x66, 0x48, 0x03, 0xF6, 0x0E, 0x61, 0x35, 0x57, 0xB9, 0x86, 0xC1, 0x1D, 0x9E,
        0xE1, 0xF8, 0x98, 0x11, 0x69, 0xD9, 0x8E, 0x94, 0x9B, 0x1E, 0x87, 0xE9, 0xCE, 0x55, 0x28, 0xDF,
        0x8C, 0xA1, 0x89, 0x0D, 0xBF, 0xE6, 0x42, 0x68, 0x41, 0x99, 0x2D, 0x0F, 0xB0, 0x54, 0xBB, 0x16,
    ]
    # I had originally split it into two coordinates, but this is completely unnecessary for reasons that
    # become clear once you give a few seconds thought to it.
    return [[sbox[i] for i in row] for row in state]

def ShiftRows(state):
    return [
        state[0],
        # These are the equivalent left-rotations
        state[1][1:] + state[1][:1],
        state[2][2:] + state[2][:2],
        state[3][3:] + state[3][:3],
    ]

def MixColumns(state):
    reduction_polynomial = Mod2Polynomial([1, 1, 0, 1, 1, 0, 0, 0, 1])
    def do_mult(a, b):
        return (a * b) % reduction_polynomial
    def byte_to_polynomial(a):
        list_rep = list(bin(a).lstrip("-0b"))
        list_rep.reverse()
        return Mod2Polynomial([int(i) for i in list_rep])
    def polynomial_to_byte(a):
        vector = [str(i) for i in a.vector]
        vector.reverse()
        return int("".join(vector), 2)
    p01 = byte_to_polynomial(1)
    p02 = byte_to_polynomial(2)
    p03 = byte_to_polynomial(3)
    state = map(list, zip(*state))
    new_state = []
    for row in state:
        row = [byte_to_polynomial(i) for i in row]
        new_row = [
            do_mult(p02, row[0]) + do_mult(p03, row[1]) + do_mult(p01, row[2]) + do_mult(p01, row[3]),
            do_mult(p01, row[0]) + do_mult(p02, row[1]) + do_mult(p03, row[2]) + do_mult(p01, row[3]),
            do_mult(p01, row[0]) + do_mult(p01, row[1]) + do_mult(p02, row[2]) + do_mult(p03, row[3]),
            do_mult(p03, row[0]) + do_mult(p01, row[1]) + do_mult(p01, row[2]) + do_mult(p02, row[3]),
        ]
        new_state.append([polynomial_to_byte(i) for i in new_row])
    new_state = map(list, zip(*new_state))
    return new_state

def KeyAddition(state, subkey):
    pairs = [zip(a, b) for a, b in zip(state, subkey)]
    return [[a ^ b for a, b in row] for row in pairs]

def encryption_round(state, k1):
    print "\nByteSubstitution:"
    state = ByteSubstitution(state)
    pretty_print_state(state)
    print "\nShiftRows:"
    state = ShiftRows(state)
    pretty_print_state(state)
    print "\nMixColumns:"
    state = MixColumns(state)
    pretty_print_state(state)
    print "\nk1 KeyAddition:"
    state = KeyAddition(state, k1)
    pretty_print_state(state)
    return state

if __name__ == "__main__":
    # Credit to Lisa Roy for pointing out this matrix is filled column-wise.
    k1 = [
        [0xA0, 0x88, 0x23, 0x2A],
        [0xFA, 0x54, 0xA3, 0x6C],
        [0xFE, 0x2C, 0x39, 0x76],
        [0x17, 0xB1, 0x39, 0x05],
    ]
    state1 = [
        [0x2A, 0x28, 0xAB, 0x09], # Q1
        # [0x2B, 0x28, 0xAB, 0x09], # Q2
        [0x7E, 0xAE, 0xF7, 0xCF],
        [0x15, 0xD2, 0x15, 0x4F],
        [0x16, 0xA6, 0x88, 0x3C],
    ]
    print "\nk0 KeyAddition:"
    pretty_print_state(state1)
    state1 = encryption_round(state1, k1)
{% endhighlight %}

{% include _understanding-crypto/previous-and-next-exercise.html %}
