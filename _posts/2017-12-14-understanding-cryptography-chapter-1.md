---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 1 Solutions"
description: "A full solution-set for the problems at the end of Chapter 1 of Understanding Cryptography"
post_url: understanding-crypto-chapter-1
layout: post
headerImage: false
projects: false
date: 2017-12-14 00:56
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: chapter
externalLink: false
---

* [Return to index](/understanding-cryptography-full-solution-set/)
* [Exercise 1.1](#exercise-11)
* [Exercise 1.2](#exercise-12)
* [Exercise 1.3](#exercise-13)
* [Exercise 1.4](#exercise-14)
* [Exercise 1.5](#exercise-15)
* [Exercise 1.6](#exercise-16)
* [Exercise 1.7](#exercise-17)

## Exercise 1.1

The ciphertext below was encrypted using a substitution cipher.

Decrypt the ciphertext without knowledge of the key.

<pre class="pre-wrap-enabled">
lrvmnir bpr sumvbwvr jx bpr lmiwv yjeryrkbi jx qmbm wi bpr xjvni mkd ymibrut jx irhx wi bpr riirkvr jx ymbinlmtmipw utn qmumbr dj w ipmhh but bj rhnvwdmbr bpr yjeryrkbi jx bpr qmbm mvvjudwko bj yt wkbrusurbmbwjk lmird jk xjubt trmui jx ibndt

wb wi kjb mk rmit bmiq bj rashmwk rmvp yjeryrkb mkd wbi iwokwxwvmkvr mkd ijyr ynib urymwk nkrashmwkrd bj ower m vjyshrbr rashmkmbwjk jkr cjnhd pmer bj lr fnmhwxwrd mkd wkiswurd bj invp mk rabrkb bpmb pr vjnhd urmvp bpr ibmbr jx rkhwopbrkrd ywkd vmsmlhr jx urvjokwgwko ijnkdhrii ijnkd mkd ipmsrhrii ipmsr w dj kjb drry ytirhx bpr xwkmh mnbpjuwbt lnb yt rasruwrkvr cwbp qmbm pmi hrxb kj djnlb bpmb bpr xjhhjcwko wi bpr sujsru msshwvmbwjk mkd wkbrusurbmbwjk w jxxru yt bprjuwri wk bpr pjsr bpmb bpr riirkvr jx jqwkmcmk qmumbr cwhh urymwk wkbmvb
</pre>

1. Compute the relative frequency of all letters A...Z in the ciphertext. You may want to use a tool such as the open-source program CrypTool [50] for this task. However, a paper and pencil approach is also still doable.
2. Decrypt the ciphertext with the help of the relative letter frequency of the English language (see Table 1.1 in Sect. 1.2.2). Note that the text is relatively short and that the letter frequencies in it might not perfectly align with that of general English language from the table.
3. Who wrote the text?

### Solution

*This solution is verified as correct by the official [Solutions for Odd-Numbered Questions][solution-manual] manual.*

1\. Tallying up the letter frequencies produces the table below:

![Frequency table]({{ site.url }}/assets/images/ex1-1-freq-table.png){: .image-center}

2\. Pairing this frequency table with the list of most common letters in the English language does not yield a key which produces intelligible text. However, enough words "stick out" that it's fairly simple to swap mismatches manually until the plaintext reveals itself:

<pre class="pre-wrap-enabled">
BECAUSE THE PRACTICE OF THE BASIC MOVEMENTS OF KATA IS THE FOCUS AND MASTERY OF SELF IS THE ESSENCE OF MATSUBAYASHI RYU KARATE DO I SHALL TRY TO ELUCIDATE THE MOVEMENTS OF THE KATA ACCORDING TO MY INTERPRETATION BASED ON FORTY YEARS OF STUDY

IT IS NOT AN EASY TASK TO EXPLAIN EACH MOVEMENT AND ITS SIGNIFICANCE AND SOME MUST REMAIN UNEXPLAINED TO GIVE A COMPLETE EXPLANATION ONE WOULD HAVE TO BE QUALIFIED AND INSPIRED TO SUCH AN EXTENT THAT HE COULD REACH THE STATE OF ENLIGHTENED MIND CAPABLE OF RECOGNIZING SOUNDLESS SOUND AND SHAPELESS SHAPE I DO NOT DEEM MYSELF THE FINAL AUTHORITY BUT MY EXPERIENCE WITH KATA HAS LEFT NO DOUBT THAT THE FOLLOWING IS THE PROPER APPLICATION AND INTERPRETATION I OFFER MY THEORIES IN THE HOPE THAT THE ESSENCE OF OKINAWAN KARATE WILL REMAIN INTACT
</pre>

3\. This was said by [Shoshin Nagamine](https://en.wikipedia.org/wiki/Sh%C5%8Dshin_Nagamine).

I wrote a [python script](https://github.com/tombusby/understanding-cryptography-exercises/blob/master/Chapter%2001/ex1.1.py) which can perform this frequency analysis attack:

{% highlight python %}
import collections, re

def get_ciphertext_letters_by_frequency(ciphertext):
    ciphertext_nopunc = re.sub(r"\W+", "", ciphertext)
    letter_freqs = collections.Counter(ciphertext_nopunc)
    return [i[0] for i in letter_freqs.most_common()]

def reconstruct_key(ciphertext_letters):
    letters_ordered_by_freq = ["E","T","A","O","I","N","S","R","H","D","L",
        "U","C","M","F","Y","W","G","P","B","V","K","X","Q","J","Z"]
    return zip(ciphertext_letters, letters_ordered_by_freq)

def get_manually_tweaked_key():
    # The statistical analysis produced a plaintext that was still garbled
    # but certain words stood out. I swapped these mismatches manually until
    # the true plaintext revealed itself. This was very easy given the
    # headstart provided by the statistical analysis
    return [
        ('r', 'E'), ('b', 'T'), ('m', 'A'),
        ('k', 'N'), ('j', 'O'), ('w', 'I'),
        ('i', 'S'), ('p', 'H'), ('u', 'R'),
        ('d', 'D'), ('h', 'L'), ('v', 'C'),
        ('x', 'F'), ('y', 'M'), ('n', 'U'),
        ('s', 'P'), ('t', 'Y'), ('l', 'B'),
        ('o', 'G'), ('q', 'K'), ('a', 'X'),
        ('c', 'W'), ('e', 'V'), ('g', 'Z'),
        ('f', 'Q')
    ]

def decipher(ciphertext, key):
    for x, y in key:
        ciphertext = ciphertext.replace(x, y)
    return ciphertext

def print_ciphertext(ciphertext):
    print "\n==========="
    print "Ciphertext:"
    print "==========="
    print ciphertext

def print_deciphered_plaintext(type, key, plaintext):
    header = "{} Statistical Key Reconstruction:".format(type)
    print "=" * len(header), "\n", header, "\n", "=" * len(header)
    print "\n", "Key:", "\n"
    key.sort(key=lambda x: x[1])
    i = 0
    for x, y in key:
        print "{} = {},".format(y, x),
        i += 1
        if not i % 5: print
    print "\n", plaintext

with open("ex1.1-ciphertext.txt") as f:
    ciphertext = f.read()
    print_ciphertext(ciphertext)
    ciphertext_letters = get_ciphertext_letters_by_frequency(ciphertext)
    key = reconstruct_key(ciphertext_letters)
    print_deciphered_plaintext("Naive", key, decipher(ciphertext, key))
    key = get_manually_tweaked_key()
    print_deciphered_plaintext("Manually Tweaked", key, decipher(ciphertext, key))
    print "Q1.1.3 Answer: This was said by Shoshin Nagamine", "\n"
{% endhighlight %}

---

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

I wrote a [python script](https://github.com/tombusby/understanding-cryptography-exercises/blob/master/Chapter%2001/ex1.2.py) which can perform this frequency analysis attack:

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

---

## Exercise 1.3

We consider the long-term security of the Advanced Encryption Standard
(AES) with a key length of 128-bit with respect to exhaustive key-search attacks.

AES is perhaps the most widely used symmetric cipher at this time.

1. Assume that an attacker has a special purpose application specific integrated circuit (ASIC) which checks $5 \times 10^8$ keys per second, and she has a budget of $1 million. One ASIC costs $50, and we assume 100% overhead for integrating the ASIC (manufacturing the printed circuit boards, power supply, cooling, etc.). How many ASICs can we run in parallel with the given budget? How long does an average key search take? Relate this time to the age of the Universe, which is about $$10^{10}$$ years.
2. We try now to take advances in computer technology into account. Predicting the future tends to be tricky but the estimate usually applied is Moore’s Law, which states that the computer power doubles every 18 months while the costs of integrated circuits stay constant. How many years do we have to wait until a key-search machine can be built for breaking AES with 128 bit with an average search time of 24 hours? Again, assume a budget of $1 million (do not take inflation into account).

### Solution

*This solution is verified as correct by the official [Solutions for Odd-Numbered Questions][solution-manual] manual.*

1\. By the numbers specified, each machine costs \$100.

<div style="text-align: center;">
$$ $1,000,000 \div $100 = 10,000\,\mathsf{machines} $$.
</div>

Each of our 10,000 machines checks $5 \times 10^8$ keys per second, so we can calculate the speed of our parallelised machines as:

<div style="text-align: center;">
$$ 5 \times 10^8 \times 10^4 = 5 \times 10^{12}\,\mathsf{keys/second} $$
</div>

On average, we'll find the correct key halfway through our search of the keyspace ($$2^{128}$$), so the average case will be $$2^{127}$$ checks.

To calculate the length of time to find a key in the average case:

<div style="text-align: center;">
$$ \frac{2^{127}\,\mathsf{keys}}{5 \times 10^{12}\,\mathsf{keys/second}} \approx 3.4 \times 10^{25}\,\mathsf{seconds}$$
</div>

This comes out to:

<div style="text-align: center;">
$$ 1.08 \times 10^{18}\,\mathsf{years} $$
</div>

This is approximately $$10^8$$ times longer than the elapsed age of the universe.

2\. If we represent the number of Moore's Law iterations to bring the using $$i$$ then we can express the equation as:

<div style="text-align: center;">
$$ 1.08 \times 10^{18}\,\mathsf{years} \times \frac{365}{2^i} = 1\,\mathsf{day} $$
</div>

We can rearrange this equation to make $$2^i$$ the only item on the left:

<div style="text-align: center;">
$$ 2^i = 1.08 \times 10^{18} \times 365\,\mathsf{days} $$
</div>

The reveals a value of $$i$$ which is roughly $$68.42$$.

Rounding the number of iterations to 69 allows us to calculate the number of years:

<div style="text-align: center;">
$$ 1.5\,\mathsf{years} \times 69\,\mathsf{iterations} = 103.5\,\mathsf{years} $$
</div>

---

## Exercise 1.4

We now consider the relation between passwords and key size. For this purpose we consider a cryptosystem where the user enters a key in the form of a password.

1. Assume a password consisting of 8 letters, where each letter is encoded by the ASCII scheme (7 bits per character, i.e., 128 possible characters). What is the size of the key space which can be constructed by such passwords?
2. What is the corresponding key length in bits?
3. Assume that most users use only the 26 lowercase letters from the alphabet instead of the full 7 bits of the ASCII-encoding. What is the corresponding key length in bits in this case?
4. At least how many characters are required for a password in order to generate a key length of 128 bits in case of letters consisting of a) 7-bit characters? b) 26 lowercase letters from the alphabet?

### Solution

*I haven't yet verified these solutions independently. If you spot any mistakes, please leave a comment in the Disqus box at the bottom of the page.*

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

4\. In order to calculate what password length corresponds to 128 bits, we need to work out what power of the number of characters corresponds to $$2^{128}$$.

a)

<div style="text-align: center;">
$$ 26^i = 2^{128} \\ i = log_{26}\,2^{128} \\ i \approx 27.231\,\mathsf{lower\,case\,letters} $$
</div>

b)

We could calculate this in the same way, but since 128 is also a power of 2, the equations can be understood in a simpler way, where $$i$$ is the character length required to create the equivalent of a $$2^{128}$$ bit key:

<div style="text-align: center;">
$$ 128^i = 2^{7i} = 2^{128} $$
</div>

If we take the powers of two and do $$log_2$$ on both sides, we can solve this equation fairly trivially:

<div style="text-align: center;">
$$ 7i = 128 \\ i = 128 \div 7 \\ i \approx 18.285\,\mathsf{ASCII\,characters} $$
</div>

---

## Exercise 1.5

As we learned in this chapter, modular arithmetic is the basis of many cryptosystems. As a consequence, we will address this topic with several problems in this and upcoming chapters.

Compute the result without a calculator:

1. 15 · 29 mod 13
2. 2 · 29 mod 13
3. 2 · 3 mod 13
4. −11 · 3 mod 13

The results should be given in the range from 0,1,..., modulus-1. Briefly describe the relation between the different parts of the problem.

### Solution

*This solution is verified as correct by the official [Solutions for Odd-Numbered Questions][solution-manual] manual.*

We can compute these by reducing the individual terms (since all members of an equivalence class behave the same), performing the arithmetic and then reducing the result:

1\. $$ 15 \times 29\,\mathrm{mod}\,13 \equiv 2 \times 3\,\mathrm{mod}\,13 \equiv 6\,\mathrm{mod}\,13 $$

2\. $$ 2 \times 29\,\mathrm{mod}\,13 \equiv 2 \times 3\,\mathrm{mod}\,13 \equiv 6\,\mathrm{mod}\,13 $$

3\. $$ 2 \times 3\,\mathrm{mod}\,13 \equiv 6\,\mathrm{mod}\,13 $$

4\. $$ -11 \times 3\,\mathrm{mod}\,13 \equiv 2 \times 3\,\mathrm{mod}\,13 \equiv 6\,\mathrm{mod}\,13 $$

---

## Exercise 1.6

Compute without a calculator:

1. 1/5 mod 13
2. 1/5 mod 7
3. 3 · 2/5 mod 7

### Solution

*I haven't yet verified these solutions independently. If you spot any mistakes, please leave a comment in the Disqus box at the bottom of the page.*

In order to perform a division by $$x$$, we must find the multiplicative inverse $$x^{-1}$$ and multiply by it.

1\.

$$
1 \div 5\,\mathrm{mod}\,13 \equiv 1 \times 5^{-1}\,\mathrm{mod}\,13 \\
\mathsf{where}\,5 \times 5^{-1} \,\mathrm{mod}\,13 \equiv 1\,\mathrm{mod}\,13
$$

$$ 5 \times 8\,\mathrm{mod}\,13 \equiv 1\,\mathrm{mod}\,13 \\ 5^{-1}\,\mathrm{mod}\,13 \equiv 8\,\mathrm{mod}\,13$$

$$ 1 \div 5\,\mathrm{mod}\,13 \equiv 1 \times 8\,\mathrm{mod}\,13 \equiv 8\,\mathrm{mod}\,13 $$

2\.

$$
1 \div 5\,\mathrm{mod}\,7 \equiv 1 \times 5^{-1}\,\mathrm{mod}\,7 \\
\mathsf{where}\,5 \times 5^{-1} \,\mathrm{mod}\,7 \equiv 1\,\mathrm{mod}\,7
$$

$$ 5 \times 3\,\mathrm{mod}\,7 \equiv 1\,\mathrm{mod}\,7 \\ 5^{-1}\,\mathrm{mod}\,7 \equiv 3\,\mathrm{mod}\,7$$

$$ 1 \div 5\,\mathrm{mod}\,7 \equiv 1 \times 3\,\mathrm{mod}\,7 \equiv 3\,\mathrm{mod}\,7 $$

3\.

$$
3 \times 2 \div 5\,\mathrm{mod}\,7 \equiv 3 \times 2 \times 5^{-1}\,\mathrm{mod}\,7 \\
\mathsf{where}\,2 \times 5^{-1} \,\mathrm{mod}\,7 \equiv 1\,\mathrm{mod}\,7
$$

$$ 5 \times 3\,\mathrm{mod}\,7 \equiv 1\,\mathrm{mod}\,7 \\ 5^{-1}\,\mathrm{mod}\,7 \equiv 3\,\mathrm{mod}\,7$$

$$
3 \times 2 \div 5\,\mathrm{mod}\,7 \equiv 3 \times 2 \times 3\,\mathrm{mod}\,7 \equiv 4\,\mathrm{mod}\,7 \\
\mathsf{because}\,3 \times 2 \times 3\,\mathrm{mod}\,7 \equiv 18\,\mathrm{mod}\,7 \equiv 4\,\mathrm{mod}\,7
$$

---

## Exercise 1.7

We consider the ring Z4. Construct a table which describes the addition of all
elements in the ring with each other:

1. Construct the multiplication table for $$\mathbb{Z}_4$$.
2. Construct the addition and multiplication tables for $$\mathbb{Z}_5$$.
3. Construct the addition and multiplication tables for $$\mathbb{Z}_6$$.
4. There are elements in Z4 and Z6 without a multiplicative inverse. Which elements are these? Why does a multiplicative inverse exist for all non-zero elements in $$\mathbb{Z}_5$$?

### Solution

*This solution is verified as correct by the official [Solutions for Odd-Numbered Questions][solution-manual] manual.*

1\. Multiplication table for $$\mathbb{Z}_4$$:

<div style="text-align: center;">
<script type="math/tex">
\begin{array}{c|c c c c}
\times & \text{0} & \text{1} & \text{2} & \text{3} \\ \hline
\text{0} & 0 & 0 & 0 & 0 \\
\text{1} & 0 & 1 & 2 & 3 \\
\text{2} & 0 & 2 & 0 & 2 \\
\text{3} & 0 & 3 & 2 & 1 \\
\end{array}
</script>
</div>

2\. Addition and Multiplication Tables for $$\mathbb{Z}_5$$:

<div style="text-align: center;">
<script type="math/tex">
\begin{array}{c|c c c c c}
+ & \text{0} & \text{1} & \text{2} & \text{3} & \text{4} \\ \hline
\text{0} & 0 & 1 & 2 & 3 & 4 \\
\text{1} & 1 & 2 & 3 & 4 & 0 \\
\text{2} & 2 & 3 & 4 & 0 & 1 \\
\text{3} & 3 & 4 & 0 & 1 & 2 \\
\text{4} & 4 & 0 & 1 & 2 & 3 \\
\end{array}
</script>
&nbsp;
<script type="math/tex">
\begin{array}{c|c c c c c}
\times & \text{0} & \text{1} & \text{2} & \text{3} & \text{4} \\ \hline
\text{0} & 0 & 0 & 0 & 0 & 0 \\
\text{1} & 0 & 1 & 2 & 3 & 4 \\
\text{2} & 0 & 2 & 4 & 1 & 3 \\
\text{3} & 0 & 3 & 1 & 4 & 2 \\
\text{4} & 0 & 4 & 3 & 2 & 1 \\
\end{array}
</script>
</div>

3\. Addition and Multiplication Tables for $$\mathbb{Z}_6$$:

<div style="text-align: center;">
<script type="math/tex">
\begin{array}{c|c c c c c c}
+ & \text{0} & \text{1} & \text{2} & \text{3} & \text{4} & \text{5} \\ \hline
\text{0} & 0 & 1 & 2 & 3 & 4 & 5 \\
\text{1} & 1 & 2 & 3 & 4 & 5 & 0 \\
\text{2} & 2 & 3 & 4 & 5 & 0 & 1 \\
\text{3} & 3 & 4 & 5 & 0 & 1 & 2 \\
\text{4} & 4 & 5 & 0 & 1 & 2 & 3 \\
\text{4} & 5 & 0 & 1 & 2 & 3 & 4 \\
\end{array}
</script>
&nbsp;
<script type="math/tex">
\begin{array}{c|c c c c c c}
\times & \text{0} & \text{1} & \text{2} & \text{3} & \text{4} & \text{5} \\ \hline
\text{0} & 0 & 0 & 0 & 0 & 0 & 0 \\
\text{1} & 0 & 1 & 2 & 3 & 4 & 5 \\
\text{2} & 0 & 2 & 4 & 0 & 2 & 4 \\
\text{3} & 0 & 3 & 0 & 3 & 0 & 3 \\
\text{4} & 0 & 4 & 2 & 0 & 4 & 2 \\
\text{5} & 0 & 5 & 4 & 3 & 2 & 1 \\
\end{array}
</script>
</div>

4\. To determine whether an element has a multiplicative inverse, we must check if it can be multiplied with another element to produce 1:

Elements without a multiplicative inverse in $$\mathbb{Z}_4$$ are 2 and 0

Elements without a multiplicative inverse in $$\mathbb{Z}_6$$ are 2, 3, 4 and 0

For all non-zero elements of $$\mathbb{Z}_5$$, there exists a multiplicative inverse because 5 is a prime. Hence, all non-zero elements smaller than 5 are relatively prime to 5.






[solution-manual]: http://wiki.crypto.rub.de/Buch/en/download/Understanding_Cryptography_Odd_Solutions.pdf
