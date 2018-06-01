---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 1 Solutions - Ex1.13"
description: "Breaking the Affine Cipher with a Chosen Plaintext Attack"
layout: post
redirect_from: /understanding-cryptography-ex1.13/
headerImage: false
projects: false
date: 2017-12-17 15:39
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 1
exercise: 13
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 1.13

In an attack scenario, we assume that the attacker Oscar manages somehow to provide Alice with a few pieces of plaintext that she encrypts. Show how Oscar can break the affine cipher by using two pairs of plaintext–ciphertext, $$(x_1, y_1)$$ and $$(x_2, y_2)$$. What is the condition for choosing $$x_1$$ and $$x_2$$?

Remark: In practice, such an assumption turns out to be valid for certain settings, e.g., encryption by Web servers, etc. This attack scenario is, thus, very important and is denoted as a chosen plaintext attack.

### Solution

*This solution is verified as correct by the official [Solutions for Odd-Numbered Questions](http://wiki.crypto.rub.de/Buch/en/download/Understanding_Cryptography_Odd_Solutions.pdf) manual.*

In order to use the chosen plaintext attack, the plaintexts need to be chosen such that $$ \mathrm{gcd}(x_2 - x_1, m) = 1 $$, where $$m$$ is the size of the alphabet being encrypted. Another way to put this is that $$ x_2 - x_1 $$ must have multiplicative inverse in $$m$$.

The following equations can be derived by trying to solve the two chosen plaintext encryptions as a pair of simultaneous equations:

$$ a \equiv (x_1 −x_2)^{-1}(y_1 − y_2)\,\mathrm{mod}\,m $$

$$ b \equiv y_1 − ax_1\,\mathrm{mod}\,m $$

{% include _understanding-crypto/previous-and-next-exercise.html %}
