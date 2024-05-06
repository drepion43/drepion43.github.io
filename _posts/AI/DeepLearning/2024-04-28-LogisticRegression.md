---
title:  "Logistic Regression"
categories: DeepLearning
tag: [theory, AI, DeepLearning]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: 
comments: true
date: 2024-04-28
toc_sticky: true
---

## Sigmoid Function
Logistic Regression을 들어가기전에 Logistic Regression에서 activation function으로 사용하는 sigmoid 함수에 대해 먼저 살펴보겠습니다.   
<img src="../../../assets/images/DeepLearning/2024-04-28-LogisticRegression/sigmoid function.png" alt="sigmoid function" style="zoom:80%;" />    
상기의 이미지에 있는 함수가 sigmoid 함수의 그래프입니다. 이미지에서 볼 수 있듯이 sigmoid는 0~1사이의 값을 가집니다. 즉, activation function으로 sigmoid을 이용하게 되면 그 activation function을 거친 후 값은 0~1사이의 값으로 범위가 줄어들게 됩니다. 즉, 확률적으로 접근이 가능해지게 됩니다. 그럼 이번엔 sigmoid 함수의 수식을 하기에 정의하겠습니다.   
\begin{aligned} 
y = \frac{1}{1 + e^{-x}} = \frac{e^x}{1 + e^x}
\end{aligned}   

상기의 수식에서 한 번에 $x \rightarrow \infty$로 가면 sigmoid 함수는 1로 수렴하는 것을 알 수 있습니다. 또한, $x \rightarrow - \infty$ 보낸다면 0으로 수렴합니다. 그럼 $x \rightarrow 0$이라면 $\frac{1}{2}$로 수렴하는 것을 알 수 있습니다. 이런 sigmoid 값의 범위 때문에 sigmoid를 쓰는 이유는 이전에 말했듯이 **확률적 접근**이 가능하기 때문입니다.

## Logistic Regression
그럼 이제 Logisitc Regression에 대해 알아보겠습니다. Logisitc Regression은 activation function으로 sigmoid function을 쓴다고 계속 얘기드렸습니다. sigmoid function을 쓰는 궁극적인 목표는 **non-linear**를 표현하기도 하면서 또한 **확률적 접근**이 가능해지기 때문이라고 했습니다. 그럼 Logistic Regression은 이런 확률적 접근을 통해 데이터가 어떤 카테고리에 속하는지를 0~1사이의 확률값을 출력으로 예측하는 Regression 알고리즘이 됩니다. 즉, 이 확률을 기반으로 "데이터가 어떤 카테고리에 속할 확률이 xx%이다"라고 말을 할 수 있게 됩니다. 하지만, Logisitc은 sigmoid function을 사용하기 때문에 **두 가지의 카테고리에 대한 확률만을 정의**할 수 있습니다. 예를 들어, 동전의 앞면이 나올확률, 뒷면이 나올확률 또는 강아지일 확률 강아지가 아닐 확률 등과 같은 것을 의미합니다. 즉, 두가지 class에 대한 <span style='color:blue'>**binary classification**</span>만 가능하다는 말입니다.   

### Likelihood
Likelihood는 통계학 시간에서 많이 배우셨을 겁니다. 이번 절에서는 Likelihood를 매우 간단하게 다룰 것이며 조금 더 자세히 알고 싶으시다면 [Likelihood](https://drepion43.github.io/statistics/Parameter-Estimation2/) 해당 링크에서 확인해보시면 될 것 같습니다. 우선 likelihood는 조건부 확률에서 **확률 분포가 주어졌을 때, 관측값(데이터)에 대한 확률**을 나타내는 것입니다. 즉, 확률은 확률 분포를 통해 관측값에 대한 확률 추정하는 것이었다면 likelihood는 관측값을 통해 확률 분포를 추정한다고 이해하시면 됩니다.    
<br>
그럼 분류 문제의 예시를 들어 계속 설명해보겠습니다.   
Logisitc Regression을 통해 동물을 분류하고 싶다고 해보겠습니다. 그럼 관측값(데이터)로 동물의 사진을 주어줘야할 것입니다. 보통 사진은 2D로 이루어진 Matrix라고 생각하실 수 있으실 겁니다. 그럼 이 2D Matrix를 1D인 column으로 잘라서 아래로 쭉 이어붙였다고 가정해보겠습니다. 그럼 rank-1 matrix인 vector로 변형이 가능할 것 입니다. 이 데이터를 input으로 줄 때, 강아지 사진은 label인 $Y=1$이고 나머지 강아지가 아닌 동물들에 대해서는 $Y=0$이라고 labeling을 했다고 가정해보겠습니다. 그럼 이 강아지일 확률은 $P(Y | \underline{X})$가 될 것이며, 이 때의 강아지일 확률에 대한 확률은 $P(Y=1 | \underline{X})$의 값을 가지는 것을 알 수 있을 것입니다. 그럼 강아지가 아닌 동물의 확률은 $P(Y=0 | \underline{X})$인 것을 알 수 있습니다. 여기서 강아지일 확률가 강아지가 아닐 확률을 나눠준 것이 바로 **Odds**가 됩니다. 그럼 Odds의 수식을 하기와 같이 정의해보겠습니다.   
\begin{aligned} 
Odds =& \frac{P(X)}{1 - P(X)} \newline   
=& \frac{P(Y=1 | X)}{P(Y=0 | X)} \newline   
=& \frac{P(Y=1 | X)}{1 - P(Y=1 | X)}   
\end{aligned}   

### Odds Ratio
그럼 최종적으로 이 확률 $P(Y | X)$는 어떻게 구해지는 알아보겠습니다. Logistic Regression은 마지막 layer의 activation function을 sigmoid를 이용하여 확률적 접근을 가능하게 한다고 했습니다. 그럼 그 이전의 Neural Network에 대한 함수를 $f(\underline{x}, \underline{w})$라고 하겠습니다. 이 $f(\underline{x}, \underline{w})$는 입력 데이터 $\underline{x}$에 대해 weighted sum을 수행하는 함수라고 했습니다. 그럼 최종적으로 확률을 내려면 $sig(f(\underline{x}, \underline{w}))$이 이루어져야하며 이 결과가 곧 $P(Y=1 | X)$인 강아지일 확률을 의미하게 될 것 입니다. 그럼 이와 반대인 강아지가 아닐 확률은 $P(Y=0 | X) = 1 - P(Y=1 | X) =1 - sig(f(\underline{x}, \underline{w}))$이 될 것입니다.    

### Loss fucntion
그럼 최종적으로 얻은 이 확률은 가능도함수라고 했습니다. 가능도함수는 예상되는 확률 분포들에 대해 모두 곱한 값입니다. 즉, $Likelihood=P(Y=0 | X)P(Y=1 | X)$라는 의미가 됩니다. 이를 조금더 일반화해서 나타내면 $Loss = \Pi_i^2 P(Y_i | X_i)$가 됩니다. 하지만 이 곱셈의 연산의 경우 연산 비용이 너무 큽니다. 그래서 곱셈 연산을 덧셈연산으로 바꿀 수 있다면 연산 비용이 굉장히 줄어들 것 입니다. 그래서 likelihood에서도 보통 그냥 likelihood를 쓰기보다는 log likelihood를 이용한다고 이전에 말씀드렸습니다. 근데 log를 취해주기 위해서는 함수가 **monotonic**한 함수에 대해서만 log를 취해주어도 우리가 구하고자 하는 최대값을 가지는 $x$값이 바뀌지 않고 찾을 수 있습니다. 해당 함수의 경우에는 monotonic하기 때문에 적용이 가능합니다. 따라서 log를 취해줄 수 있는데, 추가적으로 보통 Loss function이라 함은 해당 함수의 값을 최소화를 해주는 것이 목적입니다. 하지만 현재 $Loss = \Pi_i^2 P(Y_i | X_i)$는 값을 최대화를 해주는 것이 목적이기 때문에 여기에 음수부호를 붙여준다면 우리가 구하고자 하는 $P(Y | X)$의 함수를 최대화해도 Loss함수는 최소화가 될 것입니다. 따라서 최종적으로 $Loss = -\Pi_i^2 P(Y_i | X_i) = -\sum_i P(Y_i | X_i)$라는 Logistic Regression의 Loss함수를 얻을 수 있습니다.   
참고로 이전에 Logistic Regression은 binary classification이라고 했습니다. 따라서 $Loss = -sum_i (P(Y_i=1 | X_i) + (1 - P(Y_i=0 | X_i))$로 나타내도 같은 의미가 됩니다. 