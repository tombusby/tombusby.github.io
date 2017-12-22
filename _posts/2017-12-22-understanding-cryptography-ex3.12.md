---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 3 Solutions - Ex3.12"
description: "Calculating Equivalent Key Lengths for DES Keys With Flawed Generation Procedures"
layout: post
headerImage: false
projects: false
date: 2017-12-22 01:11
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 3
exercise: 12
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 3.12

We study a real-world case in this problem. A commercial file encryption program from the early 1990s used standard DES with 56 key bits. In those days, performing an exhaustive key search was considerably harder than nowadays, and thus the key length was sufficient for some applications. Unfortunately, the implementation of the key generation was flawed, which we are going to analyze. Assume that we can test 106 keys per second on a conventional PC.

The key is generated from a password consisting of 8 characters. The key is a simple concatenation of the 8 ASCII characters, yielding 64 = 8 · 8 key bits. With the permutation PC−1 in the key schedule, the least significant bit (LSB) of each 8-bit character is ignored, yielding 56 key bits.

1. What is the size of the key space if all 8 characters are randomly chosen 8-bit ASCII characters? How long does an average key search take with a single PC?
2. How many key bits are used, if the 8 characters are randomly chosen 7-bit ASCII characters (i.e., the most significant bit is always zero)? How long does an average key search take with a single PC?
3. How large is the key space if, in addition to the restriction in Part 2, only letters are used as characters. Furthermore, unfortunately, all letters are converted to capital letters before generating the key in the software. How long does an average key search take with a single PC?

### Solution

*I haven't yet verified this solution independently. If you spot any mistakes, please leave a comment in the Disqus box at the bottom of the page.*

1\. If the key is 8 randomly chosen 8-bit ASCII characters, then the key length will be 7 bits per character (since the least significant bit will be disposed of by DES):

$$ 7 \text{ bits/character} \times 8 \text{ characters} = 56 \text{ bits} $$

If we were to perform an exhaustive search, the average amount of keys to be checked is $$ 2^{56} \div 2 = 2^{55} $$.

$$ 3.46 \times 10^{14} \text{ seconds} = \frac{2^{55} \text{ keys}}{104 \text{ keys/second}} $$

This comes out to 4,009,615,052.86 days, or 10,977,727.73 years.

2\. If the 8 characters are 7-bit ASCII characters, then the key length will be 6 bits per character (since DES will dispose of the least significant bit, and the most significant is always 0):

$$ 6 \text{ bits/character} \times 8 \text{ characters} = 48 \text{ bits} $$

If we were to perform an exhaustive search, the average amount of keys to be checked is $$ 2^{48} \div 2 = 2^{47} $$.

$$ 1.35 \times 10^{12} \text{ seconds} = \frac{2^{47} \text{ keys}}{104 \text{ keys/second}} $$

This comes out to 15,662,558.8 days, or 42,881.75 years.

3\. If the key-generation phrase is restricted to upper-case ASCII letters, then size of the keyspace is:

$$ 26 \text{ possible letters/character} \times 8 \text{ characters} = 208 \text{ keys} $$

$$ \log_2 208 \text{ keys} \approx 7.7 \text{ bits} $$

If we were to perform an exhaustive search, the average amount of keys to be checked is $$ 208 \div 2 = 104 $$.

$$ 1 \text{ second} = \frac{104 \text{ keys}}{104 \text{ keys/second}} $$

The entire keyspace can be search in 1 second because the keyspace was so thoroughly reduced by the flawed generation procedure.

{% include _understanding-crypto/previous-and-next-exercise.html %}
