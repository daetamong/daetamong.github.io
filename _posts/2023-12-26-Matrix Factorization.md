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

MF는 간단하게 말하자면 User(m명)와 Item(n개) 간의 평가 정보로 이루어진 하나의 Mxn행렬을 User Latent matrix와 Item Latent Matrix로 분해하는 기법을 의미한다.

다시 말해, User Latent Matrix와 Item Latent Matrix의 역행렬을 곱해서 다시 User-Item Matrix의 추측된 행렬을 구하면 원래의 근사값을 구할 수 있다는 아이디어에서 기반한 방식이다.

그리고 PCA와 같이 차원축소의 기법으로 사용되기도 한다. 왜 그런지는 나중에 살펴보도록 하자.


### Matrix Factorization의 장점은?

MF에 대해서 알아보기 전에 왜 한때(?) 추천시스템 분야에서 유명했었는지 장점을 알아보고 넘어가자

1. 차원축소를 통해서 계산량이 감소되고, 컴퓨팅 리소스를 절약할 수 있다.
2. 노이즈를 감소시켜 과적합을 방지할 수 있다.
3. 사용자의 패턴을 통해서 sparse matrix의 문제를 어느정도 보완할 수 있다.

이외에도 장점이 더 있는데, 나머지는 각자 천천히 공부해보면서 나름대로 정리를 해보자

### SVD(Singular Value Decomposition)이란?
MF에 대해 본격적으로 알아보기 전에 중요한 개념 하나만 정리하고 넘어가려고 한다.

**"특이값 분해"**라고 하는 알고리즘인데 SVD라고도 불리며 행렬분해기법 중 하나이다.

SVD는 아래의 그림과 같이 하나의 행렬(NxM)을 3개의 행렬의 곱으로 분해하는 과정을 의미한다.

우선, 식으로 표현해보면 다음과 같다.

> $A = U \sum V^T$

여기서 3개의 행렬은 각각을 의미한다.

1. $U = m \times m 직교행렬$
2. $V = n \times n 직교행렬$
3. $\sum = m \times n 직사각 대각행렬$

### Matrix Factorization은 무엇일까?
MF에 대해서 간단한 개념과 장점을 알아봤으니 정확히 어떤 알고리즘을 타고 있는지 알아보자.

