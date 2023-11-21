---
layout: post
title: Gradient Descent(경사하강법) 실습
subtitle: Gradient Descent을 Python으로 구현해보자!
categories: 머신러닝
tags: [머신러닝]
---

`개인적으로 공부한 내용을 포스팅하기 때문에 잘못된 내용이 있을 수 있습니다. 만약 틀린 내용이 있다면 적극적인 피드백 부탁드립니다^^`

### Gradient Descent를 Python으로 구현해보자!

이전 포스팅에서 Gradient Descent의 개념과 계산 방식에 대해 알아보았다.

이론까지 공부하고 넘어가기에는 부족한 거 같아서, 간단한 데이터를 가지고 python을 통해 직접 구현해보고자 한다.


### 파라미터 초기 설정

우선, 우리가 찾고자 하는 값인 파라미터(w, b)에 대해서 초기값을 설정해주자!

```python
weight, bias = np.random.rand(2)
```