---
layout: post
title:  "Deep Portrait Image Completion and Extrapolation 논문 review"
image: ''
date:   2019-02-27 17:33:00
tags:
- image completion
description: 'machine learning'
categories:
- image completion
---

<p class="music-read"><a href="https://arxiv.org/abs/1808.07757">논문 URL</a></p>

## Abstract

2018년 8월에 발표된 논문으로, image completion과 extrapolation을 수행한 논문입니다. portrait은 초상화를 뜻하는데, 여기서는 몸까지 나온 인물사진을 주로 다룹니다. 사람을 찍은 이미지에서, 신체 일부분이 지워져있거나(image completion), 이미지에 나타나지 않은 바깥부분 신체를 채우는 것(image extrapolation)을 다룬 논문입니다.
다른 관련 연구들에서는 사람 몸을 채우는 것에 실패한다는 것을 지적하며 Abstract를 시작합니다. 진짜 그런지 image completion을 다룬 인용수가 높은 논문 2가지를 살펴보았습니다. <br>
<br>
<a href="http://openaccess.thecvf.com/content_cvpr_2018/html/Yu_Generative_Image_Inpainting_CVPR_2018_paper.html">"Generative Image Inpainting With Contextual Attention"</a> 은 cvpr 2018에 발표된 논문으로 발표된지 1년이 지나지 않았음에도 인용수가 50회에 달합니다. 
<br>
<img src="/assets/img/portrait-image/K-001.png">
<br>
사람 얼굴이나 자연 경관 등의 이미지를 복원하지만, 사람 몸을 완성시키는 결과는 없었습니다.
<br><br>
마찬가지로 nvidia에서 18년도에 발표한 <a href="http://openaccess.thecvf.com/content_ECCV_2018/html/Guilin_Liu_Image_Inpainting_for_ECCV_2018_paper.html">partial conv</a> 논문에서도 사람 몸을 채우는 결과는 확인할 수 없었습니다.
<img src="/assets/img/portrait-image/K-002.png">
<br>
실제로 사람 몸을 잘 채우는 이전 연구는 없는 것 같습니다. 이 논문에서는 이 문제를 2 단계의 딥러닝을 이용하여 해결했습니다. 첫번째 단계에서는 사람의 structure를 복원하고, 두번째 단계에서 1단계의 이미지를 실제처럼 보이게 만듭니다. 
<br><br>

## Instruction

Portrait(초상화) 이미지 복원 문제에서 고려해야할 특성은 2가지입니다. 첫번째는 **semantic structure**이고, 두번째는 **missing object parts**입니다. **semantic structure**는 옷이라든가 신발이라든가, 또는 배경 따위를 말합니다. 비록 그 값들의 variance가 크지만, 그만큼 강한 제약 또한 있습니다. 예를 들어, 양쪽 신발의 색깔은
항상 같다는 제약을 통해, 세상에는 굉장히 다양한 신발이 존재하지만, 그럼에도 한쪽 신발을 통해 다른쪽 신발을 예측할 수 있을 것입니다. **object parts**는 사람의 몸이 대표적입니다. object parts를 예상하는 것은 힘든 일입니다. 사람의 몸은 3D 상에서 강한 제약을 가지고 있지만, 이미지는 이를 2D로 투영한 것입니다. 2D로 투영된 사람 몸은, 3D에서 처럼 강한 제약을 가지고 있지 않기 때문에, structure을 예상하고 복원하는 것은 어려운 일입니다.
<br>
다행히도, 이미지나 영상으로부터 사람의 몸의 structure와 pose를 추출하는 것은 이미 잘 연구되어 있습니다. 그래서 1차적으로, 복원해야하는 portrait 이미지에서 이미 연구되어 있는 방법을 이용하여 사람의 몸의 structure와 pose를 예측한 **parsing map**을 만듭니다. 그리고 2차적으로, 만들어진 parsing map으로부터 **image completion**을 수행합니다.
<br>
흔히 image completion은 image에 구멍이 나있을 때, 이를 매꾸는 inpainting 문제를 지칭하고, image extrapolation은 이미지 외부를 채우는 것을 지칭합니다. 이 연구는 portrait과 관련하여 image completion과 extapolation을 모두 수행할 수 있습니다.
<img src="/assets/img/portrait-image/K-003.png">
<br>
<br>
## Approach
<br>
위에서 언급했듯이 이 연구의 task에서 고려해야할 특성은 2가지가 있습니다. 사람 몸의 일부가 지워져있을 때, 그럴듯한 구조로 채워야하고, 일관적인 texture로 매꾸어야합니다. 그러나 이 2가지를 동시에 충족시키는 네트워크를 훈련시키는 것은 매우 어렵습니다. 그래서 2가지 stage로 나누어서 task를 풀었습니다.
<br>
- Stage-1: Human parsing
<br>
먼저 지워진 부분의 사람몸의 structure를 복원하기 위해 2018년도에 발표된 <a href="https://arxiv.org/abs/1804.01984">JPPNet</a>을 이용하였다. JPPNet은 Human parsing과 Pose estimation을 종합한 방법이다. Human parsing은 사람 몸의 이미지를 segment하는 것이다. 주어진 부분 뿐만 아니라 boundary
부분도 같이 예상하기 때문에 image completion 문제에 적합한 방식이다. 여기에 pose estimation을 더하면 정확도가 증가한다고 한다. 다음 그림을 보면 이해하기 편합니다.
<img src="/assets/img/portrait-image/K-004.png">
<br>
<br>
- Stage-2: Image completion
<br>
<img src="/assets/img/portrait-image/K-005.png">
<br>
input으로 주어진 이미지와, stage-1에서 만들어진 parsing map, 그리고 채워야하는 부분을 나타내는 bianry mask를 넣어줍니다. Encoder-decoder 구조를 사용하여서 output을 뽑아냅니다. Loss를 측정하기 위해 3가지 네트워크를 사용했습니다. 첫번째로 global discriminator는 전체 아웃풋 이미지를 인풋으로 받습니다.
local discriminator는 채워야하는 부분만을 input으로 받습니다. 그리고 vgg-19 네트워크는 perceptual loss를 계산하는데 사용됩니다.

## Result

<br>
<img src="/assets/img/portrait-image/K-006.png">