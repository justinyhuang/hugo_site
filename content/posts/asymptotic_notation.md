---
title: Asymptotic Notation
date: 2010-05-23T21:39:00
libraries:
- mathjax
---

One of my reading projects is to work on the [Introduction to Algorithm](http://mitpress.mit.edu/catalog/item/default.asp?ttype=2&tid=8569).

After a few chapters I realized that I might have already forgot all little pieces of mathematics learned in college.

<!--more-->

This note shall keep me aware of the asymptotic notations frequently used in the algorithms:

$$f(n)=\Theta(g(n)) \Leftrightarrow c_{1}g(n) \le f(n) \le c_{2}g(n)$$




$$f(n)=O(g(n)) \Leftrightarrow 0 \le f(n) \le cg(n)$$




$$f(n)=\Omega(g(n)) \Leftrightarrow 0 \le cg(n) \le f(n)$$




$$f(n)=o(g(n)) \Leftrightarrow \lim \limits_{n \to \infty} \frac{f(n)}{g(n)}=0$$




$$f(n)=\omega(g(n)) \Leftrightarrow \lim \limits_{n \to \infty}\frac{f(n)}{g(n)}=\infty$$


To make the notations easier to remember/understand, the book gives the following loose analogies:


$$f(n)=\Theta(g(n)) \approx f(n) = g(n)$$




$$f(n)=O(g(n)) \approx f(n) \le g(n)$$




$$f(n)=\Omega(g(n)) \approx f(n) \ge g(n)$$




$$f(n)=o(g(n)) \approx f(n) < g(n)$$




$$f(n)=\omega(g(n)) \approx f(n) > g(n)$$
