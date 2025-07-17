---
layout: post
title: CrossEntropyLoss와 NLLLoss의 차이점
subtitle: CrossEntropyLoss와 NLLLoss의 차이점에 대해 알아보자!
categories: 딥러닝
tags: [딥러닝]
---

`개인적으로 공부한 내용을 포스팅하기 때문에 잘못된 내용이 있을 수 있습니다. 만약 틀린 내용이 있다면 적극적인 피드백 부탁드립니다^^`

---

### CrossEntropyLoss와 NLLLoss의 차이점에 대해 알아보자

Pytorch로 손실함수를 구할 때, CrossEntropyLoss와 NLLLoss를 사용할 때가 종종 있다.

두 함수 모두 cross-entropy를 구하는 함수인데 약간의 차이가 있다.

그 차이에 대해서 알아보자

우선 크로스-엔트로피(Cross-entropy)와 NLLL(Negative Log Likelihood Loss)에 대해서 살펴보자.

cross entropy는 예측 모형은 실제 분포인 q 를 모르고, 모델링을 하여 q 분포를 예측하고자 하는 것이다.

> $ cost(W) = -\frac1n \sum_{i=1}^{n}\sum_{j=1}^{k} y_{i}&i log(p_{j}^{i}) $

그리고 NLLLoss는 주어진 예측과 실제 레이블 간의 손실을 계산합니다.

비슷한 개념으로 Kullback-Leibler Divergence라는 것도 있는데, **KL-Divergence**는 서로 다른 두 분포의 차이를 측정하는 데 사용하는 measure이다.

이것도 추후에 정리할 예정이다.

우선 간단한 예시로 보자!

#### CrossEntropyLoss를 사용한 예시

```Python
import torch
import torch.nn as nn

logits = torch.FloatTensor([[1.2, 0.5, -0.8],
                            [-0.5, 0.8, 2.0]],
                            requires_grad = True)
labels = torch.FloatTensor([0,2])

loss_fn = nn.CrossEntropyLoss()
loss = loss_fn(logits, labels)
loss.backward()

print(loss.item())
```

#### NLLLoss를 사용한 예시

```Python
import torch
import torch.nn as nn

log_probs = torch.log(torch.tensor([[0.7, 0.2, 0.1],
                                   [0.1, 0.2, 0.7]],
                                  dtype=torch.float32))
targets = torch.tensor([0, 2])

loss_fn = nn.NLLLoss()
loss = loss_fn(log_probs, targets)
loss.backward()

print(loss.item())
```

---

코드만 봤을 때는 두 함수 모두 차이가 없어 보인다.

하지만 CrossEntropyLoss는 LogSoftmax와 NLLLoss가 함께 사용된다.

> F.CrossEntropy = F.log_softmax + NLLLos

그리고 Crossentropy를 사용한 모델이 일반적으로 더 안정적이고 효율적이다.

NLLLoss를 사용한 모델의 마지막 레이어는 log_softmax를 적용해줘야 한다.

<br>
<hr>

ref : https://wikidocs.net/60575
ref : https://velog.io/@skyhelper/CrossEntorpyLoss-NLLLoss-%EB%AC%B4%EC%97%87%EC%9D%B4-%EB%8B%A4%EB%A5%B8%EA%B0%80
ref : https://velog.io/@rcchun/%EB%A8%B8%EC%8B%A0%EB%9F%AC%EB%8B%9D-%ED%81%AC%EB%A1%9C%EC%8A%A4-%EC%97%94%ED%8A%B8%EB%A1%9C%ED%94%BCcross-entropy