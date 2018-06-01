---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 1 Solutions - Ex1.4"
description: "Password Length and Key Sizes in Bits"
layout: post
redirect_from: /understanding-cryptography-ex1.4/
headerImage: false
projects: false
date: 2017-12-16 22:43
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 1
exercise: 4
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 1.4

We now consider the relation between passwords and key size. For this purpose we consider a cryptosystem where the user enters a key in the form of a password.

1. Assume a password consisting of 8 letters, where each letter is encoded by the ASCII scheme (7 bits per character, i.e., 128 possible characters). What is the size of the key space which can be constructed by such passwords?
2. What is the corresponding key length in bits?
3. Assume that most users use only the 26 lowercase letters from the alphabet instead of the full 7 bits of the ASCII-encoding. What is the corresponding key length in bits in this case?
4. At least how many characters are required for a password in order to generate a key length of 128 bits in case of letters consisting of a) 7-bit characters? b) 26 lowercase letters from the alphabet?

### Solution

*I haven't yet verified this solution independently. If you spot any mistakes, please leave a comment in the Disqus box at the bottom of the page.*

1\. There are 128 ($$2^7$$) possible characters. If the password is 8 characters long, we can calculate the number of possible 8-character passwords as follows:

<div style="text-align: center;">
$$ 128^8 = 2^{56} \approx 7.205 \times 10^{16} $$
</div>

2\. The key length in bits is what power of two corresponds to the total number of possible passwords. We've already calculated that there are $$2^{56}$$ possible passwords. The key length is therefore 56.

3\. If the password is restricted to the lower case letters, then this reduces the number of possible characters from 127 to 26. We can then recalculate the number of possible 8-letter passwords:

<div style="text-align: center;">
$$ 26^8 = 208827064576\,\mathsf{keys} $$
</div>

We can then calculate what length key in bits this is equivalent to:

<div style="text-align: center;">
$$ \log_2 208827064576 \approx 37.6035177451\,\mathsf{bits} $$
</div>

4\. In order to calculate what password length corresponds to 128 bits, we need to work out what power of the number of possible characters corresponds to $$2^{128}$$.

a) Since 128 is also a power of 2, the equations can be understood in a simple way, where $$i$$ is the character length required to create the equivalent of a $$2^{128}$$ bit key:

<div style="text-align: center;">
$$ 128^i = 2^{7i} = 2^{128} $$
</div>

If we take the powers of two and do $$\log_2$$ on both sides, we can solve this equation fairly trivially:

<div style="text-align: center;">
$$ 7i = 128 \\ i = 128 \div 7 \\ i \approx 18.285\,\mathsf{ASCII\,characters} $$
</div>

b) This isn't as trivially solved, since 26 can't be cleanly represented as a power of 2. However, the equations are still fairly simple:

<div style="text-align: center;">
$$ 26^i = 2^{128} \\ i = \log_{26}\,2^{128} \\ i \approx 27.231\,\mathsf{lower\,case\,letters} $$
</div>


{% include _understanding-crypto/previous-and-next-exercise.html %}
