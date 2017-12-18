---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 1 Solutions - Ex1.14"
description: "Proving that Double Encryption with the Affine Cipher is Equivalent to Single Encryption"
layout: post
headerImage: false
projects: false
date: 2017-12-17 16:29
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 1
exercise: 14
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 1.14

An obvious approach to increase the security of a symmetric algorithm is to
apply the same cipher twice, i.e.:

$$ y = e_{k2}(e_{k1}(x)) $$

As is often the case in cryptography, things are very tricky and results are often different
from the expected and/ or desired ones. In this problem we show that a double
encryption with the affine cipher is only as secure as single encryption! Assume two
affine ciphers $$ e_{k1} = a_1x+b_1 $$ and $$ e_{k2} = a_2x+b_2 $$.

1. Show that there is a single affine cipher $$ e_{k3} = a_3x + b_3 $$ which performs exactly the same encryption (and decryption) as the combination $$ e_{k2}(e_{k1}(x)) $$.
2. Find the values for $$ a_3, b_3 $$ when $$ a_1 = 3, b_1 = 5 $$ and $$ a_2 = 11, b_2 = 7 $$.
3. For verification: (1) encrypt the letter K first with $$ e_{k1} $$ and the result with $$ e_{k2} $$, and (2) encrypt the letter K with $$ e_{k3} $$.
4. Briefly describe what happens if an exhaustive key-search attack is applied to a double-encrypted affine ciphertext. Is the effective key space increased? Remark: The issue of multiple encryption is of great practical importance in the case of the Data Encryption Standard (DES), for which multiple encryption (in particular, triple encryption) does increase security considerably.

### Solution

*This solution is verified as correct by the fact that it produces sensible output which can be fed back in for confirmation.*

1\. Where $$k_1 = (a_1, b_1)$$ and $$k_2 = (a_2, b_2)$$:

$$ e_{k1}(x) = y \equiv a_1 ⋅ x + b_1\,\mathrm{mod}\,26 $$

$$ e_{k2}(e_{k1}(x)) = y \equiv a_2(a_1x + b_1) + b_2\,\mathrm{mod}\,26 \\ \equiv a_1a_2x + a_2b_1 + b_2\,\mathrm{mod}\,26 $$

As such if you double encrypt with keys $${(a_1, b_1), (a_2, b_2)}$$, this is equivalent to a single encryption with key:

$$ k_3 \equiv (a_1a_2, a_2b_1 + b_2) \,\mathrm{mod}\,26 $$

2\. For $$ k_1 = (3, 5) $$ and $$ k_2 = (11, 7) $$ and $$ e_{k3}(x) = e_{k2}(e_{k1}(x)) $$:

$$ k_3 \equiv (3 \times 11\,\mathrm{mod}\,26, 11 \times 5 + 7\,\mathrm{mod}\,26) \equiv (7, 10) $$

3\. This can be confirmed by using the Python code I wrote for [Ex 1.11](/understanding-cryptography-ex1.11) to make sure that the output is the same for both the double encryption, and the equivalent single encryption.

4\. The keyspace does not increase by double encryption.

Since all $$e_{k2}(e_{k1}(x))$$ can be reduced to an equivalent $$e_k(x)$$, then searching all $$(k_1 \in \mathcal{K}, k_2 \in \mathcal{K})$$ is equivalent to searching all $$k \in \mathcal{K}$$.

{% include _understanding-crypto/previous-and-next-exercise.html %}
