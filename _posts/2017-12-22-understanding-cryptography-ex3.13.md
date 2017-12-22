---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 3 Solutions - Ex3.13"
description: "Calculating the State After Round 1 for the PRESENT-80 Block Cipher"
layout: post
headerImage: false
projects: false
date: 2017-12-22 01:11
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 3
exercise: 13
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 3.13
This problem deals with the lightweight cipher PRESENT.
1. Calculate the state of PRESENT-80 after the execution of one round. You can use the following table to solve this problem with paper and pencil. Use the following values (in hexadecimal notation):
    * plaintext = 0000 0000 0000 0000,
    * key = BBBB 5555 5555 EEEE FFFF.
2. Now calculate the round key for the second round using the following table.

### Solution

*This solution is verified as correct by the official [Solutions for Odd-Numbered Questions](http://wiki.crypto.rub.de/Buch/en/download/Understanding_Cryptography_Odd_Solutions.pdf) manual.*

1\. After round one, the state is as follows:

<div style="text-align: center;">
<script type="math/tex">
\begin{array}{c|l}
\text{Plaintext} & \mathtt{0000\,0000\,0000\,0000} \\ \hline
\text{Round key} & \mathtt{BBBB\,5555\,5555\,EEEE}  \\
\text{State after KeyAdd} & \mathtt{BBBB\,5555\,5555\,EEEE} \\
\text{State after S-Layer} & \mathtt{8888\,0000\,0000\,1111} \\
\text{State after P-Layer} & \mathtt{F000\,0000\,0000\,000F}
\end{array}
</script>
</div>

2\. The generation of the round key for the second round is as follows (Round 1 does not modify the key register):

<div style="text-align: center;">
<script type="math/tex">
\begin{array}{c|l}
\text{Key} & \mathtt{BBBB\,5555\,5555\,EEEE\,FFFF} \\ \hline
\text{Key state after rotation} & \mathtt{DFFF\,F777\,6AAA\,AAAA\,BDDD}  \\
\text{Key state after S-box} & \mathtt{7FFF\,F777\,6AAA\,AAAA\,BDDD} \\
\text{Key state after CounterAdd} & \mathtt{7FFF\,F777\,6AAA\,AAAA\,3DDD} \\
\text{Round key for Round 2} & \mathtt{7FFF\,F777\,6AAA\,AAAA}
\end{array}
</script>
</div>

{% include _understanding-crypto/previous-and-next-exercise.html %}
