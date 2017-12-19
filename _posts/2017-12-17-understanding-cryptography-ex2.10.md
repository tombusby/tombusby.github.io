---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 1 Solutions - Ex2.10"
description: "Performing a Chosen Plaintext Attack to Break an LFSR and Reveal its Feedback Coefficients"
layout: post
headerImage: false
projects: false
date: 2017-12-19 01:14
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 2
exercise: 10
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 2.10

We conduct a known-plaintext attack on an LFSR-based stream cipher. We
know that the plaintext sent was:

<pre class="pre-wrap-enabled">
1001 0010 0110 1101 1001 0010 0110
</pre>

By tapping the channel we observe the following stream:

<pre class="pre-wrap-enabled">
1011 1100 0011 0001 0010 1011 0001
</pre>

1. What is the degree $$m$$ of the key stream generator?
2. What is the initialization vector?
3. Determine the feedback coefficients of the LFSR.

### Solution

*This solution is verified as correct by the fact that it produces sensible output which can be fed back in for confirmation.*

1\. By XORing the ciphertext stream with the keystream, we get the following:

<pre class="pre-wrap-enabled">
0010 1110 0101 1100 1011 1001 0111
</pre>

If we arrange this into 7 bit blocks then we can easily see that the key repeats every 7 bits. The period of the LFSR is therefore 7.

<pre class="pre-wrap-enabled">
0010111 0010111 0010111 0010111
</pre>

Since the period of an LFSR (with primitive polynomial) is defined by $$ p = 2^m - 1 $$, we can rearrange this to $$m = \log_2 (p + 1)$$:

$$ \log_2 (7 + 1) = 3 $$

Therefore the degree of the LFSR must be 3.

2\. The first $$m$$ bits of the keystream are $$(s_0 = 0, s_1 = 0, s_2 = 1)$$, so therefore the initialisation vector must be:

$$ (s_2 = 1, s_1 = 0, s_0 = 0) $$

3\. Using the following 3 bits of the keystream (since we require $$2m$$ key bits for an attack), we can construct a set of simultaneous equations where the unknowns are the feedback coefficients that produced those next three values:

$$ s_2p_2 + s_1p_1 + s_0p_0 = s_3 $$

$$ s_3p_2 + s_2p_1 + s_1p_0 = s_4 $$

$$ s_4p_2 + s_3p_1 + s_2p_0 = s_5 $$

Using our specific values, we get the following:

$$ 1p_2 + 0p_1 + 0p_0 = 0\,(s_3) $$

$$ 0p_2 + 1p_1 + 0p_0 = 1\,(s_4) $$

$$ 1p_2 + 0p_1 + 1p_0 = 1\,(s_5) $$

This can be converted into an augmented matrix and solved via Gauss-Jordan Elimination:

<div style="text-align: center;">
<script type="math/tex">
\left[
\begin{array}{ccc|c}
  1 & 0 & 0 & 0 \\
  0 & 1 & 0 & 1 \\
  1 & 0 & 1 & 1
\end{array}
\right]
</script>
</div>

$$ R_3 \rightarrow R_3 + R_1 $$

<div style="text-align: center;">
<script type="math/tex">
\left[
\begin{array}{ccc|c}
  1 & 0 & 0 & 0 \\
  0 & 1 & 0 & 1 \\
  0 & 0 & 1 & 1
\end{array}
\right]
</script>
</div>

The matrix is now in reduced row echelon form, which allows the values of the unknowns to be read directly from the matrix. The feedback coefficients of the LFSR are $$(p_0 = 1, p_1 = 1, p_2 = 0)$$, which is represented graphically as follows:

![LFSR]({{ site.url }}/assets/images/understanding-crypto/ex2-10-lfsr.png){: .image-center}

To double check that our solution is correct, we can run the reconstructed LFSR outselves to be sure that it produces the same keystream:

<div style="text-align: center; margin-bottom: 20px">
<script type="math/tex">
\begin{array}{c|c c c|c}
& s_2 & s_1 & s_0 & \text{Output} \\ \hline
s_0 &  1 & 0 & 0 & 0 \\
s_1 &  0 & 1 & 0 & 0 \\
s_2 &  1 & 0 & 1 & 1 \\
s_3 &  1 & 1 & 0 & 0 \\
s_4 &  1 & 1 & 1 & 1 \\
s_5 &  0 & 1 & 1 & 1 \\
s_6 &  0 & 0 & 1 & 1 \\ \hline
& 1 & 0 & 0 & 0
\end{array}
</script>
</div>

Given that this LFSR outputs the same keystream, we have proved the correctness of our derived coefficients.

{% include _understanding-crypto/previous-and-next-exercise.html %}
