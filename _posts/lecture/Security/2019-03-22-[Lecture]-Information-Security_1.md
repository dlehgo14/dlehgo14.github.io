---
layout: post
title:  "[Lecture] Information Security_1"
image: ''
date:   2019-03-22 16:56:19
tags:
- Computer Vision
description: 'Computer Vision'
categories:
- Lectures

---

This post is based on the lecture "Information security" of professor Taekyoung Kwon from Yonsei univ.

<br>

------

`03.06 Wednesday`

## Information security

Any parts of computing system can be the target of crime.

> hardware, software, storage media, data, and even people

There are 3 basic terms. **Vulnerability** is the weakness of security system. **Threat** is the potential danger. **Risk** is the possibility for damage.

There are 4 kinds of threats. **Interruption, Interception, Modification,** and **Fabrication**.

Then what is the purpose of security? It is **CIA**. **Confidentiality** (viewed only by authorized), **Integrity** (modified only by authorized), and **Availability** (used always). 

 Need to think like an adversary. **Legacy systems** which means old systems can be a security hole. Intruder always finds **easiest penetration**. And security can be no stronger than **weakest link** such as open ports. 

**Preventing** the attack is the best way. If can't, **deter** the attacker by making the attack harder (although not impossible). If can't, **deflect** it to another target. If even can't do it, **detect** it after being attacked. At least, you can **recover** from damages. These are the *mitigation* of security.

There are 13 fundamental security design principles established by NCAE. 

<br>

## Cryptography

Cryptography is secret writing. Alice want to send a **plain text** to Bob. The text should not modified or viewed by other bad people. For this purpose, Alice encodes the plain text to **cipher text** and only Bob can read it. How can do it?  We need 3 functions. **Gen**, **Enc**, and **Dec**.

**Gen** is the key generation algorithm. it must be **probabilistic** to make the key unpredictable. **Enc** is the encryption algorithm. **Dec** is the decryption algorithm. Enc and Dec is **deterministic**. 

> Enc_k (m) -> c
> Dec_k (c) -> m

There are 2 kinds of crypto-systems. **Symmetric** and **Asymmetric**. Former one is **Enc key==Dec key**, and vice versa. The important one is **Dec key**, but not Enc key or the algorithms. Let's see **Kerckhoffs' principle**.

![Kerckhoffs' principle](https://image.slidesharecdn.com/cryptographyunderengineering1-141119104437-conversion-gate02/95/cryptography-underengineering-21-638.jpg?cb=1416394167)

This is because the algorithm can be easily discovered by others, by **reverse engineering**.

<br>

{% include lectures/security.html %}