---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 1 Solutions - Ex1.12"
description: "Decrypting with the Affine Cipher"
layout: post
headerImage: false
projects: false
date: 2017-12-17 14:56
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 1
exercise: 12
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 1.12

Now, we want to extend the affine cipher from Sect. 1.4.4 such that we can encrypt and decrypt messages written with the full German alphabet. The German alphabet consists of the English one together with the three umlauts, Ä, Ö, Ü, and the (even stranger) “double s” character ß. We use the following mapping from letters to integers:

![German Alphabet Mapping]({{ site.url }}/assets/images/understanding-crypto/ex1-12-german-affine.png){: .image-center}

1. What are the encryption and decryption equations for the cipher?
2. How large is the key space of the affine cipher for this alphabet?
3. The following ciphertext was encrypted using the key (a = 17, b = 1). What is the corresponding plaintext?
4. From which village does the plaintext come?

### Solution

*This decryption part of the solutions (1. and 3.) are verified as correct by the fact that they produce sensible output. If you can see an error in my reasoning for 2. then please leave a comment in the Disqus box below.*

1\. The encryption and decryption functions are defined as follows:

$$ e_k(x) = y \equiv a ⋅ x + b\,\mathrm{mod}\,30 $$

$$ e_k(x) = y \equiv a^{-1} ⋅ (y - b)\,\mathrm{mod}\,30 $$

$$ \mathsf{where}\, k = (a, b)\,\mathsf{such\,that}\,\mathrm{gcd}(a, 30) = 1 $$

2\. The keyspace is defined as the cartesian product of the possible $$a$$ values and the possible $$b$$ values. $$a$$ needs to be co-prime with 30 in order for it to have a multiplicative inverse in $$\mathbb{Z}_{30}$$. As such the number of possible $$a$$ values is $$\phi(30) = 8$$. Since $$b$$ can be any value in $$\mathbb{Z}_{30}$$ (beacause 0 still performs encryption) then the number of possible b values is 30.

As such the number of total keys is $$ (8 \times 30) - 1 = 239$$. We subtract 1 from the total in order to eliminate $$(1, 0)$$ since that key doesn't perform encryption.

3\. When using the key $$ (17, 1) $$, this is the result of decryption:

$$ d_k(ä, u, ß, w, ß) = FRODO $$

4\. Frodo comes from The Shire.

{% include _understanding-crypto/previous-and-next-exercise.html %}
