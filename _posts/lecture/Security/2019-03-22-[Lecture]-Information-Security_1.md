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

This is because the algorithm can be easily discovered by others, by **reverse engineering**. Besides the algorithm is longer than the key, which makes maintaining secrecy easier. Furthermore, to be used by many people, it is practical to use same algorithm and different keys. Theses reasons reach a conclusion of **open cryptography design **(standardization).

<br>

## Classical ciphers

### Transposition cipher

The Spartans used **Scytale cipher**. You can read about it <a href="https://en.wikipedia.org/wiki/Scytale">here</a>. This cipher is super simple.

> key: 12345
> **H**ENT**E**IDT**L**AEA**P**MRC**M**UAK

For more complexity you can use a longer permuted key.

> key: 4312567
> **T**TNA**A**PTM**T**SUO**A**ODW**C**OIX**K**NLY**P**ETZ

For more complexity, round more!

> key: 4312567
> **T**TNA**A**PTM**T**SUO**A**ODW**C**OIX**K**NLY**P**ETZ
> NSCYAUOPTTWLTMDNAOIEPAXTTOKZ

### Substitution cipher (Caesar cipher)

It is **shift cipher**.

> shift: 3
> I LOVE YOU -> LORYHBRX

However **the keyspace is too small**. In case of english alphabets, there are only possible 25 keys. (the number of alphabet is 26, but 0 can't be the key.) So **brute-force attack** is possible. Then how big the key should be to prevent exhaustive search? It is at least **2^80**. Then what about permutations of alphabet (one-to-one mapping): its possible keys are 26! (approximately 2^88). However this is not safe because of **the frequency of alphabets** of mono-alphabetic substitution. So  **Vigenère cipher** comes out, which is **poly-alphabetic substitution**. 

![Vigenère cipher](https://images.code.org/06858f88ac12997bba73f4f76638a068-image-1443574425185.gif)

However it is unsafe either. Kasiski examination said that certain common words such as "the" will be encrypted by same key letters, leading to repeated groups in the cipher-text.

5-rotor machine has the same security level with Vigenère cipher that has 11,881,376 length  of the key. However it failed too.

![Rotor machine](http://www.merkle.com/cryo/rotor.gif)

In conclusion, the reason that classical ciphers failed is **the frequency of letters**. **Letter frequency** must be **perfectly random**.

<br>

---

`03.13 Wednesday`

## Perfect Secrecy

Is it possible to make perfect secrecy? What is perfect secrecy? If it is same possibility to guess the plain-text, regardless of knowing cipher-text or not, without key, then it is perfect secrecy. 즉, c를 알든 모르든 m을 알아맞출 확률이 같으면 perfect secrecy를 달성했다고 볼 수 있다. To achieve it, **letter frequency should be random**, and **every plain text is equally likely to be a decryption of cipher-text**, and finally **unbreakable without the key.** And if key is random-uniform, then any message has the same probability to encrypt to specific **c**.

> 3 conditions of perfect secrecy: random, ideal, information theoretic

### Vernam cipher (OTP: One-Time-Pad)

XOR is very useful in cryptography because of this property.

|      | Pr[X] | Pr[Y] |
| ---- | ----- | ----- |
| 0    | 1/2   | P0    |
| 1    | 1/2   | P1    |

When Y is the plain-text that has some letter frequency, and X is the same-length key which has random frequency. then Pr[ (X⊕Y) = 1] = 1/2. So random frequency can be achieved. However the key must be used only once because of **self-reduction** property of XOR. *c1 ⊕ c2 == m1 ⊕ m2*. 

> Shannon's idea (1949)
> Cipher-text should reveal no information about plain-text.

Surprisingly, **OTP has perfect secrecy**. However it is too impractical, because the keys have to be at least length of the message, and can be used **only one time**. Furthermore OTP is **malleable**. If someone knows some parts of the message, then can modify the message very easily. How? Just do XOR with replaced messages. 

> k ⊕ m0 = c
> c ⊕ (m0 ⊕ m1) = c'

<br>

## Practical Security

In **indistinguishability experiment**, attacker give 2 messages to the verifier, and verifier throws the coin. And then encrypt one of 2 messages by the result of the coin. And this encrypted message is given to the attacker, and the attacker should guess which message is encrypted. In this experiment, if secrecy is perfect, the probability should be 1/2. (|k| >= |m|). However to be practical, the following is accepted. negl(n) is very small probability. (roughly 1/2^100)

> ∀A: Pr[ PrivKA = 1 ] ≤ 1/2 + negl(n)

![indistinguishability experiment](https://slideplayer.com/slide/7277800/24/images/8/CPA+Indistinguishability+Experiment.jpg)

Negligibility(*f(n)*) is achieved when **there exists an N, for every n>N, f(n)>1/p(n) such that any polynomial p()**. 

<br>

## Security model

### Ciphertext-only attack (COA)

The attacker only know the ciphertext but nothing, and try to determine underlying plaintext.

### Known-plaintext attack (KPA)

The attacker know some of pairs of plaintexts and ciphertexts under the same key. 

### Chosen-plaintext attack (CPA)

The attacker can know the ciphertexts of any plaintexts.

### Chosen-ciphertext attack (CCA)

The attacker even can know the plaintexts of any ciphertexts (but not directly, so it is his task). 

### Adaptive attacks (CCA2)

The cheating is even possible during attempting to hack. **TRG(True Random Generator)** is hard to realize. So use **PRG(Pseudo Random Generator)**. The conditions of random is **uniform distribution** and **independence**. However independence can't be tested, so use string sample. When poly-time observer can't distinguish strings from TRG and PRG, then PRG succeeds. 

<br>

## Stream cipher

Converting OTP to practical. Alice and Bob should get same keys. So **KSG** is needed. It should be **efficient (deterministic)** and **random**. 

{% include lectures/security.html %}