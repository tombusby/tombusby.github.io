---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 2 Solutions - Ex2.12"
description: "Calculating the First 70 Bits of Output During Trivium's Warm-up Phase"
layout: post
headerImage: false
projects: false
date: 2017-12-19 19:14
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 2
exercise: 12
externalLink: false
---

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 2.12

Assume the IV and the key of Trivium each consist of 80 all-zero bits. Compute the first 70 bits $$ s_1, ..., s_70 $$ during the warm-up phase of Trivium. Note that these are only internal bits which are not used for encryption since the warm-up phase lasts for 1152 clock cycles.

### Solution

*I haven't yet verified this solution independently. If you spot any mistakes, please leave a comment in the Disqus box at the bottom of the page.*

I wrote a [python script](https://github.com/tombusby/understanding-cryptography-exercises/blob/master/Chapter%2002/ex2.12.py) which inplements Trivium:

This script reveals that the first 70 bits of output are:

<div style="text-align: center; margin-bottom: 20px">
<script type="math/tex">
\begin{array}{c|c}
s_1 & s_2 & s_3 & s_4 & s_5 & ... & s_{67} & s_{68} & s_{69} & s_{70}\\ \hline
0 & 1 & 1 & 0 & 0 & 0 & 0 & 1 & 1 & 0
\end{array}
</script>
</div>

The source code is shown below:

*Note*: This is not a remotely efficient implementation of Trivium.

{% highlight python %}
import time

class LFSR(object):

    def __init__(self, reg_len, feedback, feedforward):
        self.registers = [0] * reg_len
        self.feedback = feedback
        self.feedforward = feedforward

    def initialise_registers(self, init_vector):
        self.registers = [0] * (len(self.registers) - len(init_vector)) + init_vector

    def get_output_bit(self):
        return (self.registers[-1] + self._get_feedforward_output() + self._get_and_output()) % 2

    def iterate(self, input_bit):
        self.registers.pop()
        self.registers.insert(0, (input_bit + self._get_feedback_output()) % 2)

    def show_internal_state(self):
        print self.registers
        print "Output bit:", self.get_output_bit(), "\nFeedBack bit:", self._get_feedback_output(), "\n"

    def _get_and_output(self):
        return self.registers[-3] * self.registers[-2]

    def _get_feedback_output(self):
        return self.registers[self.feedback]

    def _get_feedforward_output(self):
        return self.registers[self.feedforward]

class Trivium(object):

    def __init__(self, init_vector, key):
        self.a = LFSR(93, 68, 65)
        self.b = LFSR(84, 77, 68)
        self.c = LFSR(111, 86, 65)
        self.a.initialise_registers(init_vector + [0] * 13)
        self.b.initialise_registers(key + [0] * 4)
        self.c.initialise_registers([1, 1, 1])

    def show_internal_state(self):
        print "A:"
        self.a.show_internal_state()
        print "B:"
        self.b.show_internal_state()
        print "C:"
        self.c.show_internal_state()

    def get_output_bit(self):
        return (self.a.get_output_bit() + self.b.get_output_bit() + self.c.get_output_bit()) % 2

    def iterate(self):
        a_output = self.a.get_output_bit()
        b_output = self.b.get_output_bit()
        c_output = self.c.get_output_bit()
        self.a.iterate(c_output)
        self.b.iterate(a_output)
        self.c.iterate(b_output)

if __name__ == "__main__":
    keystream_generator = Trivium([], [])
    bits = []
    for i in range(0,70):
        bits.append(keystream_generator.get_output_bit())
        keystream_generator.iterate()
    print bits
    print len(bits)
{% endhighlight %}

{% include _understanding-crypto/previous-and-next-exercise.html %}
