---
layout: post
title:  "[Lecture] Computer Vision_1"
image: ''
date:   2019-03-19 10:48:42
tags:
- Computer Vision
description: 'Computer Vision'
categories:
- Lectures
---

This post is based on the lecture "Computer vision" of professor Kim Seon Ju from Yonsei univ.

<br>

---

`03.12 Tuesday`

## Pinhole

Why is there no image on a piece of white paper? That is because all the scattered light from everywhere comes to the paper. To reflect one selected image, it is needed to focus on some selected rays. **Pinhole** is one of the way.

![pinhole](http://www.karlwinkler.com/pinhole.jpg "pinhole image")

If pinhole size is big, the image get blurring. If pinhole size is small, we can get a sharp image, but it is hard to take enough light. So more exposure time is required, and it may cause another blurring. So **lens** are introduced

<br>

## Focal length

Focal length is **the distance from pinhole to film (sensor)**. Longer the focal length, bigger the size of object image, but less of the scene is caught. 

![focal length](https://i.stack.imgur.com/28aXU.png "focal length image")

<br>

## Lens

Sharper image, and less exposure time than pinhole. Rays passing through the center are not deviated, and other rays converge to one point. *(All parallel rays converge to one point at the focal length f)* There is specific distance at which objects are **in focus**. Other objects which are not in focus cause **circle of confusion** in the image.

![circle of confusion](http://www.theimage.com/digitalphoto2/stillimages/circleofconfusion.jpg "circle of confusions image")

<br>

## Exposure

Get the right amount of light to film (sensor). There are 3 factors that influence it. **Shutter speed** and **aperture**, and **sensitivity**.

### Shutter speed

Shutter speed has a linear effect on exposure. If shutter speed is too slow, motion blur may be caused. Shutter speed is set according to other status relatively.

> 1/30, 1/60, 1/125, 1/250, 1/500, 1/1000

### Aperture

Aperture is the diameter of the lens opening. Bigger the f number, smaller the diameter. When the **aperture is big**, **small change of depth** cause **huge change** in the image. 

!["aperture"](https://i2.wp.com/digital-photography-school.com/wp-content/uploads/2006/08/2000px-Aperture_diagram.svg_.png?resize=900%2C358&ssl=1 "aperture")

> LFC (Light Field Camera): Can focus on any point after taking a picture.

<br>

### Sensitivity (ISO)

ISO has a linear effect on exposure. (200 ISO needs half the light as 100 ISO) Trade sensitivity for grain (film) or noise (digital). Higher ISO means much more amplification, and also the noise(grain) is amplified. 

#### White balance

To make something white in real seen white in the photo. Focus the camera something white to adjust WB. 

#### Vignetting

Vignetting is the flaws which are caused by the lens at the periphery of the image. 

<br>

---

## Image

Image is **a 3D tensor** which consists of (height, width, color). Gray-scale image is a 2D function. So may types of image transformations can be done. **Filtering** is changing the pixel values, while **warping** is changing pixel locations. In other words, Filtering changes *range* of image function, and warping changes *domain* of image functions.

!["image filtering and warping"](http://ai.stanford.edu/~syyeung/cvweb/Pictures1/processing.png "image filtering and warping")

<br>

## Filtering

There are 2 kinds of filtering.

### Point processing

g(x,y) = T[ f(x,y) ]

s = T (r)

Image histogram can represents **grey level distribution**. Histogram P(rk) can be represented as following expression.

p(rk)= nk/N

### Linear shift-invariant image filtering (Neighborhood operation)

Use kernel. One of kernel is the box filter, also known as 2D rect filter or square mean filter.

!["box filter"](https://1.bp.blogspot.com/-Ue37dDpyfyE/WpYEF1GV-sI/AAAAAAAAI6M/BS_b7KwG1aIzNgFnwpGMHntBEnCzU-xQQCLcBGAs/s400/3x3%2BNormalized%2Bbox%2Bfilter.png)

<br>

## Convolution

Convolution for 2D discrete signals is defined as follow. h is filter, and x is input image.

!["proof of separable convolution 2d"](http://www.songho.ca/dsp/convolution/files/conv2dsep_eq04.png "separable conv 2d")

Separable filter is the filter that can be written as the product of a column and a row. It makes the computation faster.

> There are a M x M image, and N x N kernel.
> O(M^2 x N^2) --> O(2 x M^2 x N)

<br>

##  Gaussian filter

This is the expression of Gaussian filter. It blurs the images.

!["Gaussian filter"](https://patentimages.storage.googleapis.com/WO2010053874A1/imgf000014_0001.png)

Theoretically infinite, but practically truncated to some maximum distance: usually at **2-3σ**.

!["Gaussian filter graph"](https://homepages.inf.ed.ac.uk/rbf/HIPR2/figs/gauss1.gif)

Gaussian filter makes blurred image smoother than box filter.

<br>

## Derivative filter

To sharpen the image, detect the edges and amplify the discontinuity. Then how to detect the edges? By image gradients.

!["sharpened image"](https://enviragallery.com/wp-content/uploads/2016/02/smart-sharpen-before-after.jpg)

Derivatives of edges are large. 1D derivative filters looks like [1,0,-1]. It is similar to differential equation. **Sobel** filter is the complex of derivative and blur filter.

!["sobel filter"](http://media5.datahacker.rs/2018/11/conv_3_filtri_sobel_sch.png)

Upper sobel filter is **horizontal sobel filter**. Then you can imagine how the vertical sobel filter looks like, and how the both sobel filter transform the images. By convolving with horizontal sobel filter, you can get **δf/δx**. In the same manner, you can get **δf/δy** by convolving vertical sobel filter. Then following equation is valid.

> gradient of x = [ (δf/δy) , (δf/δx) ]
>
> amplitude of x = || gradient of x || = square root of { (δf/δy)^2 + (δf/δx)^2 }

<br>

## DoG filter

Do Gaussian filter first, and do Derivative filter. This is because eliminating subtle vibrate in the image is crucial. Laplace filter is filter that derivate twice.

!["LoG: Laplace of Gaussian"](https://homepages.inf.ed.ac.uk/rbf/HIPR2/figs/logcont.gif)

<br>

{% include lectures/vision.html %}

