---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 3 Solutions - Ex3.6"
description: "Demonstrating the Avalanche Effect of a 1-bit Key Change in DES"
layout: post
redirect_from: /understanding-cryptography-ex3.6/
headerImage: false
projects: false
date: 2017-12-20 00:45
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 3
exercise: 6
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 3.6

An avalanche effect is also desirable for the key: A one-bit change in a key should result in a dramatically different ciphertext if the plaintext is unchanged.
1. Assume an encryption with a given key. Now assume the key bit at position 1 (prior to PC − 1) is being flipped. Which S-boxes in which rounds are affected by the bit flip during DES encryption?
2. Which S-boxes in which DES rounds are affected by this bit flip during DES decryption?

### Solution

*I haven't yet verified this solution independently. If you spot any mistakes, please leave a comment in the Disqus box at the bottom of the page.*

1\. The keybit at position 1 is moved to position 8 by $$PC-1$$.

The two halves of the key which are independently rotated are referred to as $$C$$ and $$D$$.

Since $$D_0$$ is all 0s, then $$D_{1-16}$$ will also be all 0s.

$$ C_0 = 0100000_{16} $$ (due to the 1 in position 8).

In rounds 1, 2, 9, and 16, the key halves are rotated 1 position left. In all other rounds, they are rotated 2 positions left.

It's worth noting that, even after the $$PC-2$$ permutation, a keybit in the first half will remain in the first half after permutation. Second-half keybits will also remain in the second half after permutation.

This allows us to completely forget about $$D$$ when calculating which S-boxes are affected.

<div style="text-align: center;">
<script type="math/tex">
\begin{array}{c c}
\text{Round} & Position & & Position & & \text{S-Box} \\ \hline
1. & 7 & \xrightarrow{PC-2} & 20 & \xrightarrow{\,\,\,\oplus\,\,\,} &  4 \\
2. & 6 & \xrightarrow{PC-2} & 10 & \xrightarrow{\,\,\,\oplus\,\,\,} &  2 \\
3. & 4 & \xrightarrow{PC-2} & 16 & \xrightarrow{\,\,\,\oplus\,\,\,} &  3 \\
4. & 2 & \xrightarrow{PC-2} & 24 & \xrightarrow{\,\,\,\oplus\,\,\,} &  4 \\
5. & 28 & \xrightarrow{PC-2} & 8 & \xrightarrow{\,\,\,\oplus\,\,\,} &  2 \\
6. & 26 & \xrightarrow{PC-2} & 17 & \xrightarrow{\,\,\,\oplus\,\,\,} &  3 \\
7. & 24 & \xrightarrow{PC-2} & 4 & \xrightarrow{\,\,\,\oplus\,\,\,} &  1 \\
8. & 22 & \xrightarrow{PC-2} & N/A & \xrightarrow{\,\,\,\oplus\,\,\,} & N/A \\
9. & 21 & \xrightarrow{PC-2} & 11 & \xrightarrow{\,\,\,\oplus\,\,\,} &  2 \\
10. & 19 & \xrightarrow{PC-2} & 14 & \xrightarrow{\,\,\,\oplus\,\,\,} &  3 \\
11. & 17 & \xrightarrow{PC-2} & 2 & \xrightarrow{\,\,\,\oplus\,\,\,} &  1 \\
12. & 15 & \xrightarrow{PC-2} & 9 & \xrightarrow{\,\,\,\oplus\,\,\,} &  2 \\
13. & 13 & \xrightarrow{PC-2} & 23 & \xrightarrow{\,\,\,\oplus\,\,\,} &  4 \\
14. & 11 & \xrightarrow{PC-2} & 3 & \xrightarrow{\,\,\,\oplus\,\,\,} &  1 \\
15. & 9 & \xrightarrow{PC-2} & N/A & \xrightarrow{\,\,\,\oplus\,\,\,} & N/A \\
16. & 8 & \xrightarrow{PC-2} & 18 & \xrightarrow{\,\,\,\oplus\,\,\,} &  3
\end{array}
</script>
</div>

2\. It's not necessary to explicitly check what the effect of this bitflip is during decryption key-scheduling, since it must be the same in reverse. If it wasn't the S-boxes would receive different input during decryption than they do during encryption, and the cipher would not function.

I wrote a [python script](https://github.com/tombusby/understanding-cryptography-exercises/blob/master/Chapter-03/ex3.6.py) which can calculate which S-boxes are affected:

{% highlight python %}
one_bit = 7 # off by one so that bit 28 doesn't become bit 0 - is corrected via a +1 when used.

def find_one_bit_location_after_PC1(bit_position):
    map = dict(zip([
            14, 17, 11, 24, 1, 5, 3, 28,
            15, 6, 21, 10, 23, 19, 12, 4,
            26, 8, 16, 7, 27, 20, 13, 2
        ], range(1, 29)))
    return map[bit_position] if bit_position in map else None

for i in range(1,17):
    if i in [1, 2, 9, 16]:
        rotate = 1
    else:
        rotate = 2
    one_bit = (one_bit - rotate) % 28
    subkey_bit = find_one_bit_location_after_PC1(one_bit + 1)
    print "Round {}: 1-bit is occupying position {}\n" \
        "\tAfter PC-2 this is subkey bit {}\n\twhich afects S-box {}\n".format(
            str(i).zfill(2),
            one_bit + 1,
            subkey_bit if subkey_bit else "N/A",
            ((subkey_bit - 1) / 6) + 1 if subkey_bit else "N/A"
        )
{% endhighlight %}

{% include _understanding-crypto/previous-and-next-exercise.html %}
