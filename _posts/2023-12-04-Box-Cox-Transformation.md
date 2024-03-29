---
layout: post
title: Box-Cox Transformation(변환)에 대해 알아보자
subtitle: Box-Cox 변환의 개념과 사용법에 대해 알아보자
categories: 통계학
tags: [통계학]
---

`개인적으로 공부한 내용을 포스팅하기 때문에 잘못된 내용이 있을 수 있습니다. 만약 틀린 내용이 있다면 적극적인 피드백 부탁드립니다^^`


### 회귀분석에서...?
아마 통계학에 대해 조금이라도 공부한 사람이라면 회귀분석을 한번쯤은 들어봤을 것이다. 마찬가지로 회귀분석을 수행하기 위한 가정도 분명 들어봤을 것이다.

> 1. 선형성
> 2. 독립성
> 3. 등분산성
> 4. 정규성

해당 포스팅에서는 위의 개념이 주 내용이 아니기 때문에 간단하게만 살펴보고 넘어가려고 한다.

우선, 선형성은 말그대로 예측하고자 하는 종속변수 y와 독립변수 x간에 선형성을 만족해야 한다는 것을 의미한다.

두 번째, 독립성은 독립변수 x들 간에 상관관계가 없어야 한다는 것을 의미한다. 말 그대로 독립적이야 한다는 것이겠다.

세 번째, 등분산성은 오차의 모든 분산이 고르게 분포되어 있어야 한다. 즉, 어떠한 패턴을 띄는 것이 아니라 정말 '고르게' 분포되어 있어야 한다는 것이다.

우리가 중점적으로 살펴볼 마지막 가정인 **"정규성"**은 우리가 살펴볼 데이터가 정규분포를 띄고 있어야 한다는 것이다.

이 외의 통계분석에도 여러가지 가정이 있지만, 일단 가장 보편적이고 중요한 4개의 가정을 살펴봤다. 그 중에서 우리는 4번째 가정인 정규성에 대한 개념을 살펴보고자 한다.



### 정규성을 가져야 하는 이유

데이터 분석 실무나 개인 프로젝트를 하다보면 시각화를 했을 때 정규분포를 띄는 컬럼은 많이 없을 것이다.

대부분이 아래의 그림처럼 되어 있을 것이다. 왼쪽처럼 데이터가 한쪽으로 치우쳐져 있거나, 아니면 데이터가 들쑥날쑥(?) 분포되어 있거나 등등

우리가 원하는 정규분포를 띄는 경우는 내 경험상 많이 없었다.

![image](https://github.com/daetamong/daetamong.github.io/assets/111731468/e6bdaa6b-f0e7-4c15-b64d-072f8635dcf7)


아마 머신러닝을 공부했다면 데이터를 정규분포에 가깝도록 처리해줘야 모델 성능이 올라간다는 것을 알고 있을 것이다. (왜 그런지도 나중에 포스팅할 예정이다.)

그래서 우리는 위와 같은 분포를 가지는 데이터를 정규분포로 만드는 방법을 알아볼 예정이다.

### Box-Cox Transformation이란?

그래서 우리는 데이터의 정규성을 확보할 수 있는 여러 기법 중 **Box-Cox 변환**에 대해 정리해보려고 한다.

우선 Box-Cox 변환의 식은 다음과 같다.

양수인 반응변수 $y = (y_{1}, y_{2}, ... , y_{n})^{t}$ 에 대하여 Box-Cox 변환($y_{i}(\lambda)$)은 다음과 같이 정의된다.

$y_{i}(\lambda) =
\begin{cases}
    frac{y_{i}^{\lambda}-1}{\lambda}, & \text{if } \lambda \ne 0, \\
    log(y_{i}), & \text{if } \lambda = 0
\end{cases}$


식을 보면 람다값만 정해지면 변수변환을 할 수 있다. 즉, 람다는 하나의 파라미터임과 동시에 람다에 어떤 값을 지정해주느냐에 따라 여러 형태의 변수 변환법이 될 수 있다.

| Lambda | 변수 변환법 |
| :------ |:--- |
| +2 | 이차 변환 |
| +0.5 | 제곱근 변환 |
| 0 | 로그 변환 |
| -1 | 역수 변환 |

이외에도 많은 변환법이 있지만 대표적인 변환법만 정리해봤다. 

각 변수변화법마다의 특징?을 간단하게 살펴보면 Skeness가 심한 경우를 가정했을 때 이차 변환은 작은값을 더 크게 만들어줌으로써 정규분포에 가깝게 해주는 기능을 갖는다.

반대로 제곱근, 로그, 역수 변환은 큰 값을 작게 만들어줌으로써 정규분포에 가깝게 해주는 기능을 갖는다. 그리고 데이터 간의 간격이 클 경우, 간격을 좁혀줌으로써 좀 더 정규분포에 가깝게 해줄 수 있다.

그래서 상황에 맞게 우리가 찾는 $\lambda$는 최대한 정규분포에 가깝도록 하는 값으로 지정해주면 된다.




### 실습을 해보자!

변환법에 대한 내용이 주 이기 떄문에 이번 실습에서는 데이터가 어떻게 생겼는지는 따로 정리하지는 않겠다. 

대충 아래처럼 생긴 데이터가 있다고 하자. 그래프를 보면 파란색, 빨간색 두 가지 그래프가 동시에 그려져 있는데

분포가 다른 두 그룹에 동시에 같은 기법을 적용했을 떄 어떻게 변화하는지 확인하기 위해 두 개의 데이터를 만들어 보았다.

<img width="850" alt="스크린샷 2023-12-09 오후 5 35 20" src="https://github.com/daetamong/daetamong.github.io/assets/111731468/882c2916-e49d-4252-b66d-a27b2a33b459">


그래프를 보면 두 그룹 모두 정규분포를 띄고 있지 않고, 한쪽으로 쏠려있는 분포를 가지고 있는 것을 확인할 수 있다.

우리는 이 데이터를 그대로 쓰지 않고, 최대한 정규분포에 가깝도록 변환한 후에 사용할 것이다.

첫 번째 방법은 로그변환은 사용해볼 것이다. 로그변환은 numpy의 log1p 함수를 사용하면 된다.

```Python
data_group1 = np.log1p(data_group1['col1'])
data_group2 = np.log1p(data_group2['col2'])
```

우리가 갖고 있는 데이터에서 변환을 적용하고 싶은 컬럼을 지정해줘서 위의 코드와 같이 작성해서 실행하면 된다.

결과를 보자!



### 로그변환 결과

<img width="892" alt="스크린샷 2023-12-09 오후 5 40 37" src="https://github.com/daetamong/daetamong.github.io/assets/111731468/5db09537-32fd-4aeb-baf7-e4436ffcffe5">


결과를 보면 변환을 적용하지 않는 그래프와 비교했을 때 비교적 정규분포에 가까워진 것을 확인할 수 있다.

그리고 x축의 단위도 기존에는 실제 컬럼의 단위로 되어 있었지만 로그 변환을 적용하면서 x축의 단위도 바뀐 것을 확인할 수 있다.



이번에는 box-cos 변환을 사용해보자

위에서 정리를 했지만 $\lambda$에 따라 값이 바뀐다고 했었다.

이번 코드는 최적의 $\lambda$ 값을 찾고 이를 적용하는 과정을 의미한고 보면 되겠다.

```Python
y_group1, lambda_group1 = stats.boxcox(data_group1['col1'])
y_group2, lambda_group2 = stats.boxcox(data_group2['col2'])
```

결과를 보면 y_group1과 y_group2는 변환을 적용했을 때의 값들을 의미한다.

그리고, labmda_group1과 lambda_group2는 최적의 $\lambda$ 값이라고 생각하면 되겠다.

이미 우리는 box-cox 변화을 적용했다. 그렇다면 얼마나 정규분포에 가까워졌는지 시각화를 해봐야 하지 않겠는가!!

그래프를 그려보자!



### Box-Cox 변환 결과

<img width="895" alt="스크린샷 2023-12-09 오후 5 47 41" src="https://github.com/daetamong/daetamong.github.io/assets/111731468/d8fbc5a8-7c26-4da0-989d-088182842a6a">

로그 변환을 적용한 위의 그래프보다 좀 더 정규분포에 가까워졌다!

그리고 또 차이점이 있다면 두 그룹간의 분포 차이가 더 커졌다.

변환법에 따라 시각화 결과도 달라지기 때문에 어떤 데이터에서는 어떤 변환법이 적절한지는 상황과 기준에 맞게 각자 판단하여 사용하면 될 것 같다.

나머지 변환법도 실습을 해보려 했지만 두 가지로도 충분한 것 같다 여기서 stay하겠다. ㅎㅎ


지금까지 변환법에 대해 알아보았다.

확실히 분포가 한쪽으로 취우쳐져 있는 경우에는 변환법의 효과가 더 컸던 것 같다.

데이터 전처리나 여러 용도로 잘 활용하면 효율적인 분석이 될 수 있을 것 같다.

