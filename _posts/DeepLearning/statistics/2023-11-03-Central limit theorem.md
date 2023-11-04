---
title:  "중심 극한 정리(Central Limit Theorem)"
categories: statistics
tag: [DeepLearning,theory, python,statistics,math]
tags: [Jekyll, MathJax]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
---

## 중심 극한 정리(Central Limit Theorem)

중심 극한 정리는 고등학교에서 간단하게 한 줄로 표기되어 배웁니다. 고등학교에 적혀 있는 설명은 **모집단이 평균은 $\mu$이고 표준편차는 $\sigma$인 임의의 분포를 따른다고 할 때, 이 모집단으로부터 추출된 표본의 크기 n이 충분히 크다(n > 30)면 표본 평균들이 이루는 분포는 평균은 $\mu$이고 표준편차는 $\sigma$인 정규 분포를 따른다.**라고 설명되어 있습니다. 이번 포스팅에서는 이 중심 극한 정리가 성립되는 이유(증명)에 대해 알아보겠습니다. 

우선 중심 극한 정리를 증명하기 위해 **정규 분포의 적률 생성 함수**와 **표본 평균($\bar{X}$)의 적률 생성 함수가 같다는 것**을 증명하도록 하겠습니다. 증명 하기 위해서는 우선 **① 정규 분포의 적률 생성 함수 ② 표본 평균의 적률 생성 함수 ③ 두 확률 변수의 적률 생성 함수가 같다면, 확률 분포도 같다** 이렇게 3개를 증명할 것 입니다.

### 두 확률 변수의 확률 분포가 같을 조건

두 확률 변수 X, Y가 있다고 하겠습니다. X의 확률 분포는 $f(x)$, Y의 확률 분포는 $g(y)$라고 하겠습니다.    
여기서, 두 확률 분포 $f(x)=g(y)$를 보이기 위해서는 $M_{x}(t) = M_{y} (t)$인 두 확률 변수의 적률 생성 함수가 동일하다는 것을 보이면 됩니다. 그럼 우선 두 확률 변수의 적률 생성 함수가 동일하다는 것을 보이도록 하겠습니다.    
$M_{x} (t) = \int_{x_1}^{x_2} e^{tx} f(x) dx = \int_{- \infty}^{ \infty} e^{tx} f(x) dx $   
여기서 ($- \infty$, $\infty$)로 바꿀 수 있는 이유는 확률 분포가$x_1$에서 $x_2$사이의 값을 가진다고 하면, 그 외의 값들에 대해서는 0을 가지기 때문에 무한대로 변형해도 똑같은 값을 가지게 됩니다. 따라서 X의 적률 생성 함수를 구했고 똑같이 Y의 적률 생성 함수도 구할 수 있습니다.   
$M_{y} (t) = \int_{- \infty}^{ \infty} e^{ty} g(y) dy $    
여기서, x와 y가 각각 어떤 실수 집합에 포함된다고 해보겠습니다. $x \in R_x, \; y \in R_y$   
이 때, 이 실수 집합들의 합집합은 $R_x \cup R_y= A$라고 하겠습니다. 그럼 x와 y는 모두 A의 집합에 들어가 있으며, 이 때 A의 집합 중 어떤 원소 하나 $a$를 선정하겠습니다. $a \in A$, 이 $a$는 x와 y 중 어떤 값이든 될 수 있습니다. 그럼 X와 Y의 적률 생성 함수를 $a$의 원소로 표현하여 나타내겠습니다.   
$M_{x} (t) = \int_{- \infty}^{ \infty} e^{ta} f(a) da $   
$M_{y} (t) = \int_{- \infty}^{ \infty} e^{ta} g(a) da $   
그럼 $M_x (t) = M_y (t)$라고 한다면, $\int_{- \infty}^{ \infty} e^{ta} f(a) da = \int_{- \infty}^{ \infty} e^{ta} g(a) da$을 확인할 수 있습니다. 좌항을 우항으로 넘기면, $ \int_{- \infty}^{ \infty} e^{ta} (f(a) - g(a)) da = 0$이 됩니다. 이 때, $e^{ta}$나 $f(a) - g(a)$ 둘 중 하나가 0이어야지 성립하지만, 증가함수인 $e^{ta} > 0$입니다. 따라서 $f(a) = g(a)$인 것을 확인할 수 있습니다.   
따라서, <span style='color:red'>**$M_x (t) = M_y (t)$라면, $f(a) = g(a)$인 X의 분포와 Y의 분포가 동일하다는 것을 알 수 있습니다**</span>   

### 정규 분포의 적률 생성 함수

이번에 정규 분포의 적률 생성 함수를 구해보겠습니다. 우선 확률 변수 X가 평균이 $\mu$, 분산이 $\sigma$인 정규분포를 따른다고 하겠습니다.   
$X, \; \; f(x) = \frac{1}{\sqrt{2 \pi \sigma^2}}e^{- \frac{(x - \mu )^2}{2 \sigma^2}}$   
정규 분포의 적률 생성 함수를 구해보면   
<img src="../../../assets/images/statistics/2023-11-03-Central limit theorem/CTL1.jpg" alt="CTL1" style="zoom:80%;">   
상기와 같이 $\int_{- \infty}^{\infty} e^{tx} \frac{1}{\sqrt{2 \pi \sigma^2}}e^{- \frac{(x - \mu )^2}{2 \sigma^2}} dx$의 적률 생성 함수가 생성됩니다. 여기서 trick을 이용하여, 평균이 $\mu + \sigma^2 t$ 분산이 $\sigma$인 정규 분포를 만들어 줍니다. 정규분포의 확률 분포의 합은 1이 되니, 정규 분포의 적률 생성 함수는 $e^{\frac{2 \mu t + \sigma^2 t^2}{2}}$를 구할 수 있습니다. 

### 표본 평균의 적률 생성 함수
