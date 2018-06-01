---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 5 Solutions - Ex5.12"
description: "Calculating the Length of Brute-Force Attacks against 2DES with \"Meet in the Middle\" Attack"
layout: post
redirect_from: /understanding-cryptography-ex5.12/
headerImage: false
projects: false
date: 2018-05-13 21:36
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 5
exercise: 12
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 5.12

We now analyse the security of DES double encryption (2DES) by doing a cost-estimate:

$$ 2DES(x) = DES_{K_2}(DES_{K_1}(x)) $$

<ol>
    <li>
        First, let us assume a pure key search without any memory usage. For this purpose, the whole key space spanned by K<sub>1</sub> and K<sub>2</sub> has to be searched. How much does a key-search machine for breaking 2DES (worst case) in 1 week cost? In this case, assume ASICs which can perform 10<sup>7</sup> keys per second at a cost of $5 per IC. Furthermore, assume an overhead of 50% for building the key search machine.
    </li>
    <li>
        Let us now consider the meet-in-the-middle (or time-memory tradeoff) attack, in which we can use memory. Answer the following questions:
        <ul>
            <li>How many entries have to be stored?</li>
            <li>How many bytes (not bits!) have to be stored for each entry?</li>
            <li>How costly is a key search in one week? Please note that the key space has to be searched before filling up the memory completely. Then we can begin to search the key space of the second key. Assume the same hardware for both key spaces.</li>
        </ul>
        For a rough cost estimate, assume the following costs for hard disk space: $8 per 10 GByte, where 1 GByte = 10<sup>9</sup> Byte.
    </li>
    <li>
        Assuming Moore’s Law, when do the costs move below $1 million?
    </li>
</ol>

### Solution

*I haven't yet verified this solution independently. If you spot any mistakes, please leave a comment in the Disqus box at the bottom of the page.*

1\. DES has a 56 bit key length. This gives it a keyspace of $$ 2^{56} $$. Without using the "Meet in the Middle" attack (i.e. searching every 2-key combination) then breaking DES involves $$ (2^{56})^2 = 2^{112} $$ key-pair checks in the worst case. The worst case being that the very last possible key-pair is the correct one.

A week is 604,800 seconds. As such, we can define an equation for how many ASICs are required to naively crack a 2DES key-pair:

$$ \frac{2^{112}\,\mathsf{combinations} \times 2\,\mathsf{keys/combination}}{10^7\,\mathsf{keys/second} \times x\,\mathsf{ASICs}} = 604,800\,\mathsf{seconds} $$

We can re-arrange and simplify this equation to determine the value of $$ x $$:

$$ \frac{2^{113}\,\mathsf{keys}}{10^7\,\mathsf{keys/second} \times 604,800\,\mathsf{seconds}} = x\,\mathsf{ASICs} $$

$$ x $$ can now be trivially calculated and comes out to:

$$ x \approx 1.717 \times 10^{21} $$

This is an astonishingly large number. Assuming $5 per IC, (and a 50% overhead to build the machine) then the costs are as follows:

$$ 1.717 \times 10^{21} \times $5 \times 1.5 \approx $1.288 \times 10^{22} $$

For comparison, world GDP is currently about $$ $7.5 \times 10^{13} $$.

2\. We need to store a complete mapping of our chosen plaintext to every key in $$ K_1 $$'s keyspace from its corresponding ciphertext. This is therefore $$ 2^{56} $$ entries. These entries will be indexed by ciphertext, not key. We want to look up a key from a ciphertext, not the other way round.

For each entry, we need to store 56 bits of key and 64 bits of ciphertext. This comes out to exactly 15 bytes.

We can use similar maths as above to calulate the cost of finding the key in week. As with part 1, I will assume the question is asking for a worst case calculation, and not average case.

To make the index, we need the following amount of diskspace:

$$ \lceil (2^{56}\,\mathsf{keys} \times 15\,\mathsf{bytes}) \div 10^9\,\mathsf{bytes/GB} \rceil = 1,080,863,911\,\mathsf{GB} $$

If 1 GB costs $0.80, then the cost in dollars for enough hard disk space to make the index is as follows:

$$ 1,080,863,911\,\mathsf{GB} \times $0.80 = $864,691,128.80 $$

In order to calculate how many ASICs it will take to index the keyspace of $$ K_1 $$ in a week, we can use the same mathematics as above.

$$ \frac{2^{56}\,\mathsf{keys}}{10^7\,\mathsf{keys/second} \times 604,800\,\mathsf{seconds}} = 11,914.28\,\mathsf{ASICs} $$

Since it's impossible to have a fraction of an ASIC, that means we need 11,915 ASICs to get the index done within a week.

Now, we also need to search the keyspace of $$ K_2 $$. This will take the same amount of time in the worst case (bear in mind that the entirity of $$ K_1 $$ has to be indexed in the best and average case too). Since it's the worst case we're calculating, it's fairly easy to calculate the amount of ASICs required to perform the full attack. We need to do $$ 2 \times 2^{56} $$ checks in a week (to search $$ K_1 $$ and $$ K_2 $$ separately and independently).

Therefore we need 23,830 ASICs to get the job done. The cost of these ASICs is as follows:

$$ 23,830 \times $5 \times 1.5 = $178,725 $$

This difference from the naive search in cost is fairly extreme.

Therefore, the total cost of the attack is:

$$ $178,725 + $864,691,128.80 = $864,869,853.80 $$

The cost of storing the index dominates the costs, however the total cost is now well within the reach of the larger intelligence agencies, instead of the ridiculous exponential multiples of world GDP from part 1.

3\. In order to calculate how long Moore's Law (with the assumption that storage space costs also conform to Moore's Law) will take to bring the costs of attacking 2DES below $1 million, we first need to calculate how many iterations of the Law will make that happen.

Another way of saying that capacity doubles is to say that costs halve, so the following equation calculates the number of interations.

$$ \frac{$864,869,853.80}{2^i} = $1,000,000 $$

We can rearrange this equation to calculate $$ i $$:

$$ log_2 \frac{$864,869,853.80}{$1,000,000} = 9.756 \,\mathsf{iterations} $$

Therefore, the number of years required to bring this cost under $1,000,000 is:

$$ 1.5\,\mathsf{years} \times 8\,\mathsf{iterations} = 12\,\mathsf{years} $$

{% include _understanding-crypto/previous-and-next-exercise.html %}
