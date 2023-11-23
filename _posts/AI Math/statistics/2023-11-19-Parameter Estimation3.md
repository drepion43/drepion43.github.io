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

\begin{aligned} 
E(\frac{1}{\sigma^2} \sum_{i=1}^{n} (X_i - \bar{X})^2 ) =& n-1  \newline  
Var(\frac{1}{\sigma^2} \sum_{i=1}^{n} (X_i - \bar{X})^2 ) =& 2(n-1)
\end{aligned}
이제 편향과 분산의 모양을 맞춰주겠습니다.   

\begin{aligned} 
Bias(\hat{\sigma^2}) =& E(\frac{1}{n} \sum_{i=1}^{n} (X_i - \bar{X})^2 ) - \sigma^2 = \frac{n-1}{n} \sigma^2 - \sigma^2 = - \frac{1}{n} \sigma^2 \newline  
Var(\hat{\sigma^2}) =& Var(\frac{\sigma^2}{n} \frac{\sum_{i=1}^{n} (X_i - \bar{X})^2}{\sigma^2} )= \frac{(\sigma^2)^2}{n^2} Var(\frac{\sum_{i=1}^{n} (X_i - \bar{X})^2}{\sigma^2}) = \frac{2(n-1)}{n^2} (\sigma^2)^2
\end{aligned}   
따라서, **$\hat{\sigma^2}$의 편향과 분산이 모두 0으로 수렴하는 것**을 확인할 수 있습니다. 따라서, **$\bar{\sigma^2}$는 일치 추정량**입니다.   

### 추정량 비교   
우리가 이전에 배웠던 적률 추정법, 가능도 추정법 등 추정법을 이용하면 한 개의 추정량만 나오는게 아닌 여러개의 추정량이 나옵니다 이 중에 가장 모수를 잘 추정한다고 생각되는 추정량 $\theta$를 선택하게 됩니다. 그렇다면 이 여러개가 존재하는 $\theta$의 추정량 중 어떤 것이 가장 우수하다고 할 수 있을까요? 정답은 다들 알고 계시겠지만, 당연히 모수인 $\theta$에 가장 가까운 값을 가지는 추정량이 좋은 추정량이 됩니다. 하기와 같이 정리해보겠습니다.   
$\theta$의 값을 정확하게 알지 못하기 때문에 추정량이 $\theta$에 얼마만큼 가까운지 정확히 판단을 할 수가 없습니다. 따라서 추정량의 비교는 확률적으로 할 수 밖에 없어집니다. 즉, **어떤 추정량이 다른 추정량에 비해 $\theta$에 가까이 있을 확률이 높을 때** 더 우수한 추정량이라 할 수 있습니다. ($\| \hat{\theta} - \theta \|$값이 작을수록 더 우수한 추정량이라고 할 수 있습니다.)    

#### 비편향 추정량의 비교(분산)
비편향 추정량(unbiased estimator)의 경우에는 분산이 작을수록 우수한 추정량이라 할 수 있습니다. &rarr; 비편향 추정량의 경우 평균은 모평균과 동일하니 분산을 확인하면 됩니다. 분산이 작다는 뜻은 표본 데이터들이 평균에 몰려있다는 뜻이되니 더 $\theta$를 더 잘 나타내고 있다고 할 수 있습니다.   

$\hat{\theta}$와 $\tilde{\theta}$가 모두 $\theta$에 비편향 추정량일시 하기와 같은 분산의 비율을 통해 비교할 수 있습니다. $\tilde{\theta}$에 대한 $\hat{\theta}$의 효율(efficiency)라고 합니다.   
\begin{aligned} 
eff(\hat{\theta}, \tilde{\theta}) = \frac{Var(\tilde{\theta})}{Var(\hat{\theta})}
\end{aligned}  
두 비편향 추정량 $\hat{\theta}$와 $\tilde{\theta}$에 대하여 $eff(\hat{\theta}, \tilde{\theta}) > 1$이면 $\hat{\theta}$의 분산이 $\tilde{\theta}$의 분산보다 작다는 것을 의미하므로 $\hat{\theta}$가 더 좋은 추정량이라 할 수 있습니다.   

#### 평균제곱오차(MSE)
AI나 회귀분석을 조금 공부해본 사람이라면 이 MSE를 모를 수가 없다고 생각합니다. 가장 쉬우면 가장 가장 중요한 MSE같은 경우에는 가장 일반적인 추정량 비교 기준이 됩니다. 다들 알고 계시겠지만, 간략하게 MSE에 대해 정의해보겠습니다.   
**추정량 $\hat{\theta}$와 모수 $\theta$의 차이의 제곱에 대한 기댓값**이 MSE가 됩니다. $MSE(\hat{\theta})$로 나타냅니다. 하기에 수식으로 정의해보겠습니다.    
\begin{aligned} 
MSE(\hat{\theta}) = E(\hat{\theta} - \theta)^2
\end{aligned}   


이름 그대로 평균제곱오차이므로 값이 작을수록 추정량이 모수에 가까이 있다고 할 수 있으므로 더 좋은 추정량이라고도 할 수 있습니다.   
여기서 $E(\hat{\theta} - \theta)^2$이 수식은 어디서 본적이 있지 않으신가요? 바로 위에서 일치 추정량의 조건을 알아볼 때 나왔던 수식입니다. 즉, 일치 추정량은 MSE값이 0으로 수렴하면 된다는 뜻입니다. 또한, MSE는 이전에 봤듯이 분산과 편향으로도 나타낼 수 있습니다. 하기에 나타내보겠습니다.    
\begin{aligned} 
MSE(\hat{\theta}) = E(\hat{\theta} - \theta)^2 = Var(\hat{\theta}) + (Bias(\hat{\theta}))^2
\end{aligned}   
즉, 분산과 편향의 제곱의 합이 됩니다. 이 말은 **분산이 작으며, 편향이 작을수록** 우수한 추정량이라는 뜻입니다. 그럼 편향이 0인 비편향 추정량(unbiased estimator)의 경우 $MSE(\hat{\theta}) = Var(\hat{\theta})$인 MSE값이 분산과 같아집니다.    

### 최소분산 비편향 추정량의 방법
#### $\frac{d}{d \theta} log \; f(X \| \theta)$ 성질
$\frac{d}{d \theta} log \; f(X \| \theta)$의 성질을 알기 위한 조건을 우선 알아보겠습니다. 하기의 조건은 지수족에 속하는 확률 분포들은 만족합니다.   
① $log \; f(x \| \theta)$가 $\theta$에 대해 미분 가능해야합니다.   
② $log \; f(x \| \theta)$에서 적분과 미분 순서를 바꿀 수 있어야합니다.   
   
그럼 이제 성질을 알아보겠습니다.   
\- $E(\frac{d}{d \theta} log \; f(X \| \theta) ) = 0$   
\- $E(\frac{d}{d \theta} log \; f(X \| \theta) )^2 = - E(\frac{d^2}{d \theta^2} log \; f(X \| \theta) ) $   

#### 피셔 정보수(Fisher information)
우선 피셔 정보수를 알기 전에 **스코어 함수(Score function)**에 대해 간략히 설명하겠습니다.   

**스코어 함수(Score function)**   
스코어 함수는 **log-likelihood의 1차 미분 값**입니다.  
\begin{aligned} 
S(\theta) = \frac{\delta \; log \; f(x \| \theta)}{\delta \theta}
\end{aligned}   
여기서 최대 가능도 추정량(MLE)는 $sum_{i=1}^{n} \frac{\delta log \; f(x_i \| \theta)}{\delta \theta} = 0$이 되는 $\hat{\theta}$를 구하면 됩니다.   

최대 가능도 추정량을 보면, 해당 값들의 합이 0으로 갑니다. 이 뜻은 즉, 스코어 함수의 값이 0에 가깝다는 것은 $\theta$가 변함으로써, 가능도 함수의 변화에 끼치는 영향이 매우 작다는 말이됩니다. 그럼 반대로 **$\theta$가 얼마나 영향을 미치는지**도 알아볼 수 있을 것 같다고 생각합니다. $E(\frac{\delta log \; f(X \| \theta)}{\delta \theta} )$을 통해 $\theta$가 얼마나 정보를 주는지 알아보려고 합니다. 하지만, 해당 스코어 함수 $\frac{\delta log \; f(x \| \theta)}{\delta \theta}$에는 음수와 양수가 함께 존재하기 때문에, **변화하는 양**을 알아보기는 힘들다고 생각합니다. 따라서, 피셔 정보수 $I(\theta)$를 통해 알아볼 수 있습니다.   
\begin{aligned} 
I(\theta) = E( (\frac{\delta log \; f(X \| \theta)}{\delta \; \theta})^2 ) = - E(\frac{\delta^2}{\delta \theta^2} log \; f(X \| \theta) )
\end{aligned}    

만약, 비편향 주정량에 대한 피셔 정보수는 분산의 합과 같아집니다.   
\begin{aligned} 
I(\theta) =& E( (\sum_{i=1}^{n} \frac{\delta \; log \; f(x_i \| \theta)}{\delta \; \theta})^2 ) \newline
        =& Var(\sum_{i=1}^{n} \frac{\delta \; log \; f(x_i \| \theta)}{\delta \; \theta})^2) \newline
        =& \sum_{i=1}^{n} Var( \frac{\delta \; log \; f(x_i \| \theta)}{\delta \; \theta})^2)
\end{aligned}    

#### 코시-슈바르츠 부등식(Cauchy-Schwarz inequality)
임의의 두 확률 변수 $U$와 $V$에 대해 다음 부등식이 성립합니다. 
\begin{aligned} 
( Cov(U, V) )^2 \le Var(U)Var(V) \Leftrightarrow (Cor(U, V))^2 \le 1
\end{aligned}   
해당 부등식을 증명해보겠습니다.   
분산은 항상 0보다 크거나 같으므로 모든 실수 a에 대하여 다음이 성립합니다.   
\begin{aligned} 
Var(U + aV) = Var(U) + 2aCov(U, V) + a^2Var(V) \ge 0
\end{aligned}   
상기의 식을 a에대한 2차 다항시긍로 생각하면 항상 0보다 크거나 같기 위해서는 판별식 $D \le 0$을 만족하면 됩니다.   
\begin{aligned} 
\frac{D}{4} = ( Cov(U, V) )^2 - Var(U)Var(V) \le 0
\end{aligned}   

#### 크래머-라오 부등식(Cramer-Rao's inequality)
크래머-라오의 정보 부등식(Cramer-Rao's inequality)은 최소분산 비편향 추정량을 구하는 방법 중 하나입니다. 크래머-라오 부등식을 가정하려면 **정칙 조건**이 필요합니다. 하기에 정칙 조건들에 대해 소개하겠습니다.   
① 확률 밀도 함수(pdf) $f$는 $\theta$에 대해 단사입니다. 즉, 모수가 달라짐에 따라 예측하고자 하는 모집단의 분포의 pdf가 달라집니다. 하기에 수식으로 정의하겠습니다.    
\begin{aligned} 
\theta \neq \theta^{\prime} \Longrightarrow f(x_k \| \theta) \neq f(x_k \| \theta^{\prime})
\end{aligned}
② 확률 밀도 함수 $f$ 는 모든 $\theta$에 대해 같은 서포트를 가집니다.   
③ 참값 $\theta_0$는 $\Omega$의 내점입니다. 즉, $\theta_0$(true parameter 즉 우리의 목표)은 $\Omega$(모수 집합)안에 존재합니다.   
④ 확률 밀도 함수 $f$는 두 번 미분이 가능합니다.   
⑤ 적분과 미분 순서를 바꿀 수 있습니다.   

크래머-라오 부등식은 랜덤 샘플 $X_1, X_2, ..., X_n$을 이용한 $\theta$의 비편향 추정량 $T= T(\mathsf{X})$에 대하여 상기의 정칙 조건(①~⑤)을 만족하면 하기의 부등식을 성립합니다.($T(\mathsf{X})$ : $\theta$의 비편향 추정량)   
\begin{aligned} 
Var(T) \ge \frac{1}{nI(\theta)}
\end{aligned}   
위 부등식에서 $\frac{1}{n I(\theta)}$는 **크래머-라오 하한(Cramer-Rao lower bound)**라고 합니다.    

즉, $\hat{\theta}$가 $\theta$의 비편향 추정량이며 $Var(\hat{\theta}) = \frac{1}{n I(\theta)}$라면, **$\hat{\theta}$는 최소분산 비편향 추정량**이 됩니다. **크래머-라오 하한은 비편향 추정량이 가질 수 있는 최소의 분산**이기 때문입니다.   

이제 크래머-라오 부등식에 대해 증명을 해보겠습니다.   
우선 $U$와 $V$를 정의해보겠습니다.   
\begin{aligned}
U =& T(X) \newline
V =& \frac{d}{d \theta} log \; f(\mathsf{X} \| \theta) = \sum_{i=1}^{n} \frac{d}{d \theta} log \; f(X_i \| \theta)
\end{aligned}    

$\frac{d}{d \theta} log \; f(X_i \| \theta)$는 서로 독립이므로 $V$의 분산을 구해보겠습니다.   
\begin{aligned}
Var(V) =& Var( \sum_{i=1}^{n} \frac{d}{d \theta} log \; f(X_i \| \theta) ) = nVar(\frac{d}{d \theta} log \; f(\mathsf{X} \| \theta)) \newline
=& nE( \frac{d}{d \theta} log \; f(\mathsf{X} \| \theta) )^2 = nI(\theta)
\end{aligned}    

여기서 $\frac{d}{d \theta} log \; f(\mathsf{X} \| \theta)$의 성질 중 하나인 $E( \frac{d}{d \theta} log \; f(\mathsf{X} \| \theta) ) = 0$을 이용하여 $U$와 $V$의 공분산을 구해보겠습니다.   
\begin{aligned}
Cov(U, V) =& Cov(T(\mathsf{X}), \frac{d}{d \theta} log \; f(\mathsf{X} \| \theta)) \newline
=& E( T(\mathsf{X}) \frac{d}{d \theta} log \; f(\mathsf{X} \| \theta) ) - E(T(\mathsf{X}))E( \frac{d}{d \theta} log \; f(\mathsf{X} \| \theta)) = E( T(\mathsf{X}) \frac{d}{d \theta} log \; f(\mathsf{X} \| \theta) ) \newline
=& \int T(\mathsf{X}) \frac{\frac{d}{d \theta} f(\mathsf{X} \| \theta)}{f(\mathsf{X} \| \theta)} f(\mathsf{X} \| \theta) dx = \int T(\mathsf{X}) \frac{d}{d \theta} f(\mathsf{X} \| \theta) dx \newline
=& \frac{d}{d \theta} ( \int T(\mathsf{X}) f(\mathsf{X} \| \theta) dx ) = \frac{d}{d \theta} E(T(\mathsf{X})) \newline
=& \frac{d}{d \theta} \theta = 1
\end{aligned}   

따라서, 코시-슈바르츠 부등식(Cauchy-Schwarz inequality)에 의해 하기와 같이 나타낼 수 있습니다.   
\begin{aligned}
Var(T) \ge \frac{ ( Cov(T, \sum_{i=1}^{n} \frac{d}{d \theta} log \; f(X_i \| \theta) ) )^2 }{Var( \frac{d}{d \theta} log \; f(X_i \| \theta) )} = \frac{1}{n I(\theta)}
\end{aligned}    

더 쉬운 이해를 위해 예시 문제를 풀어보겠습니다.   
$X_1, X_2, X_3, ..., X_n$이 Poisson($\theta$)로부터의 랜덤샘플일 때 <span style='color:blue'>$\theta$의 추정량 $\bar{X}$(추측)</span>는 $\theta$의 비편향 추정량이고 $Var(\bar{X})=\frac{\theta}{n} = \frac{1}{n I(\theta)}$입니다. 따라서, $\bar{X}$는 $\theta$의 **best unbiased estimator**입니다.   


#### 라오-블랙웰 정리(Rao-Blackewell theorem)
라오-블랙웰 정리의 의미부터 살펴보겠습니다.   
> $S(X) : \theta$의 추정량   
> $T(X) : \theta$의 충분통계량   
> $S \* (X) = E( S(X) \| T(X) ) : \theta$의 추정량이라고 하겠습니다.   
> 단, ① $E(S (X)) = E( S \* (X))$ : $S(X)$가 unbiased estimator라면 $S \* (X)$도 $\theta$의 unbiased estiamtor입니다.   
> &nbsp; &nbsp; ② $Var( S \* (X)) \le Var( S(X))$   

상기의 의미를 정리해보겠습니다.   
$S(X)$가 $\theta$의 nbiased estimator이면, $S \* (X)$ 또한 unbiased estimator가 됩니다.   
또한, $S(X)$보다 $S \* (X)$가 더 좋은 unbiased estimator가 됩니다.   

이제 라오-블랙웰에 대해 증명을 해보겠습니다.   
우선 $S \* (X)$의 기댓값과 분산을 구해보겠습니다.   
\begin{aligned}
E( S \* (X)) =& E( E( S(X) \| T(X) ) ) = E(S(X)) \newline
Var(S(X)) =& E( Var(S(X) \| T(X) ) ) + Var( E( S(X) \| T(X) ) ) \newline
=& E( Var(S(X) \| T(X) ) ) + Var( E( S \* (X) )
\end{aligned}    

따라서, $E(S \* (X)) = E(S(X))$가 되며, $Var(S \* (X)) \le Var(S(X))$가 됩니다.   

몇가지 의문들에 정리해보겠습니다.   
\- $T(X)$가 충분통계량이라는 조건이 왜 필요한가?   
&rarr; $X \bot X$라면 $S \* (X)= E(S(X) \| Y) = \theta$이므로, $S \* (X)$는 $\theta$의 추정량이 되지 못합니다. 그러나 $S(X) \| T(X)$는 $\theta$에 의존하지 않으므로, $E(S(X) \| T(X))$는 추정량이 될 수 있습니다.  
\- 라오-블랙웰 정리에 의하면 $Bias(S \* (X)) = Bias(S(X)), \; Var(S \* (X)) \le Var(S(X))$이 성립합니다. 따라서 $S(X)$는 $\theta$의 비편향 추정량이면서 보다 더 좋은 $S \* (X)$가 있으며 이 또한 비편향 추정량입니다.   
\- 어떤 추정량이든 충분통계량의 조건부 기댓값을 통해 더 우수한 추정량을 만들 수 있습니다.   

#### 레만-쉐퍼 정리
레만-쉐퍼 정리의 의미부터 살펴보겠습니다.   
> $S(X) : \theta$의 추정량   
> $T(X) : \theta$의 완비충분통계량   
> $S \* (X) = E( S(X) \| T(X)$가 $\theta$의 최소분산 비편향 추정량입니다.   

상기의 의미를 정리해보겠습니다.   
라오-블랙웰 정리에 의해 $S \* (X)$도 $\theta$의 unbiased estimator입니다.   
또한, $U(X)$를 임의의 $\theta$에 대한 unbiased estimator라고 가정하겠습니다. 그러면 $Var(S \* (X)) \le Var(U(X))$이면 $S\*(X)$는 $\theta$의 최소분산 비편향 추정량이 됩니다.   

이제 레만-쉐퍼 정리에 대해 증명을 해보겠습니다.   
우선 $U \* (X) = E( U(X) \| T(X))$라고 가정하겠습니다. 그럼 라오-블랙웰 정리에 의해 $E(U \* (X)) = E(U(X)) = \theta, \; Var(U \* (X)) \le Var(U(X))$을 만족합니다.   
$S \* (X)$와 $U \* (X)$ 모두 $\theta$의 비편향 추정량이므로, $E( S \* (X) - U \* (X) ) = \theta - \theta = 0$이 됩니다.(여기서 $S \* (X)$와 $U \* (X)$는 모두 $T(X)$의 함수입니다.)   
**$T(X)$는 완비충분통계량**이니 완비충분통계량의 정의에 의해 하기와 같이 나타낼 수 있습니다.   
\begin{aligned}
S \* (X) - U \* (X) =& 0 \newline
S \* (X)  =& U \* (X) \newline
Var( S \* (X) ) = Var( U \* (X)) \le Var( U(X))
\end{aligned}

### 최소 분산 비편향 추정량
