---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 5 Solutions - Ex5.10"
description: "Propagation of Errors in Various Block Cipher Modes of Operation"
layout: post
redirect_from: /understanding-cryptography-ex5.10/
headerImage: false
projects: false
date: 2018-05-13 17:20
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 5
exercise: 10
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 5.10

Sometimes error propagation is an issue when choosing a mode of operation in practice. In order to analyze the propagation of errors, let us assume a bit error (i.e., a substitution of a “0” bit by a “1” bit or vice versa) in a ciphertext block $$ y_i $$.

1. Assume an error occurs during the transmission in one block of ciphertext, let’s say yi. Which cleartext blocks are affected on Bob’s side when using the ECB mode?
2. Again, assume block $$ y_i $$ contains an error introduced during transmission. Which cleartext blocks are affected on Bob’s side when using the CBC mode?
3. Suppose there is an error in the cleartext $$ x_i $$ on Alice’s side. Which cleartext blocks are affected on Bob’s side when using the CBC mode?
4. Assume a single bit error occurs in the transmission of a ciphertext character in 8-bit CFB mode. How far does the error propagate? Describe exactly how each block is affected.
5. Prepare an overview of the effect of bit errors in a ciphertext block for the modes ECB, CBC, CFB, OFB and CTR. Differentiate between random bit errors and specific bit errors when decrypting $$ y_i $$.

### Solution

*I haven't yet verified this solution independently. If you spot any mistakes, please leave a comment in the Disqus box at the bottom of the page.*

1\. In ECB mode, the only block affected will be the one containing the error. ECB mode essentially functions as a substitution cipher operating on a n-bit alphabet (where n is the block width of the cryptographic primative). Blocks have no interaction with, or dependence on, each other.

2\. In CBC mode, this error will propagate also to the next block (block $$ y_{i+1} $$), since the bit flip will be present when Bob XORs prior to decrypting the next block. Block $$ y_{i+2} $$ will not have any decryption errors, since block $$ y_{i+1} $$'s ciphertext is uncorrupted.

3\. In CBC mode, a bit-flip error introduced prior to encryption will only affect the block containing the error. This is because, in this case, the encryption and transmission have actually functioned without error. It's just that the wrong data was encrypted. As such, the exact same (correct in terms of reproducing the plaintext input provided, but) semantically incorrect plaintext will be produced by Bob as was encrypted by Alice. It's worth noting however that the error block and all the subsequent blocks will have different ciphertexts due to the propagation of the change, but this isn't an error. These blocks will decrypt correctly.

4\. In CFB mode, transmission errors will affect the block they occurred in and the following block by corrupting the keystream upon decryption (this happens due to the ciphertext being used as input to the cryptographic primitive). Note that CFB functions somewhat like a stream cipher, so in the "block" (of keystream bits produced by the primitive) that the error occurs, only that bit is altered. However, the following "block" of keybits will be totally corrupted due to the feedback of corrupted ciphertext into the cryptographic primitive upon decryption. As with CBC mode, $$ y_{i+2} $$ will not have any decryption errors, since block $$ y_{i+1} $$'s ciphertext is uncorrupted.

5\. To summarise the propagation of errors in the various modes:

+ *ECB*: As discussed in part 1, transmission and input errors are contained to the block that they occurred in. The entire block will be corrupted in the case of an error.
+ *CBC*: Transmission errors will affect the block they occurred in, and the following block. Both blocks will be entirely corrupted. Errors made prior to input will only affect the block they occurred in (but will mean that it and all subsequent blocks get different, but valid, ciphertexts).
+ *CFB*: Transmission errors will affect the block they occurred in (but only that bit), and the following block (which is corrupted entirely). Errors made prior to encryption will only affect the block they occurred in (but will mean that it and the subsequent "blocks" get different, but valid, ciphertexts).
+ *OFB*: This mode functions as a stream cipher. Feedback occurs via the keystream bits without interaction with the plaintext or ciphertext bits. As such any bit flip error, either prior to encryption in plaintext or during transmission, is limited to the bit where the error occurred.
+ *CTR*: The Counter mode is a stream cipher without any feedback, so any errors (either in plaintext or in transmission) are limited to the specific bit that was flipped.

{% include _understanding-crypto/previous-and-next-exercise.html %}
