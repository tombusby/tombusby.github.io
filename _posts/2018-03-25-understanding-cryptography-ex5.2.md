---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 4 Solutions - Ex5.2"
description: "Comparison of Brute-force Costs for Ciphers in ECB or CBC Modes"
layout: post
headerImage: false
projects: false
date: 2018-03-25 20:55
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 5
exercise: 2
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 5.2

We consider known-plaintext attacks on block ciphers by means of an exhaustive key search where the key is $$ k $$ bits long. The block length counts $$ n $$ bits with $$ n > k $$.

1. How many plaintexts and ciphertexts are needed to successfully break a block cipher running in ECB mode? How many steps are done in the worst case?
2. Assume that the initialization vector IV for running the considered block cipher in CBC mode is known. How many plaintexts and ciphertexts are now needed to break the cipher by performing an exhaustive key search? How many steps need now maximally be done? Briefly describe the attack.
3. How many plaintexts and ciphertexts are necessary, if you do _not_ know the IV?
4. Is breaking a block cipher in CBC mode by means of an exhaustive key search considerably more difficult than breaking an ECB mode block cipher?

### Solution

*I haven't yet verified this solution independently. If you spot any mistakes, please leave a comment in the Disqus box at the bottom of the page.*

1\. It depends on the level of confidence you're happy with. Since the key length is smaller in bits than the block length, it's possible (by the [pigeonhole principle](https://en.wikipedia.org/wiki/Pigeonhole_principle)) that every keys maps to a unqique ciphertext. However, it is not certain. For a given plaintext, there may be multiple keys that map to the same ciphertext.

Depending on the actual values of $$k$$ and $$n$$, having a second (plaintext, ciphertext) pair can provide a high degree of certainty. This is due to the fact that it's unlikely that the false positive key would also map to the same ciphertext for another plaintext.

To calculate the probability of finding a false-positive key (where $$t$$ is the number of chosen plaintexts you have) you can use this formula:

$$ 2^{k-tn} $$

In the worst case (that the last-checked of all possible keys is the right one), you will have to perform $$ 2^k $$ checks.

2\. If you know the IV, then it is basically no different from breaking an ECB mode cipher. You brute force until you find a key that matches your first chosen plaintext, then you check the next, and so on, until you find one that matches all your chosen plaintexts. The only difference between this and the ECB mode search is the XOR that must be performed with the $$ i - 1 $$ ciphertext (or IV) before each check. This is a negligible increase in cost.

3\. If you do not know the IV, then you need more than one plaintext to have a chance of finding the correct key. Not knowing the IV means that we don't know what was XORed with the plaintext before encryption. If we only have one plaintext/ciphertext pair then we can pick a key at random, decrypt from the ciphertext, and then derive an IV which would make it correct. This is somewhat akin to solving a one-time-pad.

If we have two chosen plaintext/ciphertext pairs, then we *do* have the "IV" for the second block: it's the ciphertext of the first block. We can search this in the same way as part 1 and 2. If we find a match, then we can decrypt the first block with that key and derive the IV. However, solving the first block doesn't give us any greater confidence for the reasons listed earlier (we can always derive a valid IV that decrypts to the chosen plaintext). If we have a third pair, we can check that to gain a level of confidence that otherwise we would have had with two chosen plaintext/ciphertext pairs.

4\. In conclusion, breaking a cipher in CBC mode (where IV is unknown) is not significantly different in terms of cost, but your level of confidence will be equivalent to $$ t - 1 $$ rather than $$t$$. Where the IV *is* known, there is no siginificant difference between breaking an ECB mode cipher and a CBC mode cipher.

{% include _understanding-crypto/previous-and-next-exercise.html %}
