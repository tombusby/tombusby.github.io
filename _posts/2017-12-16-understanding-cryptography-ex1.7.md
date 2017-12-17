---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 1 Solutions - Ex1.7"
description: "Computing Addition and Multiplication Tables in Finite Sets"
layout: post
headerImage: false
projects: false
date: 2017-12-16 22:46
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 1
exercise: 7
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 1.7

We consider the ring Z4. Construct a table which describes the addition of all
elements in the ring with each other:

1. Construct the multiplication table for $$\mathbb{Z}_4$$.
2. Construct the addition and multiplication tables for $$\mathbb{Z}_5$$.
3. Construct the addition and multiplication tables for $$\mathbb{Z}_6$$.
4. There are elements in Z4 and Z6 without a multiplicative inverse. Which elements are these? Why does a multiplicative inverse exist for all non-zero elements in $$\mathbb{Z}_5$$?

### Solution

*This solution is verified as correct by the official [Solutions for Odd-Numbered Questions](http://wiki.crypto.rub.de/Buch/en/download/Understanding_Cryptography_Odd_Solutions.pdf) manual.*

1\. Multiplication table for $$\mathbb{Z}_4$$:

<div style="text-align: center;">
<script type="math/tex">
\begin{array}{c|c c c c}
\times & \text{0} & \text{1} & \text{2} & \text{3} \\ \hline
\text{0} & 0 & 0 & 0 & 0 \\
\text{1} & 0 & 1 & 2 & 3 \\
\text{2} & 0 & 2 & 0 & 2 \\
\text{3} & 0 & 3 & 2 & 1 \\
\end{array}
</script>
</div>

2\. Addition and Multiplication Tables for $$\mathbb{Z}_5$$:

<div style="text-align: center;">
<script type="math/tex">
\begin{array}{c|c c c c c}
+ & \text{0} & \text{1} & \text{2} & \text{3} & \text{4} \\ \hline
\text{0} & 0 & 1 & 2 & 3 & 4 \\
\text{1} & 1 & 2 & 3 & 4 & 0 \\
\text{2} & 2 & 3 & 4 & 0 & 1 \\
\text{3} & 3 & 4 & 0 & 1 & 2 \\
\text{4} & 4 & 0 & 1 & 2 & 3 \\
\end{array}
</script>
&#32;
<script type="math/tex">
\begin{array}{c|c c c c c}
\times & \text{0} & \text{1} & \text{2} & \text{3} & \text{4} \\ \hline
\text{0} & 0 & 0 & 0 & 0 & 0 \\
\text{1} & 0 & 1 & 2 & 3 & 4 \\
\text{2} & 0 & 2 & 4 & 1 & 3 \\
\text{3} & 0 & 3 & 1 & 4 & 2 \\
\text{4} & 0 & 4 & 3 & 2 & 1 \\
\end{array}
</script>
</div>

3\. Addition and Multiplication Tables for $$\mathbb{Z}_6$$:

<div style="text-align: center;">
<script type="math/tex">
\begin{array}{c|c c c c c c}
+ & \text{0} & \text{1} & \text{2} & \text{3} & \text{4} & \text{5} \\ \hline
\text{0} & 0 & 1 & 2 & 3 & 4 & 5 \\
\text{1} & 1 & 2 & 3 & 4 & 5 & 0 \\
\text{2} & 2 & 3 & 4 & 5 & 0 & 1 \\
\text{3} & 3 & 4 & 5 & 0 & 1 & 2 \\
\text{4} & 4 & 5 & 0 & 1 & 2 & 3 \\
\text{4} & 5 & 0 & 1 & 2 & 3 & 4 \\
\end{array}
</script>
&#32;
<script type="math/tex">
\begin{array}{c|c c c c c c}
\times & \text{0} & \text{1} & \text{2} & \text{3} & \text{4} & \text{5} \\ \hline
\text{0} & 0 & 0 & 0 & 0 & 0 & 0 \\
\text{1} & 0 & 1 & 2 & 3 & 4 & 5 \\
\text{2} & 0 & 2 & 4 & 0 & 2 & 4 \\
\text{3} & 0 & 3 & 0 & 3 & 0 & 3 \\
\text{4} & 0 & 4 & 2 & 0 & 4 & 2 \\
\text{5} & 0 & 5 & 4 & 3 & 2 & 1 \\
\end{array}
</script>
</div>

4\. To determine whether an element has a multiplicative inverse, we must check if it can be multiplied with another element to produce 1:

Elements without a multiplicative inverse in $$\mathbb{Z}_4$$ are 2 and 0

Elements without a multiplicative inverse in $$\mathbb{Z}_6$$ are 2, 3, 4 and 0

For all non-zero elements of $$\mathbb{Z}_5$$, there exists a multiplicative inverse because 5 is a prime. Hence, all non-zero elements smaller than 5 are relatively prime to 5.

{% include _understanding-crypto/previous-and-next-exercise.html %}
