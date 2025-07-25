---
layout: post
title: Collaborative Filtering은 무엇인가?
subtitle: 협업필터링(Collaborative Filtering)에 대해서 알아보자!
categories: 추천시스템
tags: [추천시스템]
---

`개인적으로 공부한 내용을 포스팅하기 때문에 잘못된 내용이 있을 수 있습니다. 만약 틀린 내용이 있다면 적극적인 피드백 부탁드립니다^^`


이번 포스팅도 **추천시스템**에 관한 내용을 올려보고자 한다.

추천시스템에 관한 기초적인 내용을 올린적이 없어서 나의 흩어진 지식들을 체계화하고자 다시 정리하려고 한다.

각설하고! 추천시스템 분야 중에서 대표적인 개념인 협업핕터링(Collaborative Filtering)에 대해서 공부해보자!


### 협업핕터링(Collaborative Filtering)이란?

> 협업필터링이란 "사용자의 사용 패턴, 기록 등 행동 방식"을 기반으로 상품을 추천하는 알고리즘이다.

> 유형으로는 "메모리 기반 협업필터링" 과 "모델 기반 협업필터링"으로 나눌 수 있다.


<br>


### 메모리 기반 협업필터링 (Memory based Collaborative Filtering)

메모리 기반 협업필터링부터 찬찬히.. 살펴보자!

메모리 기반의 경우, 크게 두 가지 유형으로 나눌 수 있다.

<img width="690" alt="스크린샷 2024-01-09 오후 5 39 09" src="https://github.com/daetamong/daetamong.github.io/assets/111731468/3682040d-aa9c-4e63-9579-20c5ef7921c2">

<center> https://groobee.net/blog/%ED%98%91%EC%97%85-%ED%95%84%ED%84%B0%EB%A7%81/ </center>

<br>

왼족 첫 번째는 **사용자 기반 협업필터링**이고, 오른쪽 두 번째는 **아이템 기반 협업필터링** 이다.


#### 1. 사용자 기반 협업핕터링 (User based Collaborative Filtering)

> 사용자 기반 협업필터링은 특정 아이템에 대해 유사한 평가를 준 사용자를 찾은 후, 해당 사용자가 좋은 평가를 준 또 다른 상품에 대한 잠재적인 선호도를 예측하고, 이를 기반으로 추천하는 방식이다.

다시 말해, "사람들이 A 상품을 좋아하니까 얘한테도 A를 추천하면 좋아할 거야!"와 같이 성향이 비슷한 사용자들이 선호하는 주 상품군을 해당 그룹에 속한 사용자에게 추천하는 방식이다.

그림으로 표현하면 다음과 같다.

<img width="305" alt="스크린샷 2024-01-09 오후 5 42 08" src="https://github.com/daetamong/daetamong.github.io/assets/111731468/6844b7ad-c96b-4462-b9ad-b624fcfa9ffe">


첫 번째 사람부터 순서대로 A, B, C라고 했을 때, A와 C는 포도, 딸기, 수박을 공통적으로 좋아하는 만큼 취향이 비슷하다고 볼 수 있는데,

C는 우연히(?) 오렌지에 대한 평가가 없다. 하지만 우리는 이 둘의 취향이 비슷하다는 것을 대충 알고 있다.

그리고 A는 오렌지에 대한 선호도가 높다.. 그렇다면..!! 우리는 다음과 같은 결론을 내릴 수 있을 것이다.

"A가 오렌지를 좋아하니까 C도 오렌지를 좋아할 거야!"

최종적으로 우리는 위와 같은 로직으로 C에게도 오렌지를 추천할 수 있는 근거가 생기게 된다.

이처럼, 사용자 간의 선호도와 같은 행동유형을 바탕으로 유사도를 계산하여 취향이 비슷한 사용자에게 새로운 상품을 추천하는 알고리즘을 "사용자 기반 협업필터링"이라고 보면 되겠다.



#### 2. 아이템 기반 협업필터링 (Item based Collaborative Filtering)


> 아이템 기반 협업핕터링은 아이템에 대한 고유 특성을 사용한 알고리즘으로, 특정 아이템과 유사한 아이템의 잠재적인 선호도를 예측하고, 추천하는 방식이다.

> "얘가 A 상품을 좋아하는데 A하고 B가 비슷하니까 B를 추천하면 좋아할 거야!" 처럼 상품 간의 유사도를 기반으로 추천한다.

<img width="325" alt="스크린샷 2024-01-09 오후 5 42 55" src="https://github.com/daetamong/daetamong.github.io/assets/111731468/612f410e-0c9c-478c-b61d-b10c86bcb548">


"아이템 기반 협업필터링"도 비슷한 로직을 따라간다. 수박하고 포도가 비슷한(?) 특징을 갖고 있다고 하자...

그런데 마침 C가 수박을 좋아한다고 평기를 했다!! 그렇다면 우리는 수박과 비슷한 과일인 포도를 추천할 수 있는 것이다.

이처럼 사용자 선호도가 아닌, 상품의 고유 특성의 유사도를 계산하여 기존의 상품과 비슷한 상품을 추천하는 알고리즘을 "아이템 기반 협업필터링"이라고 보면 되겠다.

그런데,,, 계속해서 유사도를 계산한다고 언급을 했는데, 그렇다면 "유사도는 어떻게 계산하는데?"라는 궁금증이 생길 수 있다.

그래서 유사도를 계산하는 대표적인 방법에 대해 정리해보려고 한다.



### 유사도 계산 방법

유사도를 계산하는 방법은 대표적으로 3가지가 있다.

#### 상관계수 (Correlation coefficient)

상관계수는 두 변수 간의 상관관계를 의미하는 수치이다.

상관계수는 -1에서 1의 값을 가지며 1에 가까울수록 양의 상관관계를 갖고 있고, -1에 가까울수록 음의 상관관계를 갖고 있다고 해석한다.

식을 보면 두 변수 x와 y가 있을 때 $(x_{1}, y_{1}), (x_{2}, y_{2}) ..., (x_{n}, y_{n})$ 이 주어질 때, 상관계수는 다음과 같이 구할 수 있다.

$SIM(x, y) = \frac{\sum_{i=1}^n (x_{i} - \bar x)(y_{i} - \bar y)}{\sqrt{\sum_{i=1}^n (x_{i} - \bar x)^2 \sum_{i=1}^n (y_{i} - \bar y)^2}}$


#### 코사인 유사도 (Cosine Similarity)

코사인 유사도는 일반적으로 두 벡터 간의 유사도를 계산할 때 사용한다.

즉, 두 벡터 간의 사잇각을 구해 얼마나 유사한지 계산하는 방식이다.

$Cos(\Theta) = \frac{A \cdot B}{\Vert A \Vert \Vert B \Vert} = \frac{\sum_{i=1}^n A_{i} B_{i}}{\sqrt{\sum_{i=1}^n (A_{i}^2)} \sqrt{\sum_{i=1}^n (B_{i}^2)}}$


#### 자카드 유사도

자카드는 교집합을 합집합으로 나눈 값이다.

$J(A, B) = \frac{\vert A \cap B \vert}{\vert A \cup B \vert} = \frac{\vert A \cap B \vert}{\vert A \vert + \vert B \vert - \vert A \cap B \vert}$


#### 타니모토 유사도

타니모토 유사도는 데이터가 이진값일 때 주로 사용된다.

타니모토 유사도는 자카드 유사도와 유사한다. 차이점은 타니모토 유사도에는 벡터화를 통해 가중치를 부여한다는 점이 있다.

$Tanimoto Coefficient = \frac{A \cdot B}{\vert A \vert^2 + \vert B \vert^2 - A \cdot B}$


### 모델 기반 협업필터링

지금까지의 메모리 기반 협업필터링은 기존의 데이터와 유사도 알고리즘을 통해 추천하는 방식이었다면, 모델 기반 알고리즘은 말 그대로 **머신러닝, 딥러닝** 등 모델의 학습을 통해 아이템의 잠재 특성을 계산하여 추천하는 방식이다.

추천 로직은 크게 1) latent factor를 계산하여 추천하는 방식과 2) 분류/회귀를 통해 추천하는 방식이 있다.

대표적인 방법은 이전 포스팅에서 정리했던 MF(Matrix Factorization)과 SVD(특이점 분해, Single Value Decomposition)나, PCA를 사용할 수 있다. (이 개념들도 추후에 정리할 예정이다!)




### 협업필터링의 장단점은?

협업필터링의 유형과 개념에 대해 알아보았다.

그렇다면,, 마지막으로 협업필터링의 장단점에 대해서도 알아볼 필요가 있지 않을까??

하나씩 살펴보자!


### 장점

#### 1. 도메인 지식이 없어도 추천은 가능하다!

> 우선, 장점은 도메인 지식이 없어도 당장은 구현이 가능하다. 이 뜻은 곧, 간단한 테스트를 할 때 "일단은 돌려봐!"가 가능해진다. 다시 말해서 서비스를 기획하거나 개발할 때 서비스 방향을 세울 수 있는 초석이 될 수 있다.

> 추가적으로, 사용자의 구매 데이터와 사용자-상품 간의 상호작용 데이터만 있으면 비교적 높은 정확도의 상품 추천이 가능해진다.


#### 2. 결과가 매우 직관적이다!!!

> 두 번째는, 결과가 매우 직관적인다. user-item 형태의 행렬이나 데이터프레임 형태로 출력이 가능하기 때문에 "어떤 사용자에게 어떤 상품을 추천하면 좋을까?"라는 인사이트를 바로 얻을 수 있다.


#### 3. 다양한 상품을 추천할 수 있다!!

> 내가 생각하는 협업필터링의 가장 큰 장점이라고 생각한다. 사용, 선호도 데이터를 가지고 기존에 없는 새로운 정보를 알 수 있다는 점이다.

> 즉, 특정 사용자의 새로운 상품에 대한 새로운 선호도를 예측함으로써 다양한 상품을 추천할 수 있게 된다.


### 단점

장점이 있다면, 단점도 있는 법!!

단점에 대해서도 알아보자!


#### 1. Cold-Start 문제ㅠㅠ

추천시스템을 공부해본 사람이라면 한번쯤은 들어봤을만한 문제이다...

cold start란 **"신규 사용자나, 새로운 상품이 추가 되었을 때, 기존에 관련된 연관정보가 없기 때문에 정확한 추천이 어려워지는 문제"** 이다.

다시 말해, 협업필터링은 과거의 선호도나 이용 데이터(사용자 기반 CF) 그리고 콘텐츠 자체의 정보(아이템 기반 CF)를 가지고 추천을 했었는데, 신규 사용자 or 상품이 들어오면 연관성을 계산할 정보가 없게 되어 결과가 부정확해 지는 문제이다.ConnectionRefusedError

그렇다고 해서 해결이 안되는 문제는 아니다..

차선책으로... 신규 사용자의 경우, 가장 기본의 정보로 클러스터링 기법을 통해서 해당 군집에 포함된 사용자들의 선호도가 높은 상품을 우선적으로 추천을 한 후, 데이터가 쌓이면 그 뒤에 정확한 추천을 할 수는 있다.ConnectionRefusedError

다만, 이 경우에는 어느정도의 시간이 필요하다.


#### 2. Long-tail 문제ㅠㅠ

Long tail 문제는 **""인기가 많은 소수의 아이템이 계속해서 추천되는 한쪽 솔림현상**을 의미한다.hash

즉, "인기있는 이유가 있을 거야!!" 라는 사용자들의 심리가 반영된 문제라고 보면 되겠다. 그래서 인기있는 아이템만 계속 사용되고 이로 인해 인기있는 아이템만 계속 추천이 되는 것이다.


#### 3. 계산 효율...

지금까지는 어떻게보면 CRM이나 마케팅 측면에서의 단점이었다면,, 이번 단점은 컴퓨팅, 개발 측면에서의 단점이다.credits

협업필터링 자체가 연산량이 많은 알고리즘이기 때문에 신규 유입자나 상품이 계속 늘어날 경우, 계산량이 늘어나 효율성이 저하되는 문제가 있다.

이를 해결하려는 연구는 계속해서 나오고 있으니, 여러 논문을 참조하면 어느정도는 해결될 것이라고 기대를 하고 있다!


<br>

지금까지 협업필터링의 개념과 장단점에 대해 알아보았다.

추천시스템 관련 코드나 간단한 실습을 올릴 예정인데.... 최대한 빨리 올려야지

<hr>
<br>

ref : https://www.blossominkyung.com/recommendersystem/collaborative-filtering

ref : https://deepdaiv.oopy.io/articles/1

ref : https://chaheekwon.tistory.com/entry/Collaborative-Filtering