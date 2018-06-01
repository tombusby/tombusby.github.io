---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 2 Solutions - Ex2.6"
description: "Chosen Plaintext Attacks Against Stream Ciphers with Short Periods"
layout: post
headerImage: false
projects: false
date: 2017-12-18 20:56
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 2
exercise: 6
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 2.6

Assume we have a stream cipher whose period is quite short. We happen to know that the period is 150–200 bit in length. We assume that we do not know anything else about the internals of the stream cipher. In particular, we should not assume that it is a simple LFSR. For simplicity, assume that English text in ASCII format is being encrypted.

Describe in detail how such a cipher can be attacked. Specify exactly what Oscar has to know in terms of plaintext/ciphertext, and how he can decrypt all ciphertext.

### Solution

*I haven't yet verified this solution independently. If you spot any mistakes, please leave a comment in the Disqus box at the bottom of the page.*

For a stream cipher with a 150-200 bit period, and without knowing an attack against the underlying PRNG, we can break the cipher via a 150-200 bit chosen plaintext attack. We simply XOR the plaintext with the ciphertext bits to recover the key until the key starts to repeat somewhere between bit 150 and 200. After that we check to see if the key can decrypt the rest of the 200 bits of chosen-plaintext ciphertext bits and the remaining unknown ciphertext bits.

You might ask how we'd know if the unknown ciphertext was decrypted correctly. As per the question, we know that the message should decrypt to intelligible English-language text in ASCII-encoding. The chances of accidentally finding another keystream that decrypts the message to intelligible English text is so astronomical that it can be discounted.

If this doesn't work then what we found wasn't truly the beginning of key bit repetition. We simply found, by coincidence, a key-bit sequence that matched the first $$n$$ bits of the key. We repeat the process until the true point at which the keystream loops is discovered.

{% include _understanding-crypto/previous-and-next-exercise.html %}
