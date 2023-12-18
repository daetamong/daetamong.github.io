---
layout: post
title: L1/L2 Regularizaion에 대해 알아보자
subtitle: Regularization의 개념과 L1, L2의 차이점에 대해 알아보자
categories: 딥러닝
tags: [딥러닝]
---

`개인적으로 공부한 내용을 포스팅하기 때문에 잘못된 내용이 있을 수 있습니다. 만약 틀린 내용이 있다면 적극적인 피드백 부탁드립니다^^`


지난 포스팅에 이어서 L1, L2 regularization에 대해서 공부해보자

우선 **Regularization**이란 개념에 대해서 다시 한번 정리해보면,,

> Regularization은 모델이 과적합되지 않도록 손실함수에 규제 함수를 더하여 손실함수가 너무 작아지지 않도록 weight에 패널티를 부여하는 방법이다

그런데 이제 패널티를 주는 방법에 따라 명칭이 L1과 L2 regularization으로 나눠진다.

그래서 우리가 지난 포스팅에서 L1 norm과 L2 norm에 대해서 공부한 이유도 이 때문이다.

우선 L1 Regularization부터 살펴보자



### L1 Regularization이란?

우선 L1 regularization은 기존의 손실함수에서 가중치의 절대값의 합을 더해주는 형태이다.

그래서 미분을 했을 때, **weight의 값에 상관없이 부호에 따라 상수값을 빼주거나 더해주는** 식으로 계산이 된다.

따라서 특정 weight를 0으로 만들 수 있게 됨으로써 특정 weight를 남길 수 있는 식의 feature selection이 가능하게 된다.

이런식으로 특정 weight를 없애는 식의 과정을 통해 모델을 간단한 형태로 만들어 줄 수 있는 것이다.

일단, 식을 한번 살펴보자

> $ Cost = sum_{i=0}^N(y_{i} - sum_{j=0}^M(x_{ij}W_{j}))^2 + \lambda sum_{j=0}^M \vert W_{j} \vert $



<br>

ref : https://seongyun-dev.tistory.com/52