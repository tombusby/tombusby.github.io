---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 3 Solutions - Ex3.5"
description: "Demonstrating the Avalanche Effect of a 1-bit Input Change in DES"
layout: post
redirect_from: /understanding-cryptography-ex3.5/
headerImage: false
projects: false
date: 2017-12-19 23:49
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 3
exercise: 5
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 3.5

Remember that it is desirable for good block ciphers that a change in one input bit affects many output bits, a property that is called diffusion or the avalanche effect. We try now to get a feeling for the avalanche property of DES. We apply an input word that has a “1” at bit position 57 and all other bits as well as the key are zero. (Note that the input word has to run through the initial permutation.)

1. How many S-boxes get different inputs compared to the case when an all-zero plaintext is provided?
2. What is the minimum number of output bits of the S-boxes that will change according to the S-box design criteria?
3. What is the output after the first round?
4. How many output bit after the first round have actually changed compared to the case when the plaintext is all zero? (Observe that we only consider a single round here. There will be more and more output differences after every new round. Hence the term avalanche effect.)

### Solution

*This solution is verified as correct by the official [Solutions for Odd-Numbered Questions](http://wiki.crypto.rub.de/Buch/en/download/Understanding_Cryptography_Odd_Solutions.pdf) manual.*

1\. Where $$x$$ is the plaintext, $$IP(x)$$ maps bit 57 to bit 33. This means that $$L_0 = 0$$ and $$R_0 = 2^{31}$$.

When calculating $$f(R_0)$$ we can, as in Ex3.3, discount the 0 key (since $$ a \oplus 0 = a $$).

$$E(R_0)$$ maps bit 1 to bit 2 and 48.

This means that $$S_{2-7}$$ all take 0 as input. $$S_1$$ now takes $$010000_2$$ as input, and $$S_8$$ takes $$000001_2$$.

2\. As per the design criteria, the minimum number of output bits that will be changed (per S-box) as a result of a 1 bit change in input is 2.

3\. The output of the S-boxes is as follows:

<div style="text-align: center;">
<script type="math/tex">
\begin{array}{c|c}
S_1 & S_2 & S_3 & S_4 & S_5 & S_6 & S_7 & S_8 \\ \hline
0011 & 1111 & 1010 & 0111 & 0010 & 1100 & 0100 & 0001
\end{array}
</script>
</div>

This is then permuted by $$P$$ to produce the following:

$$ 11010000010110000101101110011110_2 $$

This value is then XORed with $$L_0$$ to produce $$R_1$$ but this step can be skipped because we know that $$L_0$$ is 0.

As such, the computed values of $$L_1$$ and $$R_1$$ are:

$$ L_1 = R_0 = 08000000_{16} $$

$$ R_1 = D0585B9E_{16} $$

4\. We now compare the binary output of $$R_1$$ with that produced during Ex 3.3:

$$ 11010000010110000101101110011110_2 $$

$$\oplus$$

$$ 11011000110110001101101110111100_2 $$

$$ = $$

$$ 00001000100000001000000000100010_2 $$

We can see clearly that 5 output bits of $$R_1$$ are now different, caused by a change of one input bit.

Remember that $$L_1$$ is simply $$R_0$$ wired through unchanged. It therefore also reflects the single changed input bit of $$R_0$$.

This means that the total number of output bits changed after one round is 6, since $$L_1$$ would be 0 otherwise.

{% include _understanding-crypto/previous-and-next-exercise.html %}
