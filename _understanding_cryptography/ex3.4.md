---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 3 Solutions - Ex3.4"
description: "Calculating the Output of the First Round of DES Encryption with All 1s"
layout: post
redirect_from: /understanding-cryptography-ex3.4/
headerImage: false
projects: false
date: 2017-12-19 23:00
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 3
exercise: 4
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 3.4

What is the output of the first round of the DES algorithm when the plaintext and the key are both all ones?

### Solution

*I haven't yet verified this solution independently. If you spot any mistakes, please leave a comment in the Disqus box at the bottom of the page.*

Because all inputs are 1s and $$IP$$, $$PC-1$$, $$PC-2$$ and the subkey rotations are all simple permutations, we can discount them knowing that they will output all 1s.

Because the subkeys consist of all 1s, we can discount them from the $$f$$ function calculation by inverting the S-box input (since $$ a \oplus 1 = a^{-1} $$ where $$a^{-1}$$ denotes a bitwise NOT operation). We can also discount the $$E$$ expansion since it's guaranteed to produce all 1s.

$$ L_0 = FFFFFFFF_{16} $$

$$ L_1 = R_0 = FFFFFFFF_{16} $$

$$ R_1 $$ is calculated as follows:

$$ R_1 = L_0 \oplus f(R_0) $$

$$ = FFFFFFFF_{16} \oplus f(FFFFFFFF_{16}) $$

$$ = (f(FFFFFFFF_{16}))^{-1} $$

Because the input to the S-boxes is the all-1s $$E$$ output inverted by the all-1s key, calculating the $$f$$ function is the same as for Ex3.3:

$$ f(FFFFFFFF_{16}) = P(S_1(0), S_2(0), S_3(0), S_4(0), S_5(0), S_6(0), S_7(0), S_8(0)) $$

We will call the intermediate value before $$P$$ is applied $$R_1^\prime$$ and after applying all the S-boxes, it is as follows:

$$ 11101111101001110010110001001101_2 $$

To finish calculating $$R_1$$ we need to apply $$P(R_1^\prime)$$ and then invert the result:

$$ P(R_1^\prime) = 11011000110110001101101110111100_2 $$

$$ (P(R_1^\prime))^{-1} = 00100111001001110010010001000011_2 $$

Therefore:

$$ L_1 = FFFFFFFF_{16} $$

$$ R_1 = 27272443_{16} $$

*Note*: The output for $$R_1$$ is the same as for Ex3.3, but inverted.

{% include _understanding-crypto/previous-and-next-exercise.html %}
