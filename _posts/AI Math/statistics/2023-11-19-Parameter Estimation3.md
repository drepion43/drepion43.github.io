---
title:  "모수 추정3"
categories: statistics
tag: [DeepLearning,theory, python,statistics,math]
tags: [Jekyll, MathJax]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: 최소 분산 비편향 추정량
comments: true
date: 2023-11-19
toc_sticky: true
---

## 모수 추정
통계학에는 크게 추론 통계학(Inference Statistical)과 기술 통계학(Descriptive Statistical)이 존재합니다. 여기서 추론 통계학에서도 모수적 추론, 비모수적 추론과 베이지안 추론이 존재합니다.   
추론 통계학은 **표본의 정보를 통해 모집단의 모수를 추정(estimation)하고 검증(test)하는 것**입니다. 즉, 추론 통계학은 **추정(estimation)과 검정(test)**으로 구성됩니다. 여기서 추정은 표본을 추출하여 측정한 결과값을 바탕으로 모집단에 대한 측정 결과로 사용하는 것입니다. 이 추정방법에는 **점 추정(point estimation)**과 **구간 추정(interval estimation)**이 있습니다. 추정을 사용하는 가장 큰 이유는 모집단 전체를 조사할 경우 큰 비용과 시간이 들기 때문입니다.   


### 추정량
우선 추정량을 알기전에 편향이 어떤 것인지 다시 설명하겠습니다.   
편향은 하기와 같은 수식이 됩니다.   
\begin{aligned} 
Bias(\hat{\theta}) = E(\hat{\theta}) - \theta
\end{aligned}   

이제  **비편향 추정량(unbiased estimator)**과 **일치 추정량(consistent estimator)**에 대해 알아보겠습니다. 
먼저 설명했던 비편향 추정량(unbiased estimator)에 대해 간단히 설명하겠습니다. 비편향 추정량은 편향이 0인 추정량을 뜻합니다. 즉, 하기와 같은 수식을 만족하면 비편향 추정량이 됩니다.    
\begin{aligned} 
E(\hat{\theta}) = \theta
\end{aligned}  

이제 일치 추정량에 대해 알아보도록 하겠습니다. 일치 추정량을 간단하고 말하면 **표본의 크기 n이 증가함에 따라 $\hat{\theta} \rightarrow \theta$(확률 수렴)일 때, $\hat{\theta}$를 일치 추정량(consistent estimator)**이라고 합니다.   
일치 추정량은 추정량이 가져야할 바람직한 성질 중 하나입니다. 만약 어느 추정량이 일치 추정량이 아니라면, **아무리 표본의 크기를 크게 하여도 모수의 참값에 가까이 가지 않는다**는 것을 의미하며 좋은 추정량이 될 수 없습니다. 일치 추정량의 조건을 위해서는 마르코프 부등식(Markov inequality)가 필요합니다. 하기에 마르코프 부등식(Markog inequality)를 정의하겠습니다. 마르코프 부등식(Markog inequality)은 확률 분포에 대한 가정이 없어 주어지지 않은 상황에서 확률 추정할 수 있습니다.    
\- **마르코프 부등식(Markov inequality)** : 평균 정보만을 이용하여 자료가 특정 구간에 위치할 확률을 추정할 수 있는 공식   
\begin{aligned} 
P(\| X \| \ge t ) \le \frac{E(X^2)}{t^2}
\end{aligned}   
\- **마르코프 부등식 적용** : $(\hat{\theta} - \theta)$   
\begin{aligned} 
P(\| \hat{\theta} - \theta \| \ge t ) \le \frac{E(\hat{\theta} - \theta)^2}{t^2}
\end{aligned}   
이번에는 마르코프 부등식과 연관있는 쳬비셰프 부등식(Chevyshev's inequality)에 대해 정의하겠습니다. 쳬비셰프 부등식(Chevyshev's inequality) 또한 확률 분포에 대한 가정이 없어 주어지지 않은 상황에서 확률 추정할 수 있습니다. 이 쳬비셰프 부등식(Chevyshev's inequality)은 마르코프 부등식을 통해 유도할 수 있습니다.     
\- **쳬비셰프 부등식(Chevyshev's inequality)** : 평균과 분산의 정보를 함께 이용하여 보다 정확하게 확률을 추정하도록 도와주는 방법   
\begin{aligned} 
P(\| \bar{X} - \mu \| > t ) \le \frac{\sigma^2}{t^2}
\end{aligned}   
    

이제 일치 추정량의 조건을 살펴보겠습니다.    
\- 일치 추정량이 되기 위해서는 $\hat{\theta} \rightarrow \theta$(확률 수렴)가 성립해야 합니다. 즉 하기를 만족해야 합니다.   
\begin{aligned} 
lim_{n \rightarrow \infty} P(\| \hat{\theta} - \theta \| \le t ) = 0
\end{aligned}   
\- 마르코프 부등식(Markog inequality)을 통해 $E(\hat{\theta} - \theta)^2$이 0으로 수렴하는 것을 보여야 합니다.   
\- $E(\hat{\theta} - \theta)^2$를 나타내 보겠습니다. 하기와 같이 나타내면, $Var(\hat{\theta})$ 와 $Bias(\hat{\theta})$가 모두 0으로 수렴한다면 $\hat{\theta}$는 일치 추정량이 됩니다.     
\begin{aligned} 
E(\hat{\theta} - \theta)^2 =& E(\hat{\theta} - E(\hat{\theta}) + E(\hat{\theta}) - \theta )^2 \newline
=& E(\hat{\theta} - E(\hat{\theta}) )^2 + 2 E(\hat{\theta} - E(\hat{\theta}) ) E(E(\hat{\theta}) - \hat{\theta} ) + E(\hat{\theta} - \theta)^2 \newline
=&  E(\hat{\theta} - E(\hat{\theta}) )^2 + E(\hat{\theta} - \theta)^2 = Var(\hat{\theta}) + (Bias(\hat{\theta}))^2 
\end{aligned}    
   

이번에는 좀 더 쉽게 이해하기 위해 예시를 통해 일치 추정량을 확인해보겠습니다.   
$X_1, X_2, ... , X_n$이 $N(\mu, \sigma^2)$으로부터의 랜덤 샘플일 때, $\mu$와 $\sigma^2$의 최대 가능도 추정량(MLE)가 일치 추정량인지 확인해보겠습니다.   
먼저 $\mu$와 $\sigma^2$의 최대 가능도 추정량에 대해 알아보겠습니다.$\hat{\mu} = \bar{X}, \hat{\sigma^2} = \frac{1}{n} \sum_{i=1}^{n} (X_i - \bar{X})^2$입니다.   
$\mu$의 최대 가능도 추정량이 일치 추정량인지 확인해보겠습니다.    
$Bias(\hat{\mu}) = E(\hat{\mu}) - \mu = E(\bar{X}) - \mu = 0$이며, $Var(\hat{\mu}) = Var(\bar{X}) = \frac{\sigma^2}{n} \rightarrow 0$(확률 수렴)을 통해 **$Var(\hat{\theta})$ 와 $Bias(\hat{\theta})$ 모두 0으로 수렴하는 것**을 확인할 수 있습니다. 따라서, **$\bar{X}$는 일치 추정량**입니다.    
    

$\sigma^2$의 최대 가능도 추정량이 일치 추정량인지 확인해보겠습니다.    
$ \frac{\sum_{i=1}^{n} (X_i - \bar{X})^2 }{\sigma^2} \sim X^2_{n-1}(Chi) $   
상기의 성립을 통해 비슷하게 모양을 바꿔주겠습니다. 카이 제곱의 기댓값과 분산을 알 수 있습니다.   

$E(\frac{1}{\sigma^2} \sum_{i=1}^{n} (X_i - \bar{X})^2 ) = n-1, Var(\frac{1}{\sigma^2} \sum_{i=1}^{n} (X_i - \bar{X})^2 ) = 2(n-1)$   
이제 편향과 분산의 모양을 맞춰주겠습니다.   

\begin{aligned} 
Bias(\hat{\sigma^2}) =& E(\frac{1}{n} \sum_{i=1}^{n} (X_i - \bar{X})^2 ) - \sigma^2 = \frac{n-1}{n} \sigma^2 - \sigma^2 = - \frac{1}{n} \sigma^2 \newline  
Var(\hat{\sigma^2}) =& Var(\frac{\sigma^2}{n} \frac{\sum_{i=1}^{n} (X_i - \bar{X})^2}{\sigma^2} )= \frac{(\sigma^2)^2}{n^2} Var(\frac{\sum_{i=1}^{n} (X_i - \bar{X})^2}{\sigma^2}) = \frac{2(n-1)}{n^2} (\sigma^2)^2
\end{aligned}   
따라서, **$\hat{\sigma^2}$의 편향과 분산이 모두 0으로 수렴하는 것**을 확인할 수 있습니다. 따라서, **$\bar{\sigma^2}$는 일치 추정량**입니다.   

### 추정량 비교   

