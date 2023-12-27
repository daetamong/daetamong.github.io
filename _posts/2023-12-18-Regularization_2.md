---
layout: post
title: L1/L2 Regularizaion에 대해 알아보자2
subtitle: Regularization의 개념과 L1, L2의 차이점에 대해 알아보자
categories: 딥러닝
tags: [딥러닝]
---

`개인적으로 공부한 내용을 포스팅하기 때문에 잘못된 내용이 있을 수 있습니다. 만약 틀린 내용이 있다면 적극적인 피드백 부탁드립니다^^`


지난 포스팅에 이어서 L1, L2 regularization에 대해서 공부해보자

우선 **Regularization**이란 개념에 대해서 다시 한번 정리해보면,,

> Regularization은 모델이 과적합되지 않도록 손실함수에 규제 함수를 더하여 손실함수가 너무 작아지지 않도록 weight에 패널티를 부여하는 방법이다

다시 말해서, 모델의 weigth를 최소화하여 모델을 단순화시키는 방법이라고 보면 되겠다.

그런데 이제 패널티를 주는 방법에 따라 명칭이 L1과 L2 regularization으로 나눠진다.

그래서 우리가 지난 포스팅에서 L1 norm과 L2 norm에 대해서 공부한 이유도 이 때문이다.

우선 L1 Regularization부터 살펴보자



### L1 Regularization이란?

우선 L1 regularization은 기존의 손실함수에서 가중치의 절대값의 합을 더해주는 형태이다.

즉, L1 norm을 특정 범위로 제한하여 그 범위 안에서만 최적의 해를 찾도록 하는 형식이다.

그래서 미분을 했을 때, **weight의 값에 상관없이 부호에 따라 상수값을 빼주거나 더해주는** 식으로 계산이 된다.

따라서 특정 weight를 0으로 만들 수 있게 됨으로써 특정 weight를 남길 수 있는 식의 feature selection이 가능하게 된다.

이런식으로 특정 weight를 없애는 식의 과정을 통해 모델을 간단한 형태로 만들어 줄 수 있는 것이다.

일단, 식을 한번 살펴보자

> $ Cost = \sum_{i=0}^N(y_{i} - \sum_{j=0}^M(x_{ij}W_{j}))^2 + \lambda \sum_{j=0}^M \vert W_{j} \vert $

앞의 식은 일반적인 Cost function임을 알 수 있다.

하지만 우리가 알고 있는 cost function하고는 살짝 다르다.

뒤에 추가된 식을 보면 가중치의 합의 절대값을 더해주고 있는데, 여기가 위에서 말한 가중치에 패널티를 부여하는 부분이라고 보면 되겠다.

다음은 L2 regularization을 보자!


### L2 Regularization이란?

L1 Regularizaion과 마찬가지로 L2 norm에 특정 범위로 제한하여 최적의 해를 찾는 방식으로 진행된다.

L1은 가중치의 절대값의 합을 더해준다고 했다.

하지만 L2는 절대값이 아닌 제곱의 합을 더해준다.

이렇게 되면 weight의 크기에 따라 더 많은 패널티를 부여하는 weight decay 방식이 적용된다. 따라서 weight에 따라 패널티가 다르게 부여되기 때문에 L1 보다는 규제의 효과가 더 크다는 특징이 있다.

식을 살펴보자!

> $ Cost = \sum_{i=0}^N(y_{i} - \sum_{j=0}^M(x_{ij}W_{j}))^2 + \lambda \sum_{j=0}^M W_{j}^2 $

L1 regularization도 마찬가지지만 $ \lambda $ 를 매우 크게 하면 모든 weight가 0이 되기 때문에 최종적으로 모델은 직선 모델이 된다.

다시 말해, under fitting이 되는데, 반대로 $ \lambda $ 를 작게하면 모든 weight가 살아남아 over fitting이 될 수 있는 위험성이 존재한다.

따라서 이러한 문제를 해결하기 위해서는 어려우면서도 쉬운 **적절한** 값을 지정해줘야 한다.

<br>

### 규제 영역과 Loss Function의 관계

아래의 그림은 weight와 loss function의 관계에 대해서 그림으로 나타낸 것이다.

각 그래프마다 다른 모형으로 되어 있는데, 이는 지난번 포스팅에서 정리했던 Lp norm에서 p값에 따른 분포형태를 보여준다.

각 모형은 규제 영역이라고 보면 되고, 그 위에 타원형으로 되어 있는 부분은 우리가 찾고자 하는 loss function의 global optimum를 의미한다고 보면 되겠다.

이를 통해 **"우리는 실제값에 대한 bias를 잃는 대신, variance를 얻어 over fitting을 막는다"**라고 보면 되겠다.

그리고 global optimum의 범위는 제약조건으로 인해 각 모형의 영역을 벗어날 수 없게 되고 weight가 무한정 커지는 것을 방지해준다.

그래서 결국에는 두 선(?)이 만나는 지점이 최적의 값이 된다고 보면 되겠다.


![image](https://github.com/daetamong/daetamong.github.io/assets/111731468/ee8ec887-d986-4f2a-94e9-454dd4b2c731)

<center>https://medium.com/m/global-identity-2?redirectUrl=https%3A%2F%2Fmedium.datadriveninvestor.com%2Fthe-art-of-regularization-caca8de7614e</center>

<br>

### Lasso / Ridge Regression Model

어떻게 보면 가장 중요한 개념을 마지막에 소개하는 것 같은데,, 어쨋든,,

회귀를 짧게나마 공부해본 사람이라면 Lasso(라쏘)와 Ridge(릿지) 회귀 모형이라는 이름을 들어 본 적 있을 것이다.

이 두 모형은 각각 L1과 L2 규제를 준 모델이라고 보면 되곘다.

두 모형의 특징을 한번 살펴보자

| 모델 | 규제 | 장점 | 단점 | 특징 |
| ----: | --------------: | ---------: | -----: |
|Lasso | L1 Regularization | - 변수 선택이 가능하다 | - L2에 비해 효과가 떨어짐 |- 변수 간 상관관계가 높으면 효과가 떨어진다|
|Ridge | L2 Regularization | - 모델의 전반적인 복잡도를 줄여준다 | - 변수 선택이 불가능하다 |- 변수 간 상관관계와 상관없이 효과가 좋다|

두 모델 모두 각각의 장단점이 있으니 뭐가 무조건 정답이라는 것이 없다. 그렇기에 상황에 맞게 적잘한 모델이나 방식을 사용하면 되겠다.

<br>

ref : https://seongyun-dev.tistory.com/52
ref : https://www.youtube.com/watch?v=pJCcGK5omhE&t=21s