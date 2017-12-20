---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 3 Solutions - Ex3.8"
description: "Proving that When the DES Key and Input are Bitwise Inverted, So is the Output"
layout: post
headerImage: false
projects: false
date: 2017-12-20 12:34
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 3
exercise: 8
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 3.8

DES has a somewhat surprising property related to bitwise complements of its inputs and outputs. We investigate the property in this problem. We denote the bitwise complement of a number $$A$$ (that is, all bits are flipped) by $$A^\prime$$. Let ⊕ denote bitwise XOR. We want to show that if

$$ y = DES_k(x) $$

then

$$ y^\prime = DES_{k^\prime}(x^\prime) $$

This states that if we complement the plaintext and the key, then the ciphertext output will also be the complement of the original ciphertext. Your task is to prove this property.

Try to prove this property using the following steps:

1. Show that for any bit strings A,B of equal length,

    $$ A^\prime \oplus B^\prime = A \oplus B $$

    and

    $$ A^\prime \oplus B = (A \oplus B)^\prime $$

    (These two operations are needed for some of the following steps.)

2. Show that $$ PC−1(k^\prime) = (PC−1(k))^\prime $$.
3. Show that $$ LS_i(C_{i-1}^\prime) = (LS_i(C_{i-1}))^\prime $$.
4. Using the two results from above, show that if $$k_i$$ are the keys generated from $$k$$, then $$k_i^\prime$$ are the keys generated from $$k^\prime$$, where $$i = 1,2,...,16 $$.
5. Show that $$ IP(x^\prime) = (IP(x))^\prime $$.
6. Show that $$ E(x^\prime) = (E(x))^\prime $$.
7. Using all previous results, show that if $$ R_{i−1}, L_{i−1}, k_i $$ generate $$R_i$$, then $$ R_{i−1}^\prime, L_{i−1}^\prime, k_i^\prime $$ generate $$ R_i^\prime $$.
8. Show that if $$ y = DES_k(x) $$, then $$ y^\prime = DES_{k^\prime}(x^\prime) $$ is true.

### Solution

*I haven't yet verified this solution independently. If you spot any mistakes, please leave a comment in the Disqus box at the bottom of the page.*

1\. Since this is a bitwise operation, there is no way for one bit to affect another in its sequence, we can prove this property by proving that it holds true for one bit. If the property always holds true for 1 bit, then it must hold true for all the other bits in the bitwise operation. The easiest way to do this is with a truth table:

<div style="text-align: center;">
<script type="math/tex">
\begin{array}{c|c}
A & B & A^\prime & B^\prime & A \oplus B & A^\prime \oplus B^\prime & A^\prime \oplus B & (A \oplus B)^\prime \\ \hline
0 & 0 & 1 & 1 & 0 & 0 & 1 & 1 \\
0 & 1 & 1 & 0 & 1 & 1 & 0 & 0 \\
1 & 0 & 0 & 1 & 1 & 1 & 0 & 0 \\
1 & 1 & 0 & 0 & 0 & 0 & 1 & 1
\end{array}
</script>
</div>

As you can see, these properties hold true for all possible values of $$A$$ and $$B$$.

2\. Since $$PC-1$$ is a simple, determininistinc, bitwise permutataion, where the value of the input bits don't affect where they're mapped to, this property is trivially, and by definition, true and does not need to be proved. Whether the bits are flipped before or after they are re-ordered makes no difference to the outcome, since each bit flip is an atomic operation unaffected by the ordering or values of other bits.

3\. The $$LS$$ subkey rotations are also simple, deterministic permutations. As such, by the same reasoning as above, this property is also trivially true by definition.

4\. Above, we showed that, at least up until $$PC-2$$ is applied, if you provide the bitwise complement of a key $$k$$, it will generate the bitwise complement of the subkeys $$k$$ would have produced. As with $$PC-1$$ and $$LS$$, we can show that $$PC-2$$ is another simple bit permutation. As such, the subkeys which are fed into $$f$$ for $$k^\prime$$ are guaranteed to be the bitwise complement of those which would have been fed into $$f$$ for $$k$$ in all cases.

5\. Again, $$IP$$ is a simple bit-order permutation, so $$IP(x^\prime) = (IP(x))^\prime $$ is trivially true.

6\. $$E$$ is slightly different, in that it is not a simple bit-order permutation. $$E$$ is a function that maps each bit in a vector to one or two bits in a larger vector. The bits which map to only one location in the expanded vector can be ignored, since computing their compliment behaves the same as a normal permutation. To prove that $$ E(x^\prime) = (E(x))^\prime $$, we need to consider whether it matters if the bits are flipped before or after being mapped to the two locations in the expanded vector. It's worth noting here that there are no combinatory operations performed, a single bit in the expanded vector always has one "parent" in the original vector. As such, it's fairly obvious that flipping one bit prior to mapping, or flipping the two copies after the mapping, makes no difference to the outcome.

7\. In order to prove this property we need to do a [proof by induction](https://en.wikipedia.org/wiki/Mathematical_induction). We first need to show that encrypting $$(L_0^\prime, R_0^\prime)$$ with $$k^\prime$$ will produce $$(L_1^\prime, R_1^\prime)$$ where $$(L_1, R_1)$$ is the result after one round of encrypting $$(L_0, R_0)$$ with $$k$$.

After we have proved this to be true, we need to prove that the same holds with $$(L_{i-1}^\prime, R_{i-1}^\prime)$$ producing $$(L_{i}^\prime, R_{i}^\prime)$$ from $$k_i^\prime$$. The first step proves the first "domino" to be true. The next step proves that all the other "dominoes" will fall after it (i.e. the equality holds through all rounds).

Firstly we need to look at the $$f$$ function. We have proved in previous steps that for all rounds, for $$k^\prime$$, the subkey $$k_i^\prime$$ being passed into $$f$$ will be the complement of subkey $$k_i$$ which would have been generated by $$k$$. We also know by definition that $$f$$ receives $$R_0^\prime$$ as input.

As such we can construct the following definition of what happens within the $$f$$ function (where $$S$$ is the application of the S-Boxes and $$P$$ is the final permutation):

$$ P(S(E(R_0^\prime) \oplus k_1^\prime)) $$

Using the equality we proved in (6), we can rearrange this to an equivalent definition:

$$ P(S((E(R_0))^\prime \oplus k_1^\prime)) $$

Now, using the equality we proved in (1) (which is that $$ A^\prime \oplus B^\prime = A \oplus B $$) we can simplify the definition:

$$ P(S(E(R_0) \oplus k_1)) $$

At this point, we've proved something quite surprising, which is that the input to the S-boxes is identical when encrypting $$R_0^\prime$$ with $$k_1^\prime$$ as it is when encrypting $$R_0$$ with $$k_1$$.

As such, we don't need to do any proofs relating to $$P$$ and $$S$$ since they will receive the same input in both cases. Since they receive the same input, they will produce the same output in both cases. As such we have shown that:

$$ f(R_0, k_1) = f(R_0^\prime, k_1^\prime) $$

The fact that the S-boxes receive the same input in both cases lies at the heart of why $$ y^\prime = DES_{k^\prime}(x^\prime) $$ holds. The S-Boxes are non-linear, so for any S-box $$S_i$$, $$S_i(x^\prime) \neq (S_i(x))^\prime$$. As such, if the S-Boxes didn't receive the same input in both cases, there'd be no way for the equality to hold, since $$f$$ would produce different, unrelated output.

The only other operation to perform in the round is to XOR the output of $$f$$ with $$L_0^\prime$$. To finish proving that $$(L_0^\prime, R_0^\prime)$$ will produce $$(L_1^\prime, R_1^\prime)$$ from $$k_1^\prime$$, we need to prove the following equality:

$$ L_0^\prime \oplus f(R_0^\prime, k_1^\prime) = (L_0 \oplus f(R_0, k_1))^\prime $$

Using the equality we just proved about $$f$$ we can simplify slightly:

$$ L_0^\prime \oplus f(R_0, k_1) = (L_0 \oplus f(R_0, k_1))^\prime $$

We can now finish the proof by invoking the equality we proved in (1) (which is that $$ A^\prime \oplus B = (A \oplus B)^\prime $$):

$$ L_0^\prime \oplus f(R_0, k_1) = L_0^\prime \oplus f(R_0, k_1) $$

This proves the equality by demonstrating that they are equalent statements.

Now that we've proved that inverting the key and input will produce bitwise inverted output after one round, we can extend this logic to say that for *any* round, if the subkey and input to that round are bitwise inverted, then it will produce the complement of what that round would have produced absent the inversion. I wont duplicate all of the work we did above, since all the equalities whold true for input/key index $$i$$ and $$i-1$$ as they do for index $$0$$ and $$1$$.

We know via the work we did above that Round 2 will receive the complement of the subkey and of the input it would have received, had the input and key not been bitwise inverted. As such, round 2 will produce the complement of the output that it would have. This means that Round 3 will receive the compliment of the subkey and input that it would have... etc. This property cascades through all the rounds, and describes a (slightly informal) proof by induction.

8\. All there is left to do to prove the equality is to show that $${IP}^{-1}(x^\prime) = ({IP}^{-1}(x))^\prime $$. Since this is the inverse of $$IP$$ and this property holds for all simple permutations, then it is trivially true.

We know that the input to $${IP}^{-1}$$ will be the complement of what it would have received had the key and input not been bitwise inverted before encryption, since we know that the final round will produce output with this property.

{% include _understanding-crypto/previous-and-next-exercise.html %}
