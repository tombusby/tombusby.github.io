---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - Chapter 4 Solutions - Ex4.3"
description: "Computing Addition and Multiplication Tables in Galois Extension Fields"
layout: post
headerImage: false
projects: false
date: 2017-12-23 22:11
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: understanding-crypto
chapter: 4
exercise: 3
externalLink: false
---

<style type="text/css">

    @media (max-width: 768px) {
        #polynomial-table {
            overflow-x: scroll;
        }
    }

</style>

{% include _understanding-crypto/chapter-menu.html %}

## Exercise 4.3

Generate the multiplication table for the extension field $$GF(2^3)$$ for the case that the irreducible polynomial is $$P(x) = x^3 + x + 1$$. The multiplication table is in this case a $$8 \times 8$$ table. (Remark: You can do this manually or write a program for it.)

### Solution

*This solution is verified as correct by the official [Solutions for Odd-Numbered Questions](http://wiki.crypto.rub.de/Buch/en/download/Understanding_Cryptography_Odd_Solutions.pdf) manual.*

<div style="text-align: center; font-size: 11.5px" id="polynomial-table">
<script type="math/tex">
\begin{array}{c|c c c c c}
\times & 0 & 1 & x & x + 1 & x^2 & x^2 + 1 & x^2 + x & x^2 + x + 1 \\ \hline
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
1 & 0 & 1 & x & x + 1 & x^2 & x^2 + 1 & x^2 + x & x^2 + x + 1 \\
x & 0 & x & x^2 & x^2 + x & x + 1 & 1 & x^2 + x + 1 & x^2 + 1 \\
x + 1 & 0 & x + 1 & x^2 + x & x^2 + 1 & x^2 + x + 1 & x^2 & 1 & x \\
x^2 & 0 & x^2 & x + 1 & x^2 + x + 1 & x^2 + x & x & x^2 + 1 & 1 \\
x^2 + 1 & 0 & x^2 + 1 & 1 & x^2 & x & x^2 + x + 1 & x + 1 & x^2 + x \\
x^2 + x & 0 & x^2 + x & x^2 + x + 1 & 1 & x^2 + 1 & x + 1 & x & x^2 \\
x^2 + x + 1 & 0 & x^2 + x + 1 & x^2 + 1 & x & 1 & x^2 + x & x^2 & x + 1
\end{array}
</script>
</div>

I wrote a [python script](https://github.com/tombusby/understanding-cryptography-exercises/blob/master/Chapter-04/ex4.3.py) which can calculate multiplication tables for $$GF(2^3)$$ fields (the class has since been split into a module):

{% highlight python %}
class Mod2Polynomial:

    def __init__(self, vector):
        while len(vector) and not vector[-1]:
            vector.pop()
        self.vector = [0] * len(vector)
        i = 0
        for p in vector:
            self.vector[i] = int(bool(p))
            i += 1

    def __str__(self):
        def monomial_str(order):
            if order == 1:
                return "x"
            elif order > 0:
                return "x^{}".format(order)
            else:
                return "1"
        indexed_coefficients = zip(self.vector, range(0, len(self.vector)))
        polynomial_string = filter(bool, [monomial_str(o) if c else "" for c, o in indexed_coefficients])
        return " + ".join(reversed(polynomial_string)) if polynomial_string else "0"

    def __add__(self, polynomial):
        size = max(self.get_order(), polynomial.get_order()) + 1
        a = self.vector + [0] * (size - len(self.vector))
        b = polynomial.vector + [0] * (size - len(polynomial.vector))
        return Mod2Polynomial([(ac + bc) % 2 for ac, bc in zip(a, b)])

    def __mul__(self, polynomial):
        if self.is_zero() or b.is_zero():
            return Mod2Polynomial([])
        vector = [0] * (self.get_order() + polynomial.get_order() + 2)
        i1 = 0
        for p1 in self.vector:
            i2 = 0
            for p2 in polynomial.vector:
                vector[i1 + i2] = (vector[i1 + i2] + (p1 * p2)) % 2
                i2 += 1
            i1 += 1
        return Mod2Polynomial(vector)

    def __mod__(self, divisor):
        def make_monomial(n):
            vector = [0] * (n+1)
            vector[n] = 1
            return Mod2Polynomial(vector)
        dividend = Mod2Polynomial(self.vector[:])
        quotient = Mod2Polynomial([0] * (max(self.get_order(), divisor.get_order()) + 1))
        divisor_order = divisor.get_order()
        if not divisor_order:
            raise ZeroDivisionError
        while divisor_order <= dividend.get_order():
            monomial = make_monomial(dividend.get_order() - divisor_order)
            quotient += monomial
            dividend += (monomial * divisor)
        return dividend

    def get_order(self):
        i = 0
        order = 0
        for p in self.vector:
            if p: order = i
            i += 1
        return order

    def is_zero(self):
        return not any(self.vector)


if __name__ == "__main__":
    # smallest power (x^0) on the left, largest on the right
    mod = Mod2Polynomial([1, 1, 0, 1])
    def all_poss_vectors():
        # ordering reversal counters the lowest-power-first input to Mod2Polynomial and
        # ensures that the ordering of the for loop below is not nonsensical
        return [[c,b,a] for a in [0,1] for b in [0,1] for c in [0,1]]
    for (a, b) in [(a, b) for a in all_poss_vectors() for b in all_poss_vectors()]:
        a = Mod2Polynomial(a)
        b = Mod2Polynomial(b)
        print str(a), "*", str(b), "\n=", str((a * b) % mod), "\n"
{% endhighlight %}

{% include _understanding-crypto/previous-and-next-exercise.html %}
