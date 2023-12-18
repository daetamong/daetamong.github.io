---
layout: post
title: L1/L2 Regularizaion에 대해 알아보자
subtitle: Regularization의 개념과 L1, L2의 차이점에 대해 알아보자
categories: 딥러닝
tags: [딥러닝]
---

`개인적으로 공부한 내용을 포스팅하기 때문에 잘못된 내용이 있을 수 있습니다. 만약 틀린 내용이 있다면 적극적인 피드백 부탁드립니다^^`


아마 간단한 프로젝트나 분석, 개발 실무를 하다보면 과적합 문제를 접해봤을 것이다. 그만큼 우리에게 편안한 데이터를 주는 경우는 많이 없기 때문이다. (이게 은근히 괴롭다...)

어쨋든 이러한 과적합 문제를 해결하는 방법은 여러가지가 있는데, 뭐가 있는지 한번 보자.

- 배치 정규화 (Batch Normalization)

- 정규화 (Weight Regularization)

- Dropout

과적합을 막는 대표적인 방법은 위의 3개 정도가 있곘다. (더 많지만)

물론 나머지도 다 정리할 예정이지만 이번 포스팅에서는 L1, L2 regularization에 대해서 정리해볼 예정이다.



### 과적합이 뭐지...?

과적합을 방지하는 방법에 대해 정리하기 전에 **과적합**이 무엇인지 알아보는게 좋을 것 같아 정리해본다.

과적합은 OverFitting이라고도 불리는데, 모델을 개발할 때 train과 test 데이터로 나눈 뒤에 train 데이터로 모델을 학습하고 test 데이터로 개발한 모델의 성능을 확인하는 것이 일반적이다.

아래의 그래프를 먼저 보자. (대충 그렸는데 그래프가 너무 이상하다..)

![do-messenger_screenshot_2023-12-14_13_44_53](https://github.com/daetamong/daetamong.github.io/assets/111731468/d306c8c1-2eaa-421f-834b-8d945eaa7dd9)

두 개의 그래프 중에서 어떤 모델이 더 정확할 것 같은가?

물론 이 상태로 봤을 때는 왼쪽의 모델이 거의 모든 데이터를 다 맞췄을 것이다.

그런데 만약 이렇게 학습된 모델에 새로운 형태의 데이터가 들어왔을 때도 과연 잘 맞출까?

오히려 오른쪽의 모델이 실제 test 데이터가 들어왔을 때 더 잘 맞출 수도 있다.

이처럼 train 데이터에 과하게 모델에 반영이 되어, 다시 말해 train 데이터에만 있는 노이즈를 모두 반영하여 모델이 학습되기 때문에 train 데이터를 다 맞추지만 새로운 데이터(=test)가 들어왔을 때, 거의 하나도 맞추지 못하는 현상을 과적합이라고 한다. (그냥 내가 이해하기 쉽게 풀어 썼다 ㅎㅎ)

어쨋든 이렇게 train 데이터에 맞춤형으로 모델이 개발되면 실제 성능 테스트를 할 때는 좋지 못하게 된다. 그래서 우리는 이를 해결할 수 있는 방법에 대해 알아볼 예정이다.



### Regularization이란?

우선 Regularization은 모델이 과적합되지 않도록 손실함수에 규제 함수를 더하여 손실함수가 너무 작아지지 않도록 weight에 패널티를 주는 기법이다.

일단, 왼쪽 그래프를 보면 우리가 중, 고등학교 때 배웠던 n차 함수형태인 것을 알 수 있다. 반대로 오른쪽 그래프는 대충 2차함수이지 않을까라는 생각을 할 수 있다.

여기서 regularization의 의미를 알 수가 있다. Regularization을 가중치 규제라고도 하는데 말 그대로 모델의 가중치를 규제하여 모델의 복잡성을 낮추는 과정을 의미한다.

즉, n차 함수에서의 각각의 가중치를 0으로 만들어서 2차 또는 3차 형태로 만드는 식으로 모델을 조금 더 간단한 형태로 만드는 과정이라고 보면 되겠다.

그런데 regularization에도 크게 두가지 종류가 있는데 L1과 L2가 있다. 이 두 개념의 차이도 한번 알아보자

그 전에 더 중요한 개념을 한번 알아보고 넘어가도록 하자



### L1 norm / L2 norm

우선 norm이란 개념부터 정리해보자.

**norm**이란 n차원의 벡터 공간에서 벡터 간 거리 또는 벡터의 절대적인 크기를 의미한다.

측정 가능한 공간인 Lp 공간에서의 norm을 Lp norm이라고 한다. Lp norm의 공식은 다음과 같다.

> $\ \vert \vert x \vert \vert _{p} = (sum_{i=1}^n \vert x_{i} \vert^p)^{1/p} $

여기서 p는 norm의 차수, n은 벡터의 차원 수를 의미하는데, p의 차수에 따라서 Lp norm의 형태는 아래의 그림과 같이 각각 다르게 만들어진다.

![do-messenger_screenshot_2023-12-14_15_39_37](https://github.com/daetamong/daetamong.github.io/assets/111731468/8e6822e3-5a5e-4db2-8fd7-56d073a82197)

<center>https://ekamperi.github.io/machine%20learning/2019/10/19/norms-in-machine-learning.html</center>

<br>

p가 1일 때는 마름모, 2일 때는 원형, 3일 떄는 뚱뚱한 원 그리고 100일 떄는 정사각형에 가까운 형태로 p차원이 커질수록 점점 정사각형에 가까워지는 형태를 띄고 있다.

이제 우리가 공부할 L1 regularization과 L2 regularization은 여기서 이름이 붙여진 것이라고 보면 되겠다.

L1 / L2 norm에 대해 좀 더 자세히 알아보자!!


### L1 norm

L1 norm은 맨하탄 거리로 불리기도 하며, 두 벡터 간의 최단 거리를 구하는데 사용되는 방법이다.

L1 loss는 실제값과 예측값 오차들의 절대값들에 대한 합을 의미한다.

>

![do-messenger_screenshot_2023-12-18_17_24_25](https://github.com/daetamong/daetamong.github.io/assets/111731468/60de789d-8eb5-4651-bc78-7c278507b217)

<center>https://rfriend.tistory.com/199</center>

<br>

위의 그림처럼 L1 norm은 하나의 목적지를 가는 데 있어서 여러가지 경로가 존재한다.



### L2 norm

그렇다면 L2 norm은 L1 norm과 어떤 차이가 있을끼?

L2 norm은 아마 많이 들어봤을텐데 유클리드 거리라고 불리며, L1 norm과 마찬가지로 두 점 사이의 최단 거리를 구할 떄 사용되는 방법이다.

L2 loss는 실제값과 예측값 오차들의 제곱의 합을 의미한다.

![do-messenger_screenshot_2023-12-18_17_28_39](https://github.com/daetamong/daetamong.github.io/assets/111731468/f56dc35e-3f9c-4746-81bc-84b3fa6cce8a)

<center>https://rfriend.tistory.com/199</center>

<br>

L2 norm은 L1 norm과는 반대로, 하나의 목적지를 가는 데 있어서 하나의 방법만 존재한다.

그리고 수식을 보면 알겠지만 L2 norm은 오차에 제곱을 해주기 때문에 outlier에 좀 더 많은 영향을 받는다.


### 정리를 하자면..?

이 두 방법의 차이점을 정리해보자면,,

미분에서도 차이가 있다. L1 norm은 절대값으로 구하기 때문에 미분이 불가능하여 편미분 과정에서 weight의 부호만 바뀌기 때문에 weight의 크기에 따라 규제가 크게 변하지 않기 때문에 Regularization의 효과가 L2 norm에 비해 작다는 단점이 있다.

반대로 L2 norm은 제곱을 사용하기 떄문에 weight의 부호뿐만 아니라 값도 크게 바뀔 수 있기 떄문에 그 만큼 패널티를 줄 수 있고, 규제를 좀 더 효과적으로 줄 수 있다.

따라서 특정 weight가 커지는 것을 방지하는 weight decay가 가능해진다.

이려한 여러 특징들 때문에 L2 norm이 여러 측면에서 보다 효과적이기 때문에 L1 norm보다는 L2 norm을 더 많이 사용한다.

그렇다고 해서 L1 norm의 장점이 없는 것은 아니다. L1 norm의 경우에는 목적지로 가는 여러 방법 중 특정 방법을 0으로 처리하는 것이 가능하기 때문에 특정 weight만 남길 수 있다. 따라서 일반적으로 언급되는 feature selection이 가능하다는 장점이 있다.

그렇기에 무조건 L2 norm을 써야지!! 보다는 상황과 데이터 특성, 분석 목적에 맞게 적절한 방법을 사용하는 것이 좋겠다.

이번 포스팅은 내용이 많기도 하고 어렵기 떄문에 나눠서 포스팅을 정리하려고 한다.

다음 포스팅에서는 L1 norm과 L2 norm을 활용한 regularization에 대해 자세히 알아보자

<br>

ref : https://seongyun-dev.tistory.com/52