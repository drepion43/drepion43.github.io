---
title:  "모수 추정2"
categories: statistics
tag: [DeepLearning,theory, python,statistics,math]
tags: [Jekyll, MathJax]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: 최대 가능도 추정법
comments: true
date: 2023-11-17
toc_sticky: true
---

## 모수 추정
통계학에는 크게 추론 통계학(Inference Statistical)과 기술 통계학(Descriptive Statistical)이 존재합니다. 여기서 추론 통계학에서도 모수적 추론, 비모수적 추론과 베이지안 추론이 존재합니다.   
추론 통계학은 **표본의 정보를 통해 모집단의 모수를 추정(estimation)하고 검증(test)하는 것**입니다. 즉, 추론 통계학은 **추정(estimation)과 검정(test)**으로 구성됩니다. 여기서 추정은 표본을 추출하여 측정한 결과값을 바탕으로 모집단에 대한 측정 결과로 사용하는 것입니다. 이 추정방법에는 **점 추정(point estimation)**과 **구간 추정(interval estimation)**이 있습니다. 추정을 사용하는 가장 큰 이유는 모집단 전체를 조사할 경우 큰 비용과 시간이 들기 때문입니다.   

### 가능도(Likelihood)
이번에 회귀 분석, AI 분야에서 가장 중요한 <span style='color:red'>**"가능도"**</span>에 대해 알아보도록 하겠습니다. 우선 가능도에대해 알아보기전에 확률의 개념을 다시 짚어보겠습니다.   

#### 이산 확률 변수의 확률
가장 많이 사용되는 예시인 주사위 예시를 들어보겠습니다. 주사위를 던져서 나올 수 있는 수는 1,2,3,4,5,6이고 각 숫자가 나올 확률은 모두 $\frac{1}{6}$이 됩니다. 이번에는 동전 던지기를 생각해보겠습니다. 동전을 5번 던져 앞면이 나올 경우의 수는 0번, 1번, 2번, 3번, 4번, 5번의 총 6가지의 경우가 존재합니다. 각 사건이 나올 확률은 $\frac{1}{2^5}$, $\frac{5}{2^5}$, $\frac{10}{2^5}$, $\frac{10}{2^5}$, $\frac{5}{2^5}$, $\frac{1}{2^5}$이 됩니다. 이 두 경우 모두 확률의 합은 <span style='color:red'>**1**</span>이 됩니다.

#### 연속 확률 변수의 확률
상기의 예시는 이산 확률 변수에 대한 얘기였습니다. 따라서, 특정 사건에 대한 확률을 딱 정의할 수 있었습니다. 하지만, 연속 확률 변수의 경우에는 특정 사건에 대한 확률을 정의할 수 없습니다.

예시를 들어 보겠습니다. 1 ~ 10사이의 숫자 중 랜덤하게 숫자를 뽑는다고 하겠습니다. 이 때 우리가 원하는 숫자가 3일경우, 3을 정확하게 뽑을 확률을 정의할 수 있을까요? 1 ~ 10사이에는 정말 무한한 수가 존재합니다. 그 수들 중에 3을 뽑는다는 확률은 곧 $\frac{1}{\infty}$가 됩니다. 이 값은 0을 의미합니다. 따라서, 이런 연속 확률 변수에 대해서는 **확률 밀도 함수(pdf)**의 구간의 넓이를 통해 확률을 정의합니다. 확률 밀도 함수(pdf)의 적분값을 이용해 확률을 정의합니다. 

#### 확률과 가능도(Probability & Likelihood)
이제 앞서 확률에 대해 설명했으니, 다시 가능도로 돌아와 보도록 하겠습니다. 연속된 사건에 대해서 특정 사건이 일어날 확률은 0으로 계산된다고 했습니다. 따라서, 사건들이 일어날 가능성을 비교하는 것은 불가능하다고 할 수 있습니다. 이를 비교할려면 **가능도**라는 개념이 필요하게됩니다. 그럼 이전 이산 확률 변수의 주사위 던지기를 통해 설명해보겠습니다. 주사위를 던져 1이 나올 확률이 $\frac{1}{6}$이라고 했습니다. 그럼 여기서 그래프를 그린다면 y값이 확률이 됩니다. 그럼 이 y값을 가능도라고 생각해보겠습니다. 그럼 y값(확률)이 높을수록 해당 사건인 1이 나올 확률의 가능성이 올라간다고 생각할 수 있습니다. 따라서, 이산 확률 변수의 경우 **가능도==확률**을 만족한다고 할 수 있습니다.   
이번에는 연속 확률 변수에 대해 설명해보겠습니다. 연속 확률 변수의 확률 그래프의 y값은 pdf를 뜻합니다. 그럼 해당 y값이 높을 수록 특정 구간에서 사건이 일어날 가능성이 높아진다고 볼 수 있습니다. 즉, 연속 확률 변수에서는 **가능도==pdf**를 만족한다고 할 수 있습니다.    

이제 정리해보겠습니다.   
확률은 주어진 확률 분포가 있을 때, 관측값 혹은 관측 구간이 분포안에서 얼마의 확률로 존재하는지를 나타내는 값입니다. 즉, **확률 분포가 주어졌을 때, 관측값에 대한 확률**을 구한다고 볼 수 있습니다. 수식으로 나타내면 하기와 같이 나타낼 수 있습니다.   
확률 = P(관측값 | 확률 분포)   
가능도는 어떤 값이 관측되었을 때, 해당 관측값이 어떤 확률분포로부터 나왔는지에 대한 확률입니다. 즉, 확률과 반대로 고정된 값이 **관측값**이라고 볼 수 있습니다. 수식으로 나타내면 하기와 같이 나타낼 수 있습니다.   
가능도 = L(확률 분포| 관측값)   
\begin{aligned} 
L(\theta) = f(x_1 \| \theta)f(x_1 \| \theta)f(x_2 \| \theta)f(x_3 \| \theta)...f(x_n \| \theta)
\end{aligned}    
쉽게 말해, 확률은 관측값을 추정, 가능도는 확률 분포를 추정한다고 생각하면 됩니다. 가능도는 주어진 관측값 $x$에 대한 함수를 $\theta$에 대한 함수로 바꾼 것이라고 볼 수 있습니다.   

여기서 로그 가능도(log-likelihood)는 가능도 함수에 log를 취해준 수식입니다. 왜 log를 취해주냐하면, 대부분의 함수가 지수 함수로 표현되며 지수 함수의 경우 증가 함수이기 때문에 영향을 끼치지 않습니다. 그럼 가능도의 함수가 multiply에서 plus로 바뀌어 계산이 더 용이해지며, 우리가 구하고자 했던 결과가 바뀌지 않으며 구할 수 있습니다.   
\begin{aligned} 
l(\theta)=log \; L(\theta) = \sum_{i=1}^{n} log \; f(x_i \| \theta)
\end{aligned}   

### 최대 가능도 추정(MLE) 
모수 추정에서 가장 많이 쓰이는 최대 가능도 추정에 대해 알아보겠습니다. 이전에 설명했던 가능도함수에 대해 어느정도 이해 하셨을 거라고 생각합니다. 다시 한번더 가능도 함수에 대해 요약하자면, **주어진 자료들($x_1, x_2, x_3, ..., x_n$)를 통해 확률 밀도 함수(pdf)의 $\theta$를 추정하는 함수**입니다. 그럼 여기서 최대 가능도 추정(MLE)란 **이 확률 밀도 함수(pdf)를 가장 크게 해주는 $\theta$을 모수의 추정값으로 정하는 방법**이라고 할 수 있습니다. 즉, 하기와 같이 나타낼 수 있습니다.   
$\hat{\theta} = {arg \; max}_{\theta} \; L(\theta)$   

보통 **가능도 를 최대로 하는 $\theta$와 로그 가능도 함수를 최대로 하는 $\theta$**와 동일하여 로그 가능도를 이용해 추정합니다. 로그 가능도를 어떻게 추정하는지에 대해 알아보겠습니다.   
\- 로그 가능도는 보통 오목 함수(concave function)로 $\theta$에 대해 미분 가능한 함수인 경우가 많습니다.   
\- 오목 함수(concave function)이며 미분 가능한 경우 보통 **미분 값이 0인 지점이 최대**가 됩니다.   
\- 오목 함수(concave function)일 1가지 충분 조건은 **두 번 미분한 함수의 값이 항상 음수**일 때 입니다.($f''(x) < 0$)   
즉, $\frac{d}{d \theta} l(\theta) = \frac{d}{d \theta} \sum_{i=1}^{n} log \; f(x_i \| \theta)=0$인 $\theta$를 구하면 됩니다.   

예시 문제를 풀어보겠습니다.   
$X_1, X_2, X_3, ..., X_n$이 확률 밀도 함수 $f(x \| \theta) = \frac{1}{2 \theta} e^{- \frac{\| x \| }{\theta}}, \; - \infty < x < \infty$의 랜덤 샘플일 때, $\theta$의 최대 가능도 추정량을 구해보겠습니다.    
$l(\theta) = \sum_{i=1}^{n} log \; f(x_i \| \theta) = -n \; log(2 \theta) - \frac{\sum_{i=1}^{n} \| x_i \|}{\theta}$가 됩니다. 해당 로그 가능도 함수는 concave function이며 미분 가능합니다. $\theta$에 대해 미분하여 0이 되는 $\theta$값이 최대 가능도 추정량의 값이 됩니다.   
$\frac{d}{d \theta} l(\theta) = - \frac{n}{\theta} + \frac{\sum_{i=1}^{n} \| x_i \|}{\theta^2}$으로, $\hat{\theta} = \frac{1}{n} \sum_{i=1}^{n} \| X_i \|$이 됩니다. 

이번에는 모수가 2개인 경우에 대한 최대 가능도 추정량(MLE)를 구해보겠습니다. 이전과 동일하게 로그 가능도 $l(\theta_1, \theta_2)$가 **미분 가능**하고 **concave function**일 때, 하기를 만족하는 $\hat{\theta_1}, \hat{\theta_2}$를 구해주면 됩니다.   
 $\begin{Bmatrix} \frac{d}{d \theta_1} l(\theta_1, \theta_2) = 0 \newline \frac{d}{d \theta_2} l(\theta_1, \theta_2) = 0 \end{Bmatrix}$   
 이번엔 **concave function**인지에 대한 충분 조건을 확인해보겠습니다. 하기와 같이 로그 가능도 함수를 두번 미분한(Hessian matrix)이 음의 정부호 행렬(negative definite matrix)이면 됩니다.   
$\begin{bmatrix} 
\frac{d^2}{d \theta_1^2} l(\theta_1, \theta_2) & \frac{d^2}{d \theta_1 \; d \theta_2} l(\theta_1, \theta_2) \newline
\frac{d^2}{d \theta_2 \; d \theta_1} l(\theta_1, \theta_2) &  \frac{d^2}{d \theta_2^2} l(\theta_1, \theta_2)
\end{bmatrix}$   

예시 문제를 풀어보겠습니다.   
$X_1, X_2, X_3, ..., X_n$이 $N(\mu, \sigma^2)$의 랜덤 샘플일 때, $\theta=(\mu, \sigma^2)$의 최대 가능도 추정량을 구해보겠습니다.    
$l(\mu, \sigma^2) = - \frac{n}{2} log(2 \phi \sigma^2) - \frac{1}{2 \sigma^2} \sum_{i=1}^{n} (x_i - \mu)^2$이 돕니다. Hessian matrix가 음의 정부호 행렬이니 각 미분을 통해 최대 가능도 추정량을 구할 수 있습니다.   
$\frac{d}{d \mu} l(\mu, \sigma^2) = \frac{\sum_{i=1}^{n} (x_i - \mu)}{\sigma^2}, \frac{d}{d \sigma^2} l(\mu, \sigma^2) = - \frac{n}{2 \sigma^2} + \frac{\sum_{i=1}^{n} (x_i - \mu)^2}{2 (\sigma^2)^2}$으로, $\hat{\mu} = \bar{X}, \hat{\sigma^2} = \frac{1}{n} \sum_{i=1}^{n} (X_i - \bar{X})^2$이 됩니다.   

#### Invariance Property   
최대 가능도 추정량(MLE)의 성질 중 **가장 좋은 불변 성질(Invariance Property)**에 대해 알아보도록 하겠습니다. Invariance Property란 **$\theta$의 최대 가능도 추정량이 $\hat{\theta}$일 때, $h(\theta)$의 최대 가능도 추정량 또한 $h(\hat{\theta})$가 됩니다.**즉, $\hat{\mu} \rightarrow \bar{X}$라면, $\hat{\mu^2} \rightarrow (\bar{X})^2$가 된다는 뜻입니다.   

증명을 해보도록 하겠습니다.   
우선, $h(\theta)$가 일대일 함수($\theta = h^{-1}(\eta)$)라고 가정하겠습니다.   
$\eta = h(\theta)$라 할 때, $\eta$로 나타낸 로그 가능도 $m(\eta)$는 $m(\eta) = l(\theta) = l(h^{-1}(\eta))$가 됩니다.   
$\eta$의 최대 가능도 추정량(MLE)은 $m(\eta)$를 초대로 하는 $\eta$이므로 $\hat{\eta} = arg \; max_{\eta} \; m(\eta) = arg \; max_{\eta} \; l(h^{-1}(\eta))$가 됩니다.   
$l(\theta)$는 $\theta = \hat{\theta}$일 때 최대가 되므로, $m(\eta)$는 $h^{-1}(\eta)= \hat{\theta}$일 때 최대가 됩니다. 따라서 $\hat{\eta} = h(\theta) = h(\hat{\theta})$가 됩니다. 

Invariance Property에 의하여 $\theta$의 최대 가능도 추정량(MLE)를 알수만 있다면, 함수 $h(.)$가 무엇이든 $h(\theta)$의 최대 가능도 추정량(MLE) 또한 쉽게 알 수 있습니다. 