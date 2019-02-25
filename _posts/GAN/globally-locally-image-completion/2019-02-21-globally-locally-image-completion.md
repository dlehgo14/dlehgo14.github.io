---
layout: post
title:  "Globally and Locally Consistent Image Completion 논문 Review"
image: ''
date:   2019-02-21 15:00:00
tags:
- GAN, image completion, CNN
description: 'Image competion'
categories:
- GAN
---

<br/><br/>
번역 + 요약 + 나의 이해를 종합해서 써놓은 글입니다. 질문 + 피드백 환영합니다 :)

## Abstract

<img src="{{ "/assets/img/globally-locally-image-completion/5.png"}}" alt="">

<p class="music-read"><a href="https://dl.acm.org/citation.cfm?id=3073659">논문 URL</a></p>

- image completion, inpainting<br/>
임의적으로 부분부분이 지워져있는 이미지를 복원해내는 문제이다.

- 2개의 discriminator 사용<br/>
각각 locally, globally consistent를 유지하게 한다.
덕분에 얼굴과 같은 highly specific structure의 복원도 가능하다.

## Introduction
 지워진 이미지를 복원하기 위해서 textured pattern만이 중요한 것이 아니다.<br/>
scene과 objects의 anatomy를 이해하는 것도 중요하다. <br/>
가령 다음의 이미지처럼 눈이 있어야 할 부분을 살로 채우면 안될 것이다.

<img src="{{ "/assets/img/globally-locally-image-completion/1.png"}}" alt="">

 이 work는 Context Encoder (Pathak et al. 2016) 에 기반한다. <br/>
CE는 CNN을 adversarial loss를 이용해 훈련한 구조이다. 이미지의 feature learning을 위해 도입된 아이디어이지만, **임의의 inpainting mask**와 **높은 해상도의 이미지**를 어떻게 처리할 것인지는 설명하지 못하였다. 이 논문에서는 랜덤한 영역이 지워져있는 고해상도 이미지를 complete 하는 것을 보여준다.

> 전체 네트워크는 3가지 네트워크로 구성되어 있다.

1. Completion network <br/>
fully conv net 으로 되어있다.

3. Global context discriminator<br/>
full image를 input으로 받는다.

3. Local context discriminator<br/>
채워야하는 small region을 input으로 받는다.

## Approach
 기본적으로 deep CNN 기반이다. 전체적인 네트워크 구성은 다음 그림과 같다.

<img src="{{ "/assets/img/globally-locally-image-completion/2.png"}}" alt="">

### Convolutional Neural Networks
 CNN (Fukushima 1988; LeCun et al. 1989) 에 기반하고 있다. ~~CNN이 1980년대 기원이라니..~~<br/>
표준 CNN 대신에 **dilated CNN** (Yu and Koltun 2016) 을 사용하였다. dilated CNN을 사용함으로써 더 적은 parameters로 더 넓은 영역을 cover할 수 있다는 논리이다. 

### Completion network
 fully-CNN으로 이루어져 있다. 
 
 인풋으로 2가지를 넣어준다. **RGB 이미지**와 **binary channel mask**. 마스크는 지워져있는 부분을 나타낸 것이다. *(1이면 채워야하는 픽셀)* 

 아웃풋은 **RGB 이미지**이다.

 구멍이 아닌 부분은 Input image를 그대로 restored한다. memory usage를 줄이기 위해, input 이미지의 resolution을 줄여서 처리한 후 deconv (Long et al. 2015) 하여 원래 resolution (image size)으로 복원시킨다. *(흔히 쓰이는 **decoder-encoder** 방식)*

 기존의 다른 모델들은 resolution을 위해 많은 pooling layer을 거치는데 반해, 이 모델은 단 2번만 pooling layer을 거친다. (정확히는 stride 2 conv) 이것은 non-blurred texture를 generate하는데 중요하다.

 resolution 이 작은 이미지를 처리하는 mid-layer에서 dilated-conv가 사용되는데, 이로써 적은 parameters로 넓은 region을 커버하여 효과적으로 large area를 "see" 할 수 있다.

 논문에 네트워크가 자세히 기술되어 있는데, 실제로 커널의 크기는 3x3 ~ 5x5 로 크지 않지만, mid-layer에서 dilation을 2, 4, 8, 16으로 상당히 크게 잡는 것을 알 수 있다.

### Context discriminators
 Local과 global context discriminator는 각각 이미지가 real인지 네트워크에 의해 complete된 이미지인지 구분한다. 역시 CNN으로 구성되어 있고, 작은 feature vector로 압축해나간다. 

 5번의 stride 2 CNN을 거친 후에, 마지막에 FC 레이어를 거쳐 local, global context discriminator가 각각 1024-dimensional 아웃풋을 가지게 한다. 그 후 두 vector를 concat하여 2048-dimensional output을 가진 vector을 생성하고, FC레이어를 한 번 더 거쳐서 [0~1] 사이의 값을 가지는 vector을 아웃풋으로 내놓는다.

### Training
네트워크가 image를 complete 하는 것을 C(x, Mc) 라고 하자. x는 input image를 나타낸다. Mc 는 binary 마스크를 나타내고, 1이면 채워야하는 곳, 0이면 채우지 않아도 되는 곳을 의미한다. 
 preprocessing으로써, 이미지에 채워야하는 부분을 training set의 mean pixel value로 채운다. 
마찬가지로 D(x, Md) 는 discriminator의 output을 나타낸다.
Loss는 **MSE**와 **GAN loss**를 동시에 사용하였다.
> MSE<br/>
L(x, Mc ) = || Mc ⊙ (C(x, Mc ) − x) || ^ 2

> GAN loss<br/>
min C max D E[ logD(x, Md ) + log(1 − D(C(x, Mc ), Mc ) ]

 아무래도 GAN이다 보니, 안정적인 training이 쉽지는 않았다. 여타 GAN과는 달리 noise를 통해서 이미지를 생성해내는 task는 아니기 때문에, 그나마 용이했다. 네트워크에서 나온 output에 간단한 post-processing을 하였다. 반칙 아닌가요?

## Results
8,097,967 장의 training images를 사용하였다. Places2 dataset을 사용하였다. (Zhou et al. 2016) 

α = 0.0004 으로 설정 (learning rate를 말하는 것인지?)하였다. 

completion network를 먼저 96장의 batch size로 90,000 iters 훈련

discriminator를 10,000 iters 훈련 후에,

jointly 500,000 more iters 훈련하였다.

K80 GPU 4개로 무려 2달간 훈련... 

## Limitations
Hole이 너무 크면 채울 수 없다. 또는 Hole이 너무 길거나 한 방향으로 치우쳐 있어도 채울 수 없다.
<img src="{{ "/assets/img/globally-locally-image-completion/3.png"}}" alt="">

## 총평
사람 얼굴을 복원해 낸 최초의 network인 것 같다.

 2018년도에 나온 GAN을 사용하지 않은 partial conv와의 비교하면 어떨까

 다음과 같이 얼굴이 있어야 할 위치에 얼굴로 채우지 않는 것은 global context가 잘 반영이 안되어 있는 것 같다.
<img src="{{ "/assets/img/globally-locally-image-completion/4.png"}}" alt="">

