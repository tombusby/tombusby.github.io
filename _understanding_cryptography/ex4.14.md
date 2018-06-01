---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 4 Solutions - Ex4.14"
description: "Calculating the AES S-box for Certain Values Using the Affine Mapping"
layout: post
redirect_from: /understanding-cryptography-ex4.14/
headerImage: false
projects: false
date: 2018-02-05 19:56
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 4
exercise: 14
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 4.14

Your task is to compute the S-Box, i.e., the ByteSub, values for the input bytes 29, F3 and 01, where each byte is given in hexadecimal notation.

1. First, look up the inverses using Table 4.2 to obtain values $$B^\prime$$. Now, perform the affine mapping by computing the matrix–vector multiplication and addition.
2. Verify your result using the S-Box Table 4.3.
3. What is the value of $$S(0)$$?

### Solution

*This solution is verified as correct by the fact that it produces sensible output which can be checked against Table 4.3.*

1\. We can reuse the inverses we looked up in Ex4.13:

$$ 26_{16}^{-1} = A8_{16} $$

$$ F3_{16}^{-1} = 34_{16} $$

$$ 01_{16}^{-1} = 01_{16} $$

The next step is to perform the affine mapping (which destroys the mathematical relationship inherent to the Galois Field aspect of S-box and makes it thoroughly non-linear). This involves a bitwise  matrix multiplication (AND) and a bitwise XOR which is defined as follows:

<div style="text-align: center;">
<script type="math/tex">
\left(
\begin{array}{c}
b_0 \\
b_1 \\
b_2 \\
b_3 \\
b_4 \\
b_5 \\
b_6 \\
b_7 \\
\end{array}
\right)
\equiv
\left(
\begin{array}{c}
1 & 0 & 0 & 0 & 1 & 1 & 1 & 1 \\
1 & 1 & 0 & 0 & 0 & 1 & 1 & 1 \\
1 & 1 & 1 & 0 & 0 & 0 & 1 & 1 \\
1 & 1 & 1 & 1 & 0 & 0 & 0 & 1 \\
1 & 1 & 1 & 1 & 1 & 0 & 0 & 0 \\
0 & 1 & 1 & 1 & 1 & 1 & 0 & 0 \\
0 & 0 & 1 & 1 & 1 & 1 & 1 & 0 \\
0 & 0 & 0 & 1 & 1 & 1 & 1 & 1
\end{array}
\right)
\left(
\begin{array}{c}
b_0 \prime \\
b_1 \prime \\
b_2 \prime \\
b_3 \prime \\
b_4 \prime \\
b_5 \prime \\
b_6 \prime \\
b_7 \prime \\
\end{array}
\right)
+
\left(
\begin{array}{c}
1 \\
1 \\
0 \\
0 \\
0 \\
1 \\
1 \\
0
\end{array}
\right)
</script>
</div>

It should be noted here that $$b_0^\prime$$ is the least significant (rightmost) bit which may be the reverse of your intuition.

To start with $$26_{16}$$, we already know the values for $$b_i^\prime$$ because of the lookup we did (and verified) in the previous exercise. Its inverse is $$A8_{16}$$.

$$ 26_{16}^{-1} = A8_{16} = 10101000_2 $$

Applying the matrix multiplication and the XOR, you get the following:

$$ S(26_{16}) = F7_{16} $$

Next, we calculate the inverse of $$F3_{16}$$:

$$ F3_{16}^{-1} = 34_{16} = 00110100_2 $$

Applying the matrix multiplication and the XOR, you get the following:

$$ S(F3_{16}) = 0D_{16} $$

Finally, we calculate the inverse of $$01_{16}$$:

$$ 01_{16}^{-1} = 01_{16} = 00000001_2 $$

Applying the matrix multiplication and the XOR, you get the following:

$$ S(01_{16}) = 7C_{16} $$

2\. Checking these results in Table 4.3 confirms them as correct.

3\. 0 technically has no inverse (there is no value which produces 1 when multiplied with 0), but 0 is treated as its own inverse. This means that the affine mapping will take the value of the final XOR ($$ 01100011_2 = 63_{16} $$)

These results can also be checked using the following [python script](https://github.com/tombusby/understanding-cryptography-exercises/blob/master/Chapter-04/ex4.14.py) which can perform the Affine Mapping:

{% highlight python %}
# This is backwards compared with the book because it is easier to
# do this than reverse the input bits
matrix = [
    0b11110001,
    0b11100011,
    0b11000111,
    0b10001111,
    0b00011111,
    0b00111110,
    0b01111100,
    0b11111000,
]

def affine_mapping(byte):
    output = []
    for row in matrix:
        new_row = bin(row & byte).lstrip("-0b")
        output.insert(0, str(new_row.count('1') % 2))
    return int("".join(output), 2) ^ 0b01100011

if __name__ == "__main__":
    print "S(26):", hex(affine_mapping(0xA8)).lstrip("-0x").zfill(2).upper()
    print "S(F3):", hex(affine_mapping(0x34)).lstrip("-0x").zfill(2).upper()
    print "S(01):", hex(affine_mapping(0x01)).lstrip("-0x").zfill(2).upper()
{% endhighlight %}

{% include _understanding-crypto/previous-and-next-exercise.html %}
