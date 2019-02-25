---
layout: post
title:  "Batch Normalization 논문 Review"
image: ''
date:   2019-02-22 19:33:00
tags:
- normalization, batch normalization, CNN, BN
description: 'batch normalization'
categories:
- Normalization
---

<br/><br/>
번역 + 요약 + 이해를 종합해서 써놓은 글입니다. 질문 + 피드백 환영합니다 :)

## Abstract


<p class="music-read"><a href="https://arxiv.org/abs/1502.03167">논문 URL</a></p>
너무나도 유명한 훈련 스킬 Batch Normalization. 처음 소개된 것은 인용 수가 8000회가 넘는 2015년도 3월, Sergey loffe와 Christian Szegedy의 논문 [Batch Normalization: Accelerating Deep Network Training by Reducing Internal Covariate Shift] 에서이다.
<br/><br/>

앤드류 엉 박사님의 <a href="https://www.youtube.com/watch?v=nUUqwaxLnWs">유투브 영상</a>에서 개념이 이해하기 쉽게 잘 설명되어 있으니 참고하면 좋을 것 같다.
<br/><br/>

네트워크에서 수많은 신경망을 거치면서, 깊은 layer일수록 input의 variance가 커지게 된다. 이로 인해 learning rate를 줄이고, 초기 parameter 값의 선정에 심혈을 기울여야만 한다. 또한 non-linerarity 특성이 강한 모델은 더욱 훈련이 어렵게 된다. 이러한 현상을 **internal covariate shift**라고 한다. 
<br/><br/>

이러한 문제를 해결하기 위해, **mini-batch** 별로 normalize를 진행하였다. 그럼으로써 높은 learning rate로 훈련이 가능하고, 초기 parameters 선정에 덜 신경을 써도 된다. 또한 drop-out을 대신하여 regularizer로서 역할을 하기도 한다. 종합하자면, 빠르고 손쉬운 훈련을 가능하게 하고, over-fitting 까지 예방할 수 있다는 말이다. 당시 image-net classification을 14배나 적은 training step으로 가장 높은 정확도를 달성하였다. 
<br/><br/>

## Introduction

데이터 하나 씩 normalize하지 않고, mini-batch 별로 normalize 함으로써 얻을 수 있는 이점은 다음 2가지이다.

1. mini-batch의 gradient는 전체 data set의 gradient를 더 잘 반영한다.<br/>
2. 각각의 데이터마다 계산하는 것보다 더욱 빠르게 병렬처리 가능하다.

<br/>
SGD (Stochastic Gradient Descent)는 용이하지만 초기 parameter 선정에 심혈을 요한다. n 번째 레이어의 input은 그 이전의 모든 레이어의 parameters들의 영향을 받기 때문이다. 그래서 앞선 레이어의 작은 변화가 나중 레이어의 인풋에 큰 변화로 amplify 되는 현상이 발생한다. 즉 sub-network의 input의 distribution이 fix되어 있지 않고 매 mini-batch마다 다르게 된다. 
<br/><br/>
activation function 중 sigmoid를 떠올려보자. input (Wx + b) 의 절대값이 작은 부분을 제외하고는 gradient가 작아 학습이 더디게 진행된다. 하지만 x는 W, b 뿐만 아니라 이전 레이어의 모든 parameters의 영향을 받는다. 때문에 네트워크의 depth가 깊어질수록 x는 통제하기 어려워지고, 많은 x가 saturated regime of non-linearity (activation function에서 gradient가 작은 부분) 에 놓이게 한다.<br/>
<img src="https://i.stack.imgur.com/voC4J.png"/><br/>

*이미지 출처: https://datascience.stackexchange.com/questions/16911/how-does-sigmoid-saturate-with-large-weights*
<br/><br/>

이와 같은 saturation problem과 vanishing gradients는 **ReLu, hyper-parameter의 심혈을 기울인 초기설정, 작은 learning rate** 로 해결이 가능하지만, 애초에 모든 레이어의 input의 distribution을 보장할 수 있다면 문제를 더 근본적으로 해결할 수 있을 것이다.
<br/>

## Covariate shift 줄이기
<br/>

**internal covariate shift** : 매 레이어마다 이전 parameters에 의해 distribution이 크게 변화하는 것<br/>
이라고 정의했다. 
<br/>
input을 whitening하면 빠르게 결과가 converge 한다. whitening 한다는 것은 input이 평균이 0이고, 분산이 일정하고, 서로 decorrelate하다는 것을 말한다.
<br/>
그럼 간단하게 매 layer마다 normalize 해주면 어떨까? 그렇게 되면 훈련 effect가 줄고, bias가 상쇄된다. ( var(X) == var(X+b) ). 훈련 effect가 주는 이유는 매 레이어의 output의 스케일이 반영되지 않기 때문이다. (모든 output이 평균 0의 일정한 std로 고정될 것이다.)

## Normalization via Mini-batch statistics
<br/>

위에서 언급한 스케일 문제를 해결하기 위해 **scale, shift value**를 추가하였다.
<br/>
<img src="{{ "/assets/img/BN/1.png"}}" alt="">
x hat은 mean 0, std 1로 normalize한 값이고, scale vector로 **gamma**, shift vector로 **beta**를 추가하였다. *gamma가 x hat의 std이고, beta가 x hat의 mean 이라면 y는 x와 동일하게 된다.*
<br/>

 ### Training 시와 Inference (test) 시의 차이
 <br/>
 training 시에는 mini-batch의 평균과 분산을 이용하지만, Inference 시에는 population (전체 데이터 셋)의 평균과 분산을 이용한다. 모분산은 mini-batch의 분산의 평균에 m/(m-1) 을 곱하여 구한다. (m 은 mini-batch의 data 수) Inference 시에는 평균과 분산이 일정하기 때문에, 단순한 linear 변환으로 해석할 수 있다. 
 <br/>
 <br/>

>  z = g (BN ( Wu ) )

<br/>
g는 ReLu나 sigmoid와 같은 non-linearity activation function이다. bias를 생략한 것은 BN에서 bias가 상쇄되기 때문이다.
<br/>

### Higher learning rate

기존의 deep networks에서는 앞 쪽 parameter의 작은 변화가 나중 레이어에 큰 영향을 미치기 때문에 learning rate를 제한할 수 밖에 없었다. BN을 사용함으로써 이것을 방지할 수 있다.

### Regularization

mini-batch 별로 반영을 함으로써, 특정 training set에 overfitting 하는 것을 예방할 수 있다.