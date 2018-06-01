---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 2 Solutions - Ex2.1"
description: "Developing a Stream Cipher which Operates on the Latin Alphabet"
layout: post
redirect_from: /understanding-cryptography-ex2.1/
headerImage: false
projects: false
date: 2017-12-17 18:00
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 2
exercise: 1
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 2.1

The stream cipher described in Definition 2.1.1 can easily be generalized to work in alphabets other than the binary one. For manual encryption, an especially useful one is a stream cipher that operates on letters.
1. Develop a scheme which operates with the letters A, B,..., Z, represented by the numbers 0,1,...,25. What does the key (stream) look like? What are the encryption and decryption functions?
2. Decrypt the cipher text "bsaspp kkuosp" which was encrypted using the key "rsidpy dkawoa".
3. How was the young man murdered?

### Solution

*This solution is verified as correct by the official [Solutions for Odd-Numbered Questions](http://wiki.crypto.rub.de/Buch/en/download/Understanding_Cryptography_Odd_Solutions.pdf) manual.*

1\. Assuming the keystream is a stream of random bits in $$\mathbb{Z}_{26}$$, we can define a stream cipher on the Latin Alphabet as follows (where A ↔ 0, B ↔ 1, C ↔ 2 etc):

$$ y_i = x_i + k_i\,\mathrm{mod}\,26 $$

$$ x_i = y_i − k_i\,\mathrm{mod}\,26 $$

2\. There is a mistake in the book, the key should be "rsidpy dkawoy". If you use this key to decrypt the ciphertext, we get the following message:

<pre class="pre-wrap-enabled">
KASPAR HAUSER
</pre>

3\. [Kaspar Hauser](https://en.wikipedia.org/wiki/Kaspar_Hauser) was murdered by knife wound.

I wrote a [python script](https://github.com/tombusby/understanding-cryptography-exercises/blob/master/Chapter-02/ex2.1.py) which can perform encryption and decryption with this system:

{% highlight python %}
def letter_to_number(c):
    return ord(c.lower()) - ord('a')

def number_to_letter(n):
    return chr(n % 26 + ord('a'))

def encrypt_letter(key, plaintext):
    if plaintext == " ": return plaintext
    key = letter_to_number(key)
    plaintext = letter_to_number(plaintext)
    return number_to_letter((plaintext + key) % 26)

def decrypt_letter(key, ciphertext):
    if ciphertext == " ": return ciphertext
    key = letter_to_number(key)
    ciphertext = letter_to_number(ciphertext)
    return number_to_letter((ciphertext - key) % 26)

def encrypt(key, plaintext):
    return "".join(encrypt_letter(k, p) for (k, p) in zip(key, plaintext))

def decrypt(key, ciphertext):
    return "".join(decrypt_letter(k, c) for (k, c) in zip(key, ciphertext))

def print_ciphertext_and_key(key, ciphertext):
    print "\n===="
    print "Key:"
    print "====\n"
    print key
    print "\n==========="
    print "Ciphertext:"
    print "===========\n"
    print ciphertext

def print_plaintext(plaintext):
    print "\n=========="
    print "Plaintext:"
    print "==========\n"
    print plaintext, "\n"

if __name__ == "__main__":
    ciphertext = "bsaspp kkuosp"
    key = "rsidpy dkawoy" # mistake in the key in the book, should end with y not a
    print_ciphertext_and_key(key, ciphertext)
    plaintext = decrypt(key, ciphertext)
    print_plaintext(plaintext)
    print "Q2.1.3: Kaspar Hauser was murdered via a stab wound\n"
{% endhighlight %}

{% include _understanding-crypto/previous-and-next-exercise.html %}
