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



<br>

{% include lectures/vision.html %}