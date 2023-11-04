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

<img src="../../../assets/images/statistics/2023-11-03-Central limit theorem/CTL2.jpg" alt="CTL2" style="zoom:80%;">   
모집단에서 추출한 표본 평균 $\bar{X}$를 확인해 보겠습니다. 우선 $\bar{X_1}, \bar{X_2}, ...$들의 표본 평균의 기댓값인 $E(\bar{X})$는 모집단의 평균인 $\mu$와 동일합니다. 또한, 표본 평균의 분산인 $V(\bar{X})=\frac{\sigma^2}{n} = s^2$임을 알 수 있습니다. 그럼 표본 평균의 적률 생성 함수인 $M_{\bar{X}} (t)$를 구해보도록 하겠습니다.   
<img src="../../../assets/images/statistics/2023-11-03-Central limit theorem/CTL3.jpg" alt="CTL3" style="zoom:80%;" />   
우리가 이전에 배웠던 적률 생성 함수 수식에 맞게 $M_{\bar{X}} (t) = E(e^{\bar{X}t})$를 전개하겠습니다. 우선 표본 평균 $\bar{x}=\frac{x_1+x_2+...}{n}$입니다. 그리고 표본들은 $x_1, x_2, ... \approx = x$들은 모집단의 확률 변수와 동일합니다. 따라서 $E(e^{\frac{x}{n}t})^n$으로 나타낼 수 있습니다. 그 후 상기의 이미지에 전개한 것처럼 테일러 급수를 이용하여 $e^{\frac{(x- \mu )}{n}t}$를 전개한 후 기댓값을 씌어주며 기댓값의 성질을 이용하여 전개합니다. 상기의 이미지에서 k는 뒤에 쭉 이어지는 항들을 $\frac{1}{n^2}$으로 묶은 후 남은 값들에 대한 것입니다. 따라서 현재 상태에서 표본 평균의 적률 생성 함수는 $M_{\bar{X}} (t) = e^{\mu t} (1 + \frac{t^2 s^2}{2n} + \frac{1}{n^2}k)^n$임을 확인할 수 있습니다.   
<img src="../../../assets/images/statistics/2023-11-03-Central limit theorem/CTL4.jpg" alt="CTL4" style="zoom:80%;" />   
이어서 전개하면 상기와 같이 전개됩니다. 우선 n이 무한대로 간다는 뜻은 표본을 무한히 뽑을 때라는 의미가 됩니다.    

### 결론

최종적으로 우선 정규 분포의 적률 생성 함수는 $e^{\frac{2 \mu t + \sigma^2 t^2}{2}}$이 되고, 표본 평균의 적률 생성 함수는 $e^{\frac{2 \mu t + s^2 t^2}{2}}$이 됩니다. $s^2$은 표본 평균의 분산이 되니 동일하다고 볼 수 있습니다. 이렇게 정규 분포와 표본 평균의 적률 생성 함수가 동일하다는 뜻은 **확률 분포도 동일**하다는 뜻이 됩니다. 따라서, 중심 극한 정리가 증명이 됩니다.   
<span style='color:red'>**표본 크기가 무한히 클 때, 모집단의 분포와 상관없이 표본 평균의 분포는 정규 분포를 따른다.**</span>라는 결론이 나옵니다.   
