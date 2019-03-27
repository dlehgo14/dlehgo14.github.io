---
layout: post
title:  "[Lecture] Automata Theory_3"
image: ''
date:   2019-03-26 16:28:13
tags:
- Automata Theory
description: 'Automata Theory'
categories:
- Lectures


---

This post is based on the lecture "Automata theory" of professor Han Yo Sub from Yonsei univ.



------

`03.26 Tuesday`

## Regular expression and FAs

Here is the prove of the claim that given regular language E, the Thompson automata A for E satisfies L(E) = L(A). The proof used induction.

Let **OP(_E_)** the number of operators in E.
Basis: when **OP(_E_)** = 0, E is ∅, λ or σ ∈ Σ. And their Thompson automata is same with themselves. 
Induction: When L(E) = L(A) for every E of **OP(_E_)**  <= k (k >= 0), L(E') = L(A') for every E of **OP(_E_)** = k+1.

This induction is proved by checking all the possible operators (3: +, concatenate, union)

Opposite is also true: which is converting FA to RE. This can be proved by **an expression automaton**.

<br>

## Expression Automaton (EA)

EA has only one **s** (with no in-transition) and one **f** (with no out-transition, and different with s). And its transition is expressed in RE. By **state elimination**, it is possible to convert FAs to EAs.

![K-002](/assets/img/automata/K-002.png)

<br>

{% include lectures/automata.html %}

