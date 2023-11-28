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

일단 실습을 위한 데이터를 준비하자!

총 50개의 랜덤 데이터를 numpy를 사용해서 만들어두자

```python
x = np.random.rand(50)
```

그리고 우리가 찾고자 하는 값인 파라미터(w, b)에 대해서 초기값을 설정해주자!

```python
weight, bias = np.random.rand(2)
```

그런데 만약 이대로 선형식을 그리게 되면 일직선으로 그려질 것이다.

그래서 약간의 noise를 추가해줘서 불규칙(?)적인 데이터로 다시 만들어보자

noise 값은 uniform을 써서 -0.08과 0.08 사이의 임의값을 위에서 만들었던 데이터 크기만큼 만든다. (여기서 범위는 제 마음대로 지정했습니다)

```python
noise = np.random.uniform(-0.08, 0.08, size = x.shape)
```

이렇게 계산한 값들을 가지고 선형식을 만들어보자

```python
y = weight * x + bias + noise
```

### 그래프를 그려보자!

위에서 계산한 값들이 어떠한 분포를 띄고 있는지 보기 위해서 간단한 그래프를 그려보자

```python
plt.figure(figsize=(10, 5))
plt.scatter(x, y, label='data')
plt.show()
```

위 코드를 실행하면 아래와 같이 하나의 직선 위에 점들이 찍히는 것이 아니라 약간의 noise로 인해 오차가 생긴채로 값들이 위치하는 것을 볼 수 있다.

![image](https://github.com/daetamong/daetamong.github.io/assets/111731468/d7e51a9d-525a-4e5c-9227-715edb41a4ba)

그래서 우리는 이러한 데이터를 바탕으로 최적의 선형식을 추정해보도록 할 것이다!!!



### 초기값을 지정해주자!

위에서는 weight와 bias의 초기값을 임의로 지정했는데 나는 그냥 uniform을 써서 일정 범위 내의 임의의 값을 가져오도록 하겠다.

```python
weight = np.random.uniform(low = -1.0, high = 1.0)
bias = np.random.uniform(low = -1.0, high = 1.0)
```

그 다음, 우리는 이렇게 지정한 초기값을 바탕으로 예측값을 계산한다. 이 때의 예측값을 'y_hat'이라고 하겠다.

```python
y_hat = weight * x + bias
```

이전 포스트에서도 공부했듯이, 우리가 최적의 선형식을 찾기 위해서는 cost function이 최소화 되는 지점(기울기)을 찾아야 한다.

여기서는 MSE를 cost function으로 사용하도록 하겠다.

python 코드와의 비교를위해 다시 한번 MSE의 식을 복습 차원에서 적고 넘어가자. 

> MSE = $\frac1n(wx_i + b - y_i)^2$


```python
MSE = ((y-hat - y) ** 2).mean()
```

MSE에서 예측값과 실제값의 차이를 평균내는 것을 python의 mean함수를 사용하여 계산하였다.

그리고 초기값을 지정해줘야 할 것이 하나 더 있는데,, learning rate를 지정해줘야 한다.

나는 초기값으로 0.01을 주도록 하겠다.

그리고 epoch이라는 것도 지정을 해줄 건데, 여기서 epoch은 몇번 학습을 할 것인가에 대한 옵션이라고 보면 되겠다. 나는 50회 정도 학습하도록 지정을 했다.

```python
learning_rate = 0.01
epoch = 1000
```

이제 모든 초기값을 지정해줬으니, 계산을 한번 해보자!



### 최적의 선형식을 추정해보자!!

```python
errors = []
for epo in range(epoch):
    y_hat = weight * x + bias

    MSE = ((y_hat - y) ** 2).mean()
    if MSE < 0.0005:
        break

    weight = weight - learning_rate * ((y_hat - y) * x).mean()
    bias = bias - learning_rate * (y_hat - y).mean()

    errors.append(MSE)

    if epo % 5 == 0:
        print(f'weight : {weight} // bias : {bias} // MSE : {MSE}')
```

위의 코드에서 error을 빈 리스트로 반환했는데, 한번 학습을 할 때마다 생기는 mse값을 담기 위한 리스트라고 보면 되겠다.

그리고 for문을 통해 앞에서 지정한 1000회를 학습하도록 지정한다.

그리고 예측값과 실제값의 차이를 계산한 MSE를 구하고, 만약 MSE값이 0.0005보다 작으면 stop, 크면 계속 학습 진행하도록 한다.

weight와 bias는 global optimum에 근접하게끔 매 epoch마다 계속 업데이트 해준다.

최종 출력은 매 epoch마다 weight와 bias 그리고 MSE가 어떤 값을 가지는지 출력한다.

![image](https://github.com/daetamong/daetamong.github.io/assets/111731468/527a0cba-86bf-4051-aff1-dd0db55213cc)

이런식으로 MSE가 0에 가까워지도록 계속 학습이 되는 것을 볼 수 있다.

이렇게 보면 값들이 어떻게 변화하는지 직관적으로 알아보기가 어려우니까!

그래프로 한 번 그려보자!

```python
plt.figure(figsize=(10, 5))
plt.plot(errors)
plt.xlabel('Epochs')
plt.ylabel('Error')
plt.show()
```

![image](https://github.com/daetamong/daetamong.github.io/assets/111731468/f3fab178-b0af-4c94-980d-ce9a38987c9f)

위의 그림처럼 학습을 많이 할수록 error값이 계속 줄어드는 것을 확인할 수 있다.

아마 epoch을 더 늘리면 거의 0에 수렴하지 않을까?라는 생각이 들지만 귀찮으니 pass!


<br>
<hr>


ref : https://teddylee777.github.io/scikit-learn/gradient-descent/