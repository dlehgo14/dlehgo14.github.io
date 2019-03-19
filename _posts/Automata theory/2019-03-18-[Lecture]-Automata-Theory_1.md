---
layout: post
title:  "[Lecture] Automata Theory_1"
image: ''
date:   2019-03-18 10:56:12
tags:
- Automata Theory
description: 'Automata Theory'
categories:
- Lectures
---

This post is based on the lecture "Automata theory" of professor Han Yo Sub from Yonsei univ.



---

`03.12 Tuesday`

## What can be learned from this lecture?

This course deals with the theory of computation. In other words, this course will treat the computing machines, their capabilities and limitations.

There are 2 kinds of solvable problems.

1. solvable problems **in principle**.
2. solvable problems **in practice**: that is related with NP-hardness.

This course is related with 1, which is about **the impossible problems**. When the time is resource, there are 3 types of problems. Undecidable. Intractable. Tractable. And once again, this course will treat undecidable problems.

<br>

## Is blue-screen solvable?

Blue-screen can cause huge damages to some systems, such as power plants, baking systems, cars, and so on. So this problem should be solved, and many strives had been committed to solve it. However, no one succeeded it because **this problem is unsolvable in principle**.

<br>

## Alphabet 

**Finite** and **non-empty** set of symbols. It is denoted by ∑. 

<br>

## String

**Finite** sequence of symbols from some alphabet. It is also called "Word". The empty string is denoted by λ. Set of strings of ∑ is denoted by ∑*.

Strings can be concatenated by the operation "·", and it can be omitted. 

<br>

## Languages

Set of strings. L ⊆ Σ*. L can be infinite, but all strings in L are some finite set of symbols ∑. ∅ is the empty language, and it is over any alphabet. Note that {λ} is not same with ∅.

<br>

---

`10.14 Thursday`

## FAs

**Finite-state Automata** is the way designing and checking the behavior of digital circuits. It can verify all systems which have a finite number of distinct states. Automata means "machines", and finite-state means the automata has the finite number of states.

<br>

## DFAs

**Deterministic Finite-state Automata** has one and only one possible move at every state. DFA can be specified by a tuple that has 5 elements.

>  (Q, Σ, δ, s, F)

Q is finite and nonempty set of states. Σ is a set of input alphabet. δ is a transition function Q × Σ → Q. s is the start state. F is a set of final states.

Given a sequence w, after all the input symbols in w be computed, if the current state is one of the final states, w is accepted, otherwise it is rejected.

A **configuration** of DFA is expressed by **(current state, remainder of input strings)**, which is an element of Q × Σ*

We say *w* is accepted by A, if and only if

![image of computations](/assets/img/automata/K-001.png "computations")

The language *L(A)* of A is a set of all accepted strings.

*L(A)* = {*w* | *w* is accepted by A}.

<br>

## NFAs

**Non-deterministic Finite-state Automata** may sometimes have more than one possible move. So the next state is only partially determined by the current state and input symbol. In other words, **δ is a set** in NFAs.

> δ ⊆ Q × Σ × Q

All NFAs can be converted to DFAs by **subset construction**. So NFAs have the same expressive power as DFAs.

<br>

{% include automata.html %}

