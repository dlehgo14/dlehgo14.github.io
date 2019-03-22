---
layout: post
title:  "[Lecture] Computer Vision_3"
image: ''
date:   2019-03-22 16:13:19
tags:
- Computer Vision
description: 'Computer Vision'
categories:
- Lectures

---

This post is based on the lecture "Computer vision" of professor Kim Seon Ju from Yonsei univ.

<br>

------

`03.21 Thursday`

## Nyquist-Shannon sampling theroem

What is the optimum sampling to avoid aliasing? This is the solution of Nyquist.

![Nyquist frequency](https://i.ytimg.com/vi/v7qjeUFxVwQ/hqdefault.jpg)

When f_sampling < 2f_signal, there occurs overlaps. 

![overlap](http://dsplab.eng.fiu.edu/realtimedsp/images/Tutorial/Figure4.gif)

However, oversampling is not always possible. In other words, sometimes it is not possible to avoid under-sampling because of hardware limitation or other reasons. So here is the method to anti-aliasing.

![anti-aliasing](https://upload.wikimedia.org/wikipedia/commons/thumb/d/d5/AliasedSpectrum.png/400px-AliasedSpectrum.png)

<br>

## Resotration

Restoration is the process restoring **degraded images**. It is possible by undoing the degradation process. So restoration is distinguished from **enhancement** which means making image look good for perception.

### Degradation model

Convolution with degradation function _h(x,y)_, and add noise. Noise arises during image acquisition and transmission. 

### noise

These are types of noises. Gaussian, Rayleigh, Erlang, Exponential, Uniform, Impulse. Impulse noise is also known as "salt and pepper noise" because it seems like salt and pepper. Anyway, removing noise is possible by blurring. Let's see 3 different filters.

1. Spatial filtering

   It is filter we have seen until now. The most famous spatial filter is *simple average blur kernel* which is called *box filter*.

2. Median filtering

   It removes noise very well **without smoothing effects**. Particularly good at salt and pepper noise. The following image makes your understanding easier.

   ![median filter](https://www.researchgate.net/profile/Benjamin_Weyori/publication/280925268/figure/fig1/AS:391477344653326@1470346880378/A-graphical-depiction-of-the-median-filter-operation.png)

3. Bilateral filtering

   It is designed for preserving edges. 

   ![Bilateral filter](https://t1.daumcdn.net/cfile/tistory/272E805058313BE103)

   ![example of Bilateral filtering](https://www.researchgate.net/profile/Fatih_Porikli/publication/221361504/figure/fig4/AS:305607526633475@1449873920200/PSNR-accuracy-of-the-presented-O-1-bilateral-filter-with-box-spatial-given-in-Eq-4.png)

<br>

{% include lectures/vision.html %}