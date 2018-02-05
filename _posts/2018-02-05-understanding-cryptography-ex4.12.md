---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 4 Solutions - Ex4.12"
description: "Calculating the Number of XOR Gates Required for AES Diffusion"
layout: post
headerImage: false
projects: false
date: 2018-02-05 17:45
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 4
exercise: 12
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 4.12

We now look at the gate (or bit) complexity of the MixColumn function, using the results from problem 4.11. We recall from the discussion of stream ciphers that a 2-input XOR gate performs a $$GF(2)$$ addition.

1. How many 2-input XOR gates are required to perform one constant multiplication by 01, 02 and 03, respectively, in $$GF(2^8)$$.
2. What is the overall gate complexity of a hardware implementation of one matrix–vector multiplication?
3. What is the overall gate complexity of a hardware implementation of the entire Diffusion layer? We assume permutations require no gates.

### Solution

*I haven't yet verified this solution independently. If you spot any mistakes, please leave a comment in the Disqus box at the bottom of the page. I should also note that I'm not, personally, tremendously confident about the correctness of my answer.*

Firstly, the question is slightly vague. I'm going to assume maximum parallelisation, i.e. that no XOR gate is used for multiple purposes and that every cell in the matrix multiplication is calculated in parallel (the circuitry for 02, for example, is not reused to calculate each 02 mult in sequential fashion). The true minimum number of gates required to perform an AES diffusion is actually much lower than I give here, but if you care much more about speed than economising for size, this is the correct answer.

1\. No gates are required to perform a 01 multiplication, since 01 is the unit of multiplication. 02 requires four XOR operations. 03 requires 11 XOR operations.

2\. During the matrix multiplication, four 02 multiplications are performed. This requires $$4 \times 4 = 16$$ XOR gates. Four 03 multiplications are also performed. This requires $$4 \times 11 = 44$$ XOR gates. The 01 multiplications do not require any XOR gates.

So far we have a total of 60 XOR gates required to create the intermediate state of the matrix multiplication. What's left now is to XOR all of the entries in each row together. Each addition requires 8 XOR gates, there are 3 XORs required for each row, and there are 4 rows:

$$ 8\,\mathsf{bits} \times 3\,\mathsf{additions} \times 4\,\mathsf{rows} = 96 $$

So all that's left to two is to add together the number of gates required for the multiplications and the row additions:

$$ 60\,\mathsf{gates} + 96\,\mathsf{gates} = 156\,\mathsf{gates} $$

3\. The entire diffusion layer consists of ShiftRows (no XOR gates needed, since only permutation) and MixColumn. What we calculated above was the number of gates required to perform the MixColumns operation for a single column. We could reuse this circuitry to calculate each of the four columns of input in sequence, however, we're assuming maximum parallelisation so, as such, the number of gates required is:

$$ 156\,\mathsf{gates} \times 4\,\mathsf{columns} = 624 \,\mathsf{gates} $$

{% include _understanding-crypto/previous-and-next-exercise.html %}
