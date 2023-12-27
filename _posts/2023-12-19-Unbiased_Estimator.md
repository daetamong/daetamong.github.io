---
layout: post
title: 불편추정량에 대해서 알아보자!
subtitle: 불편추정량의 개념과 이에 관련된 내용을 공부해보자!
categories: 통계학
tags: [통계학]
---

`개인적으로 공부한 내용을 포스팅하기 때문에 잘못된 내용이 있을 수 있습니다. 만약 틀린 내용이 있다면 적극적인 피드백 부탁드립니다^^`

일하면서 통계를 다시 끄적이게 되서 이왕 통계를 다시 쓰는거 정리해보려고 한다.

우선! 통계학을 공부해본 사람이라면 무조건 한번씩을 들었을만한 개념인데, **"불편추정량"** 에 대해서 정리해보려고 한다.

불편추정량이 대충 어떤 개념인지는 알겠는데 정확히 뭔지 모르겠는(?) 나를 위해서! 한번 알아보자!


### 불편추정량이란?

불편추정량에 대해서 간단하게 한문장으로 정리해보면 다음과 같다.

> 불편추정량이란, 추정량의 기대값과 모수의 값이 차이가 없는 추정량이다.

여기서 추정량은 표본으로부터 모수를 추정하기 위한 표본 통계량이라고 보면 되겠다.

다시 말해서, 불편추정량은 말 그대로 편향($\theta$)이 없는 추정량이다.

이를 식으로 나타내보면 다음과 같다.

> $E(\theta^(\hat X)) = \theta $

위에서 계속 말했지만 추정량의 평균이 모수의 통계량과 동일하다는 의미로 해석하면 되겠다.

반대로 모수의 통계량과 다르다면 **편의추정량**이라고 부른다.

우선, 표본평균은 모평균과 같기 때문에 불편추정량이다.

그리고 n으로 나눴을 때는 표본분산은 모분산과 다르기 때문에 편의추정량이다.

하지만 표본분산을 계산할 때 n-1로 나누는 것을 볼 수 있는데 n-1로 나누게 되면 모분산과 같아지기 때문에 불편추정량으로 된다.

왜 n-1로 나누는지 수식을 통해 살펴보도록 하자!


### 과연 표본평균과 표본분산은 불편추정량일까??

일반적으로 우리는 평균을 구할 때

> $ \bar X = \frac1n \sum_{i=1}^nX_{i}$

위의 식처럼 계산을 한다.

이걸 다시 표현해보면, 

$E(\bar X) = \mu$ 로 표현할 수 있다.

마찬가지로 분산도 표현해보면

$E(s^2) = \sigma^2$ 와 같이 표현할 수 있을 것이다.

그런데 위에서 말했듯이 표본분산은 편의추정량이기 때문에 모분산을 반영하지 못한다.

왜 그런지 한번 확인해보도록 하자


### 왜 표본분산은 불편추정량이 아닐까?

> $E(s^2) = E(\frac1n \sum_{i=1}^n (X_{i} - \bar X)^2)$

> $ = \frac1n E(\sum_{i=1}^n(X_{i}^2 - 2X_{i}\bar X + \bar X^2)) $

> $ = \frac1n E(\sum_{i=1}^n(X_{i}^2) - 2\bar X \sum_{i=1}^nX_{i} + \sum_{i=1}^n \bar X)$

> $ = \frac1n E(\sum_{i=1}^n X^2 - 2n \bar X^2 + n \bar X^2) $

> $ = \frac1n \sum_{i=1}^n E(X_{i}^2) - E(\bar X^2)$

> $ = \frac1n \sum_{i=1}^n(\mu^2 + \sigma^2) - (\mu^2 + \frac{\sigma^2}{n}) $

> $ = \frac{n-1}{n} \sigma^2$

결과를 보니 위에서 정리한 불편추정량의 조건을 만족하지 않는다.

하지만 n-1을 곱해준다면? 우리가 원하는 불편추정량의 조건을 만족하게 된다. 그래서 표본 분산을 구할 때 n대신 n-1을 곱해주는 것도 여기에 있다.