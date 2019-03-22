---
layout: post
title:  "[Lecture] Computer Vision_2"
image: ''
date:   2019-03-19 18:34:19
tags:
- Computer Vision
description: 'Computer Vision'
categories:
- Lectures
---

This post is based on the lecture "Computer vision" of professor Kim Seon Ju from Yonsei univ.

<br>

---

`03.19 Tuesday`

## Donwsampling

The most simple way of down-sampling is to delete a pixel per 2 pixels. (When down-sample to half size) However this simple way cause **aliasing**. 

!["aliasing when sampling"](https://www.dataforth.com/g/aliasing-examples.png)

By **under-sampling**, information is lost, and we could confuse with lower frequency signal. Note that we could always confuse the signal with one of higher frequency. Following image is an example of **moire effect** caused by aliasing.

!["moire effect"](https://i.ytimg.com/vi/jXEgnRWRJfg/hqdefault.jpg)

### Anti-aliasing

Very simple way to prevent aliasing is oversampling. However it is up to the hardware, and expensive. So the valid approach is **smoothing the signal**. Then there are 2 questions.

> 1. How much smoothing do I need to avoid aliasing?
> 2.  How many samples do I need to avoid aliasing?

<br>

## Fourier

He says,

> Any univariate function can be rewritten as a weighted sum of sines and cosines of different frequencies.

In other words, every signal can be expressed as sum of **A sin(Ωx + ∅)**.

> A : amplitude
> Ω : angular frequency
> ∅ : phase

### Visualizing the frequency spectrum

When the signal is **1D**, x axes is Ω, and y axes is A. When the signal is **2D**, x axes is Ω of width signal, and y axes is Ω of height signal. and A is represented as color. For 1 simple signal, 3 dots are written on the visualization. left and right dot is symmetric one, and middle dot is the signal average.

### Fourier transform

In the **rectangular coordinates**, complex numbers can be represented as **_R_+j _I_**. R is "real", and I is "imaginary".

![complex numbers](https://www.electronics-tutorials.ws/wp-content/uploads/2013/06/acp54.gif)

https://www.electronics-tutorials.ws/accircuits/complex-numbers.html

It can be re-parameterized to **polar coordinates**. **r(cos _Θ_ + j sin _Θ_)**.

!["Polar transform"](https://s3-us-west-2.amazonaws.com/courses-images-archive-read-only/wp-content/uploads/sites/923/2015/04/25181244/CNX_Precalc_Figure_08_03_0112.jpg)

Then **Θ = tan-1(I/R)**, and **r = root(R^2 + I^2)**. So the following equation is true.

> Euler’s formula
> r(cos _Θ_ + j sin _Θ_) = re^(j _Θ_)

detail explanation: <a href="https://homepages.inf.ed.ac.uk/rbf/HIPR2/fourier.htm">here</a>

### Fourier transforms of natural images

Images divide to **amplitude** and **phase** by Fourier transform. Phase has the detail information of the images.

<br>

## The convolution theorem

The Fourier transform of convolution of 2 functions (images) is same with the product of Fourier transforms of 2 functions.

> _F_{g*h} = _F_{g}_F_{h}
> _F-1_{gh} = _F-1_{g} * _F-1_{h}

<br>

{% include lectures/vision.html %}