---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 5 Solutions - Ex5.4"
description: "Why Keeping the IV Secret Does not Increase Security for Ciphers in OFB Mode"
layout: post
redirect_from: /understanding-cryptography-ex5.4/
headerImage: false
projects: false
date: 2018-05-11 16:16
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 5
exercise: 4
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 5.4

Keeping the IV secret in OFB mode does not make an exhaustive key search more complex. Describe how we can perform a brute-force attack with unknown IV. What are the requirements regarding plaintext and ciphertext?

### Solution

*I haven't yet verified this solution independently. If you spot any mistakes, please leave a comment in the Disqus box at the bottom of the page.*

For our chosen plaintext/ciphertext pairs, we can derive the keystream they were encrypted with via XOR. This means that we know what input was given to the cryptographic primitive for all blocks except the first block (since the keystream becomes the IV of the next block). This means that we can try to brute force a key that produces the keystream from the known inputs. This is completely equivalent to brute forcing a cipher in CBC mode. The IV can be derived by decrypting the first block's keystream when you've reached a high degree of confidence that your brute forced key isn't a false positive.

The reasoning behind this is the same as for [Ex 5.2 - parts 3 and 4](/understanding-cryptography-ex5.2/).

As with the CBC example, the complexity is exactly the same as if the IV is known, but the level of confidence is as if you have one less ciphertext/plaintext pair than you do.

{% include _understanding-crypto/previous-and-next-exercise.html %}
