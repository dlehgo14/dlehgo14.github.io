---
layout: post
title:  "[Lecture] Automata Theory_2"
image: ''
date:   2019-03-25 17:45:32
tags:
- Automata Theory
description: 'Automata Theory'
categories:
- Lectures

---

This post is based on the lecture "Automata theory" of professor Han Yo Sub from Yonsei univ.



------

`03.21 Thursday`

## Regular expressions

Its definition is **languages that can be recognized by FAs**. The most famous non-RL is below

> w = {consecutive 'a' followed by same number of 'b'}

The convention of automata is that it has only one start state, and one final state. Let's see inductive definition of RE.

> 1. ∅, λ, σ ∈ Σ 
> 2.  (α · β),(α + β) and (α∗) 

And let's see the precedence of operators.

> {}, () --> * --> concat --> + (union)

Let's see some algebraic laws.

> Identities 
> ∅ + E = E + ∅ = E.
> λE = Eλ = E.
>
> Annihilator
> ∅E = E∅ = ∅
>
> Idempotent
> E + E = E

<br>

##  Thompson Construction

The convention of FA construction from RL. 

<br>

{% include lectures/automata.html %}

