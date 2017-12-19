---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 3 Solutions - Ex3.3"
description: "Calculating the Output of the First Round of DES Encryption with All 0s"
layout: post
headerImage: false
projects: false
date: 2017-12-19 21:25
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 3
exercise: 3
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 3.3

What is the output of the first round of the DES algorithm when the plaintext and the key are both all zeros?

### Solution

*This solution is verified as correct by the official [Solutions for Odd-Numbered Questions](http://wiki.crypto.rub.de/Buch/en/download/Understanding_Cryptography_Odd_Solutions.pdf) manual.*

Because all inputs are 0 and $$IP$$, $$PC-1$$, $$PC-2$$ and the subkey rotations are all simple permutations, we can discount them knowing that they will output 0s.

Because the subkeys consist of all 0s, we can discount them from the $$f$$ function calculation (since $$ a \oplus 0 = a $$). We can also discount the $$E$$ expansion since it's guaranteed to produce all 0s.

$$ L_0 = 0 $$

$$ L_1 = R_0 = 0 $$

$$ R_1 $$ is calculated as follows:

$$ R_1 = L_0 \oplus f(R_0) $$

$$ = 0 \oplus f(0) $$

$$ = f(0) $$

Calculating the $$f$$ function is as follows:

$$ f(0) = P(S_1(0), S_2(0), S_3(0), S_4(0), S_5(0), S_6(0), S_7(0), S_8(0)) $$

We will call the intermediate value before $$P$$ is applied $$R_1^\prime$$ and it after applying all the S-boxes, it is as follows:

$$ 11101111101001110010110001001101_2 $$

To finish calculating $$R_1$$ we need to apply $$P(R_1^\prime)$$:

$$ P(R_1^\prime) = 11011000110110001101101110111100_2 $$

Therefore:

$$ L_1 = 00000000_{16} $$

$$ R_1 = D8D8DBBC_{16} $$

{% include _understanding-crypto/previous-and-next-exercise.html %}
