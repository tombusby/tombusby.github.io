---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 4 Solutions - Ex4.2"
description: "Computing Addition and Multiplication Tables in Galois Prime Fields"
layout: post
headerImage: false
projects: false
date: 2017-12-23 21:25
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 4
exercise: 2
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 4.2
For the AES algorithm, some computations are done by Galois Fields (GF). With the following problems, we practice some basic computations.

Compute the multiplication and addition table for the prime field $$GF(7)$$. A multiplication table is a square (here: 7×7) table which has as its rows and columns all field elements. Its entries are the products of the field element at the corresponding row and column. Note that the table is symmetric along the diagonal. The addition table is completely analogous but contains the sums of field elements as entries.

### Solution

*This solution is verified as correct by the short script I wrote to check it (see below).*

2\. Addition and Multiplication Tables for $$GF(7)$$:

<div style="text-align: center;">
<script type="math/tex">
\begin{array}{c|c c c c c}
+ & \text{0} & \text{1} & \text{2} & \text{3} & \text{4} & \text{5} & \text{6} \\ \hline
\text{0} & 0 & 1 & 2 & 3 & 4 & 5 & 6 \\
\text{1} & 1 & 2 & 3 & 4 & 5 & 6 & 0 \\
\text{2} & 2 & 3 & 4 & 5 & 6 & 0 & 1 \\
\text{3} & 3 & 4 & 5 & 6 & 0 & 1 & 2 \\
\text{4} & 4 & 5 & 6 & 0 & 1 & 2 & 3 \\
\text{5} & 5 & 6 & 0 & 1 & 2 & 3 & 4 \\
\text{6} & 6 & 0 & 1 & 2 & 3 & 4 & 5
\end{array}
</script>
&#32;
<script type="math/tex">
\begin{array}{c|c c c c c}
\times & \text{0} & \text{1} & \text{2} & \text{3} & \text{4} & \text{5} & \text{6} \\ \hline
\text{0} & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
\text{1} & 0 & 1 & 2 & 3 & 4 & 5 & 6 \\
\text{2} & 0 & 2 & 4 & 6 & 1 & 3 & 5 \\
\text{3} & 0 & 3 & 6 & 2 & 5 & 1 & 4 \\
\text{4} & 0 & 4 & 1 & 5 & 2 & 6 & 3 \\
\text{5} & 0 & 5 & 3 & 1 & 6 & 4 & 2 \\
\text{6} & 0 & 6 & 5 & 4 & 3 & 2 & 1
\end{array}
</script>
</div>

I wrote a [python script](https://github.com/tombusby/understanding-cryptography-exercises/blob/master/Chapter-04/ex4.2.py) which can calculate addition and multiplication tables for $$GF(p^1)$$ fields.

{% highlight python %}
from pprint import pprint

def generate_addition_table(n):
    row = [0] * 7
    table = [row[:] for i in range(0, n)]
    for a, b, c in [(a, b, (a+b) % 7) for a in range(0,n) for b in range(0,n)]:
        table[a][b] = c
    return table

def generate_multiplication_table(n):
    row = [0] * 7
    table = [row[:] for i in range(0, n)]
    for a, b, c in [(a, b, (a*b) % 7) for a in range(0,n) for b in range(0,n)]:
        table[a][b] = c
    return table

if __name__ == "__main__":
    n = 7
    print "Addition Table GF({}):".format(n)
    pprint(generate_addition_table(7))
    print "Multiplication Table GF({}):".format(n)
    pprint(generate_multiplication_table(7))
{% endhighlight %}

{% include _understanding-crypto/previous-and-next-exercise.html %}
