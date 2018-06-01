---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 3 Solutions - Ex3.13"
description: "Calculating the State After Round 1 for the PRESENT-80 Block Cipher"
layout: post
redirect_from: /understanding-cryptography-ex3.13/
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

I wrote a [python script](https://github.com/tombusby/understanding-cryptography-exercises/blob/master/Chapter-03/ex3.13.py) which can perform PRESENT-80 encryption:

*Note*: This is not a remotely efficient implementation of PRESENT-80.

{% highlight python %}
from bitarray import bitarray

def generate_round_keys(key, verbose=False):
    yield key[:64]
    for i in range(1, 32):
        key = key[-19:] + key[:-19]
        if verbose:
            print "KeyState After Rotation:\t", bit_array_to_hex(key)
        key = sbox(key[:4]) + key[4:]
        if verbose:
            print "KeyState After S-box:\t\t", bit_array_to_hex(key)
        round_counter = make_bit_array(5, i)
        key = key[:-20] + (key[-20:-15] ^ round_counter) + key[-15:]
        if verbose:
            print "KeyState After Counter Add:\t", bit_array_to_hex(key)
        yield key[:64]

def sbox(n):
    sbox = {
        '0000': bitarray('1100'),
        '0001': bitarray('0101'),
        '0010': bitarray('0110'),
        '0011': bitarray('1011'),
        '0100': bitarray('1001'),
        '0101': bitarray('0000'),
        '0110': bitarray('1010'),
        '0111': bitarray('1101'),
        '1000': bitarray('0011'),
        '1001': bitarray('1110'),
        '1010': bitarray('1111'),
        '1011': bitarray('1000'),
        '1100': bitarray('0100'),
        '1101': bitarray('0111'),
        '1110': bitarray('0001'),
        '1111': bitarray('0010'),
    }
    return sbox[n.to01()][:]

def apply_sbox(ciphertext):
    return \
        sbox(ciphertext[0:4]) + \
        sbox(ciphertext[4:8]) + \
        sbox(ciphertext[8:12]) + \
        sbox(ciphertext[12:16]) + \
        sbox(ciphertext[16:20]) + \
        sbox(ciphertext[20:24]) + \
        sbox(ciphertext[24:28]) + \
        sbox(ciphertext[28:32]) + \
        sbox(ciphertext[32:36]) + \
        sbox(ciphertext[36:40]) + \
        sbox(ciphertext[40:44]) + \
        sbox(ciphertext[44:48]) + \
        sbox(ciphertext[48:52]) + \
        sbox(ciphertext[52:56]) + \
        sbox(ciphertext[56:60]) + \
        sbox(ciphertext[60:64])

def apply_pLayer(ciphertext):
    temp = ciphertext[:]
    temp.setall(0)
    for i in range(0, 64):
        if i == 63:
            temp[63] = ciphertext[63]
        else:
            pi = (i * 16) % 63
            temp[pi] = ciphertext[i]
    return temp

def present_80_encrypt(plaintext, key, verbose=False):
    ciphertext = make_bit_array(64, plaintext)
    key = make_bit_array(80, key)
    i = 0
    for ki in generate_round_keys(key, verbose):
        i += 1
        ciphertext ^= ki
        if verbose:
            print "Round {i:02d} Key:\t\t\t".format(i=i), bit_array_to_hex(ki)
            print "State After KeyAdd:\t\t", bit_array_to_hex(ciphertext)
        if i == 32:
            break
        ciphertext = apply_sbox(ciphertext)
        if verbose:
            print "State After SBox:\t\t", bit_array_to_hex(ciphertext)
        ciphertext = apply_pLayer(ciphertext)
        if verbose:
            print "State After pLayer:\t\t", bit_array_to_hex(ciphertext)
        if verbose:
            print "--------"

def make_bit_array(length, n):
    return bitarray(bin(n).lstrip('-0b').zfill(length))

def bit_array_to_hex(b):
    return hex(int(b.to01(), 2)).lstrip('-0x').rstrip('L-').zfill(16)

if __name__ == "__main__":
    plaintext = int('0000000000000000', 16)
    key = int('BBBB55555555EEEEFFFF', 16)
    present_80_encrypt(plaintext, key, verbose=True)

{% endhighlight %}

{% include _understanding-crypto/previous-and-next-exercise.html %}
