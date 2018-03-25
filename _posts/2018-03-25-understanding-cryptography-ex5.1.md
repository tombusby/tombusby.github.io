---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 4 Solutions - Ex5.1"
description: "Selecting the Appropriate Mode of Operation for a Block Cipher in a Specific Use-Case"
layout: post
headerImage: false
projects: false
date: 2018-03-25 20:40
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 5
exercise: 1
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 5.1

Consider the storage of data in encrypted form in a large database using AES. One record has a size of 16 bytes. Assume that the records are not related to one another. Which mode would be best suited and why?

### Solution

*This solution is verified as correct by the official [Solutions for Odd-Numbered Questions](http://wiki.crypto.rub.de/Buch/en/download/Understanding_Cryptography_Odd_Solutions.pdf) manual.*

Cipher block chaining (CBC) mode is inappropriate since we want to be able to access individual records at random. For this very simple use case, the Electronic Code Book (ECB) mode is probably the most appropriate, although we will be able to see if any two records are the same. This could potentially be solved by using some kind of salt if this is a problem.

{% include _understanding-crypto/previous-and-next-exercise.html %}
