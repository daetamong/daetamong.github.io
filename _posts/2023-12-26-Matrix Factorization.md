---
layout: post
title: Matrix Factorization은 무엇인가?
subtitle: Matrix Factorization에 대해서 알아보자!
categories: 추천시스템
tags: [추천시스템]
---

`개인적으로 공부한 내용을 포스팅하기 때문에 잘못된 내용이 있을 수 있습니다. 만약 틀린 내용이 있다면 적극적인 피드백 부탁드립니다^^`

대학원 때 주로 추천시스템에 대해서 연구를 했었는데, 시간이 지나니까 다 까먹었기도 하고, 몰래(?) 이직 준비를 하고 있었는데

추천시스템 관련한 공고에 지원을 해서 나중에 미리 준비를 할 겸 다시 한번 정리하려고 한다.

우선, 추천시스템을 조금이라도 공부해봤다면 알게 되는 개념이 있는데, 이번 포스팅에서는 MF(Matrix Factorization)에 대해서 공부를 해보자!


### Matrix Factorization은 어떤 아이디어를 기반으로 했을까?

![image](https://github.com/daetamong/daetamong.github.io/assets/111731468/84772dd5-7707-44f6-92fb-ad9441c1d673)

<center>https://medium.com/@rebirth4vali/implementing-matrix-factorization-technique-for-recommender-systems-from-scratch-7828c9166d3c</center>

<br>

MF는 간단하게 말하자면 위의 그림과 같이 User(m명)와 Item(n개) 간의 평가 정보로 이루어진 하나의 $m \times n$ 행렬을 User Latent matrix와 Item Latent Matrix로 분해하는 기법을 의미한다.

다시 말해, User Latent Matrix와 Item Latent Matrix의 역행렬을 곱해서 다시 User-Item Matrix의 추측된 행렬을 구하면 원래의 근사값을 구할 수 있다는 아이디어에서 기반한 방식이다.

수식으로 보자면,

> $ \hat Ratings_{ui} = I_{i}^T \times U_{u}$

와 같이 user 행렬과 item 행렬의 내적을 통해서 "유저U의 아이템I에 대한 평가"에 관한 행렬을 다시 복구시킬 수 있게 된다.

하지만, 원본과 완벽히 동일한 결과를 얻을 수는 없다.

아래의 그림과 같이 "얘네들(=기존의 행렬 값)로 봤을 때 대충~ 이 정도의 값이면 될 거 같은데?"라는 컨셉이 Matrix Factorization이라고 보면 되겠다.

![image](https://github.com/daetamong/daetamong.github.io/assets/111731468/e5158757-4afd-4c51-8121-07b70169efb2)


그 다음, 두 행렬의 내적을 통해 완벽한 원본 행렬은 아니더라도 원본과 비슷한 형태로 값이 지정이 된다.

![image](https://github.com/daetamong/daetamong.github.io/assets/111731468/85d94a47-1a1e-4e87-bd73-dfcf4c0662de)


그리고, 위의 그림에서 $ feature1 $ 과 $ feature2 $ 로 되어 있는데, 예시에서는 2개를 지정했지만 임의로(=k개) 지정해줄 수가 있다.

다만, k값이 늘어나면 원본 행렬에 가깝도록 잘 복원할 수 있지만, 그만큼 계산량이 늘어나는 특징이 있다.

반대로 k값을 적게 하면, 원본 행렬과는 차이가 생길 수 있지만 계산량은 줄어들어 빠르게 구할 수 있다는 장점이 있다.

따라서 상황과 데이터 특성에 맞게 k값을 적절히 잘 골라서 수행하면 되겠다.


### Matrix Factorization의 장점은?

MF에 대해서 알아보기 전에 왜 한때(?) 추천시스템 분야에서 유명했었는지 장점을 알아보고 넘어가자

1. 차원축소를 통해서 계산량이 감소되고, 컴퓨팅 리소스를 절약할 수 있다.
2. 노이즈를 감소시켜 과적합을 방지할 수 있다.
3. 사용자의 패턴을 통해서 sparse matrix의 문제를 어느정도 보완할 수 있다.

세 번째 장점에 대해서 좀 더 자세히 풀어 써보자면 user와 item 간의 행렬에서는 일반적으로 모든 user가 모든 item을 듣지 않기 때문에 대부분이 0으로 되어 있다.

하지만, matrix factorization 과정을 거친다면 기존의 값들로부터 채워지지 않은 칸을 latent factor로 채우게 됨에 따라 sparse한 문제를 어느정도 해결할 수 있게 되는 것이다.

이외에도 장점이 더 있는데, 나머지는 각자 천천히 공부해보면서 나름대로 정리를 해보자


### Matrix Factorization은 무엇일까?
MF에 대해서 간단한 개념과 장점을 알아봤으니 정확히 어떤 알고리즘을 타고 있는지 알아보자.

![image](https://github.com/daetamong/daetamong.github.io/assets/111731468/2b2560fb-07ca-41cf-8ce6-456b0420546e)

위에서 Matrix Factorization의 기본적인 수식은 다음과 같다고 했었다.

> $ Ratings_{ui} = I_{i}^T \times U_{u}$

그리고 이를 다른 식으로 표현해보면 다음과 같다.

> $ \hat r_{ij} = q_{i}^T p_{j} = \sum_{k=1}^K p_{ik}q_{kj}$

두 행렬의 내적으로 통해 **아이템에 대한 사용자의 전반적인 선호도**를 예측할 수 있다.

이 식은 SVD와 굉장히 유사한 형태인데, 추천시스템에서는 SVD를 직접 사용하는 데는 한계가 있다.

위의 그림을 보면 user와 feature로 이루어진 $ P $ 행렬과 item과 feature로 이루어진 $ Q $ 행렬(전치)이 있다.

이 두 행렬을 곱하면 평점에 대한 행렬을 구할 수 있는데, 이것이 곧 matrix factorization의 컨셉이라고 말했었다.

그렇다면 우리는 두 요인벡터를 구해야 하는데, 이때 사용되는 기준(식)은 다음과 같다.

> $min \sum_{(u, i)\in K}(r_{ui} - q^Tp_{u})^2 + \lambda(\Vert q_{i} \Vert^2 + \Vert p_{u} \Vert^2)}$

이 때, K는 $r_{ui}$ 가 측정된 값일 때의 $(u, i)$ 를 의미한다.

그리고 식을 보면 규제를 더해주는데, 알려지지 않은 평점을 예측하는 것이 목적이기 때문에 과적합을 방지하기 위함이라고 보면 되겠다.

그리고 $\lambda$ 는 규제의 정도(=크기)를 의미한다. (주로 cross validation에 의해 결정된다)


### 천천히 살펴보자!
각 사용자의 아이템에 대한 행렬 계산식은 다음과 같다.

> $(e_{ij})^2 = (r_{ij} - \hat r_{ij})^2 = (r_{ij} - \sum_{k=1}^K p_{ik}q_{kj})^2$

이제, 우리가 구해야 하는 $e$ 를 최소화하는 $p와 q$ 를 구하기 위해서 보통 경사하강법을 사용하는데 각각에 대하여 편미분을 진행한다.

> $\frac{\partial}{\partial p_{ik}}e_{ij}^2 = -2(r_{ij} - \hat r_{ij})(q_{kj}) = -2e_{ij}q_{kj}$

> $\frac{\partial}{\partial q_{ik}}e_{ij}^2 = -2(r_{ij} - \hat r_{ij})(p_{ik}) = -2e_{ij}p_{ik}$

업데이트된 $p_{ik}$ 와 $ q_{ik}$ 는 다음과 같이 표현이 가능하다. 여기서 $\alpha$ 는 learning rate라고 보면 된다.

이러한 과정을 행렬에 존재하는 모든 user와 item에 대해서 오차가 최소가 될 때까지 반복한다.

그런데 아까 위에서 언급했던 것처럼 규제를 더해줌으로써 과적합을 방지한다고 했었는데,

규제를 더한 공식은 다음과 같다.

> $p_{ik}^` = p_{ik} + \alpha \frac{\partial}{\partial p_{ik}}e_{ij}^2 = p_{ik} + 2\alpha e_{ij}q_{kj}$

> $q_{ik}^` = q_{kj} + \alpha \frac{\partial}{\partial q_{kj}}e_{ij}^2 = q_{kj} + 2\alpha e_{ij}q_{ik}$

최종적으로 우리는 오차를 최소화하는 p와 q를 구하면 원하는 행렬을 구할 수가 있게 된다.

### Matrix Factorization의 장단점은..?

MF에 대한 개념과 구하는 알고리즘을 어느정도 파악을 했다.

그렇다면 마지막으로 MF에 대한 장단점을 간단하게 정리하고 마무리하고자 한다.

우선, 장점을 살펴보자!

- 장점

1. 차원축소가 가능하고, 이를 통한 컴퓨팅 리소스를 절약할 수 있다.

2. 차원축소를 통해 과적합 문제를 어느정도 해결할 수 있다.

3. feature 활용을 통해서 예측 결과에 대한 설명력을 부여할 수 있다.

- 단점

1. 계산량이 많기 때문에 대용량의 데이터에는 적합하지 않을 수 있다.

이 외에도 분명 장단점이 많겠지만, 대표적인 내용들만 정리해보았다.

추천 시스템에 관해서 아직까지도 많은 논문이 나오고 있지만 추천시스템 분야의 시초(?)인 MF도 분명 좋은 알고리즘은 틀림없는 사실인 것 같다!
<br>
<hr>

<br>
ref : https://yeong-jin-data-blog.tistory.com/entry/%EC%B6%94%EC%B2%9C-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-Matrix-Factorization

ref : https://medium.com/@rebirth4vali/implementing-matrix-factorization-technique-for-recommender-systems-from-scratch-7828c9166d3c

ref : https://greeksharifa.github.io/machine_learning/2019/12/20/Matrix-Factorization/