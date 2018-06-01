---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 1 Solutions - Ex1.2"
description: "Breaking a Caesar/Shift Cipher by Frequency Analysis"
layout: post
redirect_from: /understanding-cryptography-ex1.2/
headerImage: false
projects: false
date: 2017-12-16 22:40
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 1
exercise: 2
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 1.2

We received the following ciphertext which was encoded with a shift cipher:

<pre class="pre-wrap-enabled">
xultpaajcxitltlxaarpjhtiwtgxktghidhipxciwtvgtpilpitghlxiwiwtxgqadds.
</pre>

1. Perform an attack against the cipher based on a letter frequency count: How many letters do you have to identify through a frequency count to recover the key? What is the cleartext?
2. Who wrote this message?

### Solution

*This solution is verified as correct by the fact that it produces sensible output.*

1\. It's worth noting that shift ciphers have a keyspace of 25 (a mapping of 26 does not encrypt) and this is small enough for brute force. If we wish to perform an analytical attack, we need only identify one letter. This will yield the shift value, which makes up the entirety of the key. In my attack I used the letter E since the frequency gap between "most common" amd "second most common" is very stastically significant. This attack yielded the following result:

<pre class="pre-wrap-enabled">
IFWEALLUNITEWEWILLCAUSETHERIVERSTOSTAINTHEGREATWATERSWITHTHEIRBLOOD.
</pre>

2\. This was said by [Tecumseh](https://en.wikipedia.org/wiki/Tecumseh).

I wrote a [python script](https://github.com/tombusby/understanding-cryptography-exercises/blob/master/Chapter-01/ex1.2.py) which can perform this frequency analysis attack:

{% highlight python %}
import string, collections, re

def generate_key(rot):
    return zip(string.ascii_lowercase, string.ascii_uppercase[rot:] + string.ascii_uppercase[:rot])

def get_ciphertext_letters_by_frequency(ciphertext):
    ciphertext_nopunc = re.sub(r"\W+", "", ciphertext)
    letter_freqs = collections.Counter(ciphertext_nopunc)
    return [i[0] for i in letter_freqs.most_common()]

def decipher(ciphertext, rot):
    key = generate_key(rot)
    for x, y in key:
        ciphertext = ciphertext.replace(x, y)
    return ciphertext

# The keyspace is small enough (knowing that it's a shift cipher) for brute force
def brute_force(ciphertext):
    print "\n=================="
    print "Brute Force Attack"
    print "==================\n"
    for rot in range(1, 26):
        arrow = "<========== CORRECT" if rot == 11 else ""
        print "{rot:02d}: ".format(rot=rot), decipher(ciphertext, rot), arrow

# Here follows a statistical attempt to find the key
def statistical_attack(ciphertext):
    print "\n=================="
    print "Statistical Attack"
    print "==================\n"
    most_common_letter = get_ciphertext_letters_by_frequency(ciphertext)[0]
    rot = (ord('e') - ord(most_common_letter)) % 26
    print decipher(ciphertext, rot), "\n"

if __name__ == "__main__":
    ciphertext = "xultpaajcxitltlxaarpjhtiwtgxktghidhipxciwtvgtpilpitghlxiwiwtxgqadds."
    brute_force(ciphertext)
    statistical_attack(ciphertext)
    print "Q1.2.2 Answer: This was said by Tecumseh\n"
{% endhighlight %}

{% include _understanding-crypto/previous-and-next-exercise.html %}
