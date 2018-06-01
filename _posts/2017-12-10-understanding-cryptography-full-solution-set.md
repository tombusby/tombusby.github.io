---
title: "Understanding Cryptography by Christof Paar and Jan Pelzl - All Problems and Solutions"
description: "An set of posts which provides a full list of problem/exercise solutions to the exercises in Understanding Cryptography including even-numbered questions."
layout: post
headerImage: false
projects: true
date: 2017-12-10 00:50
tag:
- cryptography
- understanding-cryptography
- even-numbered-solutions
blog: false
author: tombusby
category: project
externalLink: false
---

## Useful links

+ [Official Website for "Understanding Cryptography" - crypto-textbook.com](http://www.crypto-textbook.com/)
+ [University Lecture Series Covering the Material from the Book](https://www.youtube.com/channel/UC1usFRN4LCMcfIV7UjHNuQg/videos) - taught by Christof Paar
+ Problem sets from the book:
  + [Chapter 1](http://wiki.crypto.rub.de/Buch/en/download/problems_only/problems_chaptr_1.pdf)
  + [Chapter 2](http://wiki.crypto.rub.de/Buch/en/download/problems_only/problems_chaptr_2.pdf)
  + [Chapter 3](http://wiki.crypto.rub.de/Buch/en/download/problems_only/problems_chaptr_3.pdf)
  + [Chapter 4](http://wiki.crypto.rub.de/Buch/en/download/problems_only/problems_chaptr_4.pdf)
  + [Chapter 5](http://wiki.crypto.rub.de/Buch/en/download/problems_only/problems_chaptr_5.pdf)
  + [Chapter 6](http://wiki.crypto.rub.de/Buch/en/download/problems_only/problems_chaptr_6.pdf)
  + [Chapter 7](http://wiki.crypto.rub.de/Buch/en/download/problems_only/problems_chaptr_7.pdf)
  + [Chapter 8](http://wiki.crypto.rub.de/Buch/en/download/problems_only/problems_chaptr_8.pdf)
  + [Chapter 9](http://wiki.crypto.rub.de/Buch/en/download/problems_only/problems_chaptr_9.pdf)
  + [Chapter 10](http://wiki.crypto.rub.de/Buch/en/download/problems_only/problems_chaptr_10.pdf)
  + [Chapter 11](http://wiki.crypto.rub.de/Buch/en/download/problems_only/problems_chaptr_11.pdf)
  + [Chapter 12](http://wiki.crypto.rub.de/Buch/en/download/problems_only/problems_chaptr_12.pdf)
  + [Chapter 13](http://wiki.crypto.rub.de/Buch/en/download/problems_only/problems_chaptr_13.pdf)
+ [Solutions for Odd-Numbered Questions](http://wiki.crypto.rub.de/Buch/en/download/Understanding_Cryptography_Odd_Solutions.pdf)

## Introduction

During my self-study on the topic of cryptography, I've found that the textbook "[Understanding Cryptography](http://www.crypto-textbook.com/)" by Christof Paar and Jan Pelzl, and the accompanying [YouTube lectures](https://www.youtube.com/channel/UC1usFRN4LCMcfIV7UjHNuQg/videos), are the most accessible introductory material I have found. The book contains a great many exercises related to the material.

There is a solution manual freely available from the website called [Solutions for Odd-Numbered Questions](http://wiki.crypto.rub.de/Buch/en/download/Understanding_Cryptography_Odd_Solutions.pdf), however the even numbered questions are unavailable. I have contacted the authors, but licensing restrictions prevent them providing the full manual to anyone except instructors in educational institutions. It does not appear that anyone has leaked the manual to the internet either.

As such, I have decided to create a comprehensive solution set to all problems in the book.

## List of Questions and Answers

<ul>
  <li>Chapter 1 - Introduction to Cryptography and Data Security</li>
  <ul>
    {% for exercise in site.understanding_cryptography %}
        {% if exercise.category == 'understanding-crypto' and exercise.chapter == 1 %}
          <li>Ex {{ exercise.chapter }}.{{ exercise.exercise }} - <a href="{{ exercise.url }}">{{ exercise.description }}</a></li>
        {% endif %}
    {% endfor %}
  </ul>
  <li>Chapter 2 - Stream Ciphers</li>
  <ul>
    {% for exercise in site.understanding_cryptography %}
        {% if exercise.category == 'understanding-crypto' and exercise.chapter == 2 %}
          <li>Ex {{ exercise.chapter }}.{{ exercise.exercise }} - <a href="{{ exercise.url }}">{{ exercise.description }}</a></li>
        {% endif %}
    {% endfor %}
  </ul>
  <li>Chapter 3 - The Data Encryption Standard (DES) and Alternatives</li>
  <ul>
    {% for exercise in site.understanding_cryptography %}
        {% if exercise.category == 'understanding-crypto' and exercise.chapter == 3 %}
          <li>Ex {{ exercise.chapter }}.{{ exercise.exercise }} - <a href="{{ exercise.url }}">{{ exercise.description }}</a></li>
        {% endif %}
    {% endfor %}
  </ul>
  <li>Chapter 4 - The Advanced Encryption Standard (AES)</li>
  <ul>
    {% for exercise in site.understanding_cryptography %}
        {% if exercise.category == 'understanding-crypto' and exercise.chapter == 4 %}
          <li>Ex {{ exercise.chapter }}.{{ exercise.exercise }} - <a href="{{ exercise.url }}">{{ exercise.description }}</a></li>
        {% endif %}
    {% endfor %}
  </ul>
  <li>Chapter 5 - More about Block Ciphers</li>
  <ul>
    {% for exercise in site.understanding_cryptography %}
        {% if exercise.category == 'understanding-crypto' and exercise.chapter == 5 %}
          <li>Ex {{ exercise.chapter }}.{{ exercise.exercise }} - <a href="{{ exercise.url }}">{{ exercise.description }}</a></li>
        {% endif %}
    {% endfor %}
  </ul>
</ul>
