---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 3 Solutions - Ex3.2"
description: "Verifying the Non-Linearity of the DES S-boxes"
layout: post
headerImage: false
projects: false
date: 2017-12-19 20:47
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 3
exercise: 2
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 3.2

We want to verify that $$IP(·)$$ and $$IP^{−1}(·)$$ are truly inverse operations. We consider a vector $$x = (x_1,x_2,...,x_{64})$$ of 64 bit.

Show that $$ IP^{−1}(IP(x)) = x $$ for the first five bits of $$x$$, i.e. for $$x_i, i = 1,2,3,4,5$$.

### Solution

*I haven't yet verified this solution independently. If you spot any mistakes, please leave a comment in the Disqus box at the bottom of the page.*

The easiest way to demonstrate this is to create a table that shows which location each bit gets mapped to after each operation:

<div style="text-align: center; margin-bottom: 20px">
<script type="math/tex">
\begin{array}{c}
& x_1 & x_2 & x_3 & x_4 & x_5 & x_6 \\
IP & \downarrow & \downarrow & \downarrow & \downarrow & \downarrow & \downarrow \\
& x_{58} & x_{50} & x_{42} & x_{34} & x_{26} & x_{18} \\
IP^{-1} & \downarrow & \downarrow & \downarrow & \downarrow & \downarrow & \downarrow \\
& x_1 & x_2 & x_3 & x_4 & x_5 & x_6
\end{array}
</script>
</div>

{% include _understanding-crypto/previous-and-next-exercise.html %}
