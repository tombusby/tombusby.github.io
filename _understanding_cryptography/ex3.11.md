---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 3 Solutions - Ex3.11"
description: "Using COPACOBANA to Brute-Force DES Keys"
layout: post
headerImage: false
projects: false
date: 2017-12-21 23:33
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 3
exercise: 11
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 3.11

As the example of [COPACOBANA](http://www.copacobana.org/) shows, key-search machines need not be prohibitive from a monetary point of view. We now consider a simple bruteforce attack on DES which runs on COPACOBANA.

1. Compute the runtime of an average exhaustive key-search on DES assuming the following implementational details:
  * COPACOBANA platform with 20 FPGA modules
  * 6 FPGAs per FPGA module
  * 4 DES engines per FPGA
  * Each DES engine is fully pipelined and is capable of performing one encryption per clock cycle
  * 100 MHz clock frequency
2. How many COPACOBANA machines do we need in the case of an average search time of one hour?
3. Why does any design of a key-search machine constitute only an upper security threshold? By upper security threshold we mean a (complexity) measure which describes the maximum security that is provided by a given cryptographic algorithm.

### Solution

*This solution is verified as correct (mostly, see note under 2.) by the official [Solutions for Odd-Numbered Questions](http://wiki.crypto.rub.de/Buch/en/download/Understanding_Cryptography_Odd_Solutions.pdf) manual.*

1\. Firstly we'll calculate the number of DES encryptions for one machine per clock cycle:

$$ 20 \times 6 \times 4 = 480\text{ encryptions/cycle} $$

There are $$ 1 \times 10^8 $$ clock cycles per second as per the question definition. As such, the number of keys checked per second (equivalent to number of encryptions performed per second) is:

$$ 480 \times 1 \times 10^8 = 4.8 \times 10^{10} \text{ encryptions/second} $$

To calculate the run-time of an average cause exhaustive search ($$2^{55}$$ checks), we can use the following formula:

$$ 750,600\text{ seconds} \approx \frac{2^{55}\text{ encryptions}}{4.8 \times 10^{10} \text{ encryptions/second}} $$

This comes out to approximately 8.69 days.

2\. In order to calculate how many machines we need for a time of one hour (3600 seconds), we can construct the following equation:

$$ 3,600\text{ seconds} = \frac{2^{55}\text{ encryptions}}{n\text{ machines} \times 4.8 \times 10^{10} \text{ encryptions/second}} $$

We can rearrange this equation to calculate $$n$$:

$$ 208.5 \text{ machines} \approx \frac{2^{55}\text{ encryptions}}{3,600\text{ seconds} \times 4.8 \times 10^{10} \text{ encryptions/second}} $$

Obviously, you can't have 0.5 of a COPACABANA machine, so the answer must be rounded up to 209.

*Note*: The solution manual claims that the answer is 18 machines, but after checking and re-checking my answer, I've become convinced this is a mistake. By feeding 18 machines back into the equation that produced a correct answer for (1), it does not come out to anything approaching one hour. If you divide 750,600 by 18, it also doesn't produce the desired result of 1 hour.

3\. The machine performs a brute–force attack. However, there might be more powerful analytical attacks which explore weaknesses of the cipher. Hence, the key–search machine provides only a lower security threshold

{% include _understanding-crypto/previous-and-next-exercise.html %}
