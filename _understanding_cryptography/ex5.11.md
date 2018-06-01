---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 5 Solutions - Ex5.11"
description: "Reestablishing Synchronisation and Correctness in the Case of Bit Insertion/Deletion during Transmission"
layout: post
redirect_from: /understanding-cryptography-ex5.11/
headerImage: false
projects: false
date: 2018-05-13 20:35
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 5
exercise: 11
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 5.11

Besides simple bit errors, the deletion or insertion of a bit yields even more severe effects since the synchronisation of blocks is disrupted. In most cases, the decryption of subsequent blocks will be incorrect. A special case is the CFB mode with a feedback width of 1 bit. Show that the synchronisation is automatically restored after $$ \mathcal{K} + 1 $$ steps, where $$ \mathcal{K} $$ is the block size of the block cipher.

### Solution

*This solution is verified as correct by the official [Solutions for Odd-Numbered Questions](http://wiki.crypto.rub.de/Buch/en/download/Understanding_Cryptography_Odd_Solutions.pdf) manual.*

In order to have enough bits to input into the cryptographic primitive, starting with the IV, after each round of 1-bit feedback, we (left or right, doesn't matter) shift the feedback register and store the new bit in the freed position.

In order for this answer to function, it's important that only 1 bit of plaintext be encrypted at a time per iteration of the cryptographic primitive. i.e. I do not mean that 128 bits of plaintext are encrypted, and then one of those bits shifted into the register per iteration. In that case, synchronisation errors will still occur.

As such, it will take $$ \mathcal{K} $$ steps for a feedback bit to enter the register, make its way to the other end. Upon the following step, it naturally falls off out of the register.

If a bit has been deleted then, for example, a sequence of bits which looked like [...01011...] may look like [...0111...] where the middle 0 has been deleted in transmission. This will step its way across the register until it falls off the end.

This significant difference in input to the primitive will cause significant differences in the keystream produced, corrupting all decrypted ciphertext from those steps. Once the register has been shifted enough times to contain [11...] \(and the 010 or 01 has been shifted off the end) then the value in the shift register is now equivalent to what it would be absent the deletion. Due to the fact that both ciphertext and plaintext had a bit removed, we are now still in sync too. This logic also can be employed with equal success to the effect of a bit insertion during transmission.

This means that for a given removed bit, after $$ \mathcal{K} + 1 $$ steps, the erroneous sequence (more specifically the first incorrect bit, the one in place of the deletion) will have fallen off the end of the register.

{% include _understanding-crypto/previous-and-next-exercise.html %}
