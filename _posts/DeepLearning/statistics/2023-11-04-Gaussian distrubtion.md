---
title:  "정규 분포"
categories: statistics
tag: [DeepLearning,theory, python,statistics,math]
tags: [Jekyll, MathJax]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
---

## 정규 분포

이번에는 어떻게 보면 통계학 분야에서 확률 분포 중 가장 **중요한 분포인 정규 분포**에 대해 알아보겠습니다. 정규 분포는 다른 이름으로 **가우시안 분포**라고 부르기도 합니다. 이 가우시안 분포는 여러분이 논문을 읽다 보면 가장 많이 나옵니다. 최근에 **Diffusion Model**관련 논문을 읽고 있는데, 이 **가우시안 노이즈**를 이용하기도 합니다. 우리의 일상 생활에서 가장 많은 일들을 설명할 수 있는 분포입니다. 이번 절에서는 우선 정규 분포의 확률 분포 및 확률 밀도 함수를 설명한 후, 정규 분포의 유도 과정에서 대해 다뤄보겠습니다. 

정규 분포의 유도는 크게 2가지 방법으로 유도할 수 있습니다. 첫번째는 과녁맞추기를 통해 알아보겠습니다. 두번째로는 이항 분포를 이용하여 정규 분포를 유도해보겠습니다. 

### 정규 분포 확률 밀도 함수

우선 정규 분포의 확률 밀도 함수 그래프에 대해 알아보겠습니다.   
<img src="../../../assets/images/statistics/2023-11-04-Gaussian distrubtion/Gaussian 1.jpg" alt="Gaussian 1" style="zoom:80%;" />   
그래프는 평균 $\mu$를 중심으로 대칭(symmetric)인 모양입니다.($P(X \le \mu) = 0.5, \; P(\mu \le X)=0.5$) 평균 $\mu$은 그래프의 위치를 나타내며, $\sigma$는 평균으로부터 그래프의 폭을 의미합니다. $\sigma$의 값이 작을수록 그래프가 뾰족한 모용이 됩니다. 그 말은 즉, 분산이 작다는 뜻이며, 분산이 작다는 것은 확률 변수(데이터)들이 평균에 몰려있다는 뜻입니다. 또한, 그래프에서 확인할 수 있는 점은 확률 변수(데이터)는 $\mu$에서 최댓값을 가지는 것을 확인할 수 있습니다.    
다음으로는 정규 분포의 확률 밀도 함수에 대해 확인해보겠습니다.    
<img src="../../../assets/images/statistics/2023-11-04-Gaussian distrubtion/Gaussian 2.jpg" alt="Gaussian 2" style="zoom:80%;" />    
정규 분포는 총 2개의 모수(parameter)를 가지고 있습니다. 평균인 $\mu$와 분산인 $\sigma^2$입니다. 이 모수들에 의해 그래프의 모양과 위치가 결정된다고 생각하시면 됩니다. 


### 통계량

$E(X) = \mu$   
$V(X) = \sigma^2$

### 과녁 맞추기

이번에는 과녁 맞추기를 통해 정규 분포를 유도해보겠습니다.   
<img src="../../../assets/images/statistics/2023-11-04-Gaussian distrubtion/Gaussian 3.jpg" alt="Gaussian 3" style="zoom:80%;" />    
이런 과녁판이 있다고 가정해보겠습니다. 이 과녁판 내의 (x, y)의 좌표에 맞추는 확률을 통해 정규 분포르 유도해보겠습니다. 유도하기 위해서는 몇가지의 가정이 필요합니다. 총 5가지의 가정을 두도록 하겠습니다.    
① 과녁의 중심을 향해 쏩니다.(즉, 중심에 멀어질수록 맞출 확률이 감소합니다.)   
② 중심에서 같은 거리를 맞출 확률은 동일합니다.(즉, 중심 (0,0)으로부터 r만큼 떨어진 좌표들의 맞출 확률은 동일합니다. )   
③ 좌표 평면의 x, y는 서로 독립입니다.   
④ 과녁판은 중심을 기준으로 대칭입니다.( $f(x, y)$는 대칭입니다. )   
⑤ 중심은 (0, 0)입니다. ($\mu_x = \mu_y = 0$)   

우선 확률의 합은 항상 1이됩니다. 따라서 $\int_{- \infty}^{\infty} \int_{- \infty}^{\infty} f(x, y) dx dy = 1$이 됩니다.    
$\int_{- \infty}^{\infty} f(x, y) dy = f(x)$가 됩니다. 이 뜻은, x=k인 점들의 위에 있을 확률을 뜻합니다.    
x, y가 독립이라고 할 수 있는 이유는 $f(y \| x)=f(y)$라는 것입니다. 특정한 x점이 주어졌다고 했을 때의 y의 점을 고르는 확률과 그냥 y의 점을 고르는 확률은 동일합니다. x가 어떻게 주어지든 $f(y)$의 확률은 영향이 없다는 뜻입니다. 이를 통해 $f(x , y) = f(x \cap y) = f(x)f(y)$를 알 수 있습니다.    

이제, r만큼 떨어진 좌표에 $g(r)=f(x,y)$인 g(r)이 있다고 해보겠습니다. 그럼 $r^2=x^2 + y^2$이 됩니다. 이 말은 즉, 극좌표계(polar coordinate system)로 다시 표현하면 $x = r cos(\theta), y=r sin(\theta)$로 변환이 가능합니다. 이제 g(r)을 통해 유도해보겠습니다.   
<img src="../../../assets/images/statistics/2023-11-04-Gaussian distrubtion/Gaussian 4.jpg" alt="Gaussian 4" style="zoom:80%;" />   
상기의 수식 전개를 확인해보겠습니다. x, y는 서로 독립이라 했으니 $g(r)=f(x)f(y)$로 표현할 수 있습니다. 그 후 x, y 또한 r의 변수에 대한 값으로 변형 하였으니, $\theta$에 대해 미분을 진행하겠습니다. 이때 chain rule을 적용하여 x에 대한 미분과 $\theta$에 대한 미분으로 표현을 하겠습니다. 그런 후 $\frac{sin(\theta)}{cos(\theta)}=tan(\theta)$이니 변형을 해주는데, 여기서 $tan(\theta)= \frac{y}{x}$가 되니 다시 x, y의 좌표로 변형하겠습니다. 이 값들은 특정한 상수 값을 가지게 됩니다. 따라서 특정한 상수 C에 대해 $f(x)$함수를 나타내겠습니다. 적분을 한 후 $ln$의 경우 $e$를 통해 표현하면 $f(x)=e^{\frac{C}{2} x^2}A$로 나타낼 수 있습니다.  

<img src="../../../assets/images/statistics/2023-11-04-Gaussian distrubtion/Gaussian 5.jpg" alt="Gaussian 5" style="zoom:80%;" />   
여기서 $f(x)$는 확률을 뜻하기 때문에 항상 양수를 가져야합니다. 따라서 A > 0를 만족해야 합니다. 또한, x값이 커질수록 과녁을 맞추는 확률은 작아져야하니 $f(x)$값은 작아져야합니다. 따라서 C < 0을 만족해야 합니다. 그럼 상수 값들은 모두 양수로 표현하기 위해 치환을 하여 표현하겠습니다.   
<img src="../../../assets/images/statistics/2023-11-04-Gaussian distrubtion/Gaussian 6.jpg" alt="Gaussian 6" style="zoom:80%;" />    

우선 A에 대해 구해보도록 하겠습니다.    
<img src="../../../assets/images/statistics/2023-11-04-Gaussian distrubtion/Gaussian 7.jpg" alt="Gaussian 7" style="zoom:80%;" />   
여기서 이전에 가정했던, 과녁판은 (0, 0)을 기준으로 대칭(symmetric)하다는 점을 이용하고 확률의 합은 1이라는 점을 이용하여 (0, $\infty$)까지의 값을 $\frac{1}{2}$로 나타내겠습니다. 그 후, 양변에 y를 변수로 가지는 함수를 똑같이 곱한 후, x,y는 독립이라는 점을 이용하여 $\int_{0}^{\infty} \int_{0}^{\infty} e^{- \frac{k}{2}(x^2 + y^2)} dx dy = \frac{1}{4 A^2}$으로 나타내겠습니다.    
<img src="../../../assets/images/statistics/2023-11-04-Gaussian distrubtion/Gaussian 8.jpg" alt="Gaussian 8" style="zoom:80%;" />    
그 후 극좌표계(polar coordinate system)를 통해 각도와 거리로 나타내는 함수로 변형하겠습니다. $x=r cos(\theta), y=r sin(\theta)$의 극좌표계로 변형했을 때, $dxdy=rdrd \theta$로 치환이 됩니다. 이 때 $r d\theta$는 호의 길이에 대한 변화율을 뜻하게됩니다.     
<img src="../../../assets/images/statistics/2023-11-04-Gaussian distrubtion/Gaussian 9.jpg" alt="Gaussian 9" style="zoom:80%;" />    
이렇게 되면 A값을 구할 수 있게됩니다. $A = \pm \sqrt{\frac{k}{2 \pi}}$가 되는데,  A >0이니 $A = \sqrt{\frac{k}{2 \pi}} $이 됩니다.   
<img src="../../../assets/images/statistics/2023-11-04-Gaussian distrubtion/Gaussian 10.jpg" alt="Gaussian 10" style="zoom:80%;" />   

A를 구했으니 이번에는 k값을 구해보겠습니다.   
k를 구하기 위해 $x$: 확률변수, $f(x)$ : pdf, $\mu$ = 0이라 가정하고 전개하겠습니다.   
<img src="../../../assets/images/statistics/2023-11-04-Gaussian distrubtion/Gaussian 11.jpg" alt="Gaussian 11" style="zoom:80%;" />   
분산 식의 경우 평균은 0이 되니 $V(X)=E(X^2)$이 됩니다. 똑같이 $f(x)$는 대칭이니 (0, $\infty$)의 값은 $\frac{\sigma^2}{2}$값이 됩니다. 그 후 부분 적분을 이용하는데 이 때, 두 개의 함수를 한 개는 $x$, 다른 한 개는 $xe^{- \frac{k}{2} x^2}$이라고 정하겠습니다.  $-\[ \frac{1}{k} \frac{x}{e^{\frac{k}{2}x^2}}]\_0^{\infty}$에서 무한대로 갈 때는 분모가 훨씬 커지니 0으로 갑니다.  그 후 이전에 A를 구할 때 나왔던 식인 $\int_{0}^{\infty} e^{- \frac{k}{2}x^2} dx = \frac{1}{2A}$를 이용하여 풀어주면 $k=\frac{1}{\sigma^2}$를 얻을 수 있습니다. 각 구한 A와 k값을 $f(x)$에 대입해주면 $f(x)$를 구할 수 있습니다.   
<img src="../../../assets/images/statistics/2023-11-04-Gaussian distrubtion/Gaussian 12.jpg" alt="Gaussian 12" style="zoom:80%;" />   
이 때, 만약 $\mu=0$이 아니라면, 정규 분포에서 $\mu$는 그래프 중심의 위치를 의미한다고 했기 때문에 $\mu$값만큼 대칭이동을 해주면 됩니다. 따라서 평균을 임의의 $\mu$라고 가정한다면 아래와 같이 우리가 알고 있는 정규 분포 수식이 완성됩니다.   
$f(x)=\frac{1}{\sqrt{2 \pi \sigma^2}} e^{- \frac{(x - \mu)^2}{2 \sigma^2}}$

### 이항 분포를 통한 정규 분포 유도

이번에는 이항 분포를 통해 정규 분포를 유도해보겠습니다. 우선 이항 분포의 개념에 대해 다시 짚어보도록 하겠습니다.    
<img src="../../../assets/images/statistics/2023-11-04-Gaussian distrubtion/Gaussian 13.jpg" alt="Gaussian 13" style="zoom:80%;" />   
이항 분포는 시행 횟수 n번이고 확률이 p일 때, x번의 사건이 일어날 확률에 대한 확률 밀도 함수(pmf)입니다. 편의상 사건의 여사건의 확률인 $1-p=q$로 표현하겠습니다. 이항 분포의 통계랑에 대해 설명하겠습니다. 이항 분포의 기댓값과 분산은 $E(X) = np, V(X) = npq$가 됩니다.    

이제 이항 분포를 통해 정규 분포를 유도하겠습니다. 단, 여기서 **n &rarr; $\infty$**라고 가정을 두겠습니다. 이항 분포 확률 밀도 함수인 $p(x)$를 $f(r)$로 표현하겠습니다.   
<img src="../../../assets/images/statistics/2023-11-04-Gaussian distrubtion/Gaussian 14.jpg" alt="Gaussian 14" style="zoom:80%;" />

우선 각 변에 $ln$을 취해준 후, 스털링 근사를 이용하여 표현하겠습니다. 스털링 근사로 표현한 식인 $ln(f(r))$은 길어져서 간략하게 $g(r)$이라는 함수로 표현하겠습니다.    
자 $g(r)$을 표현해보겠습니다.   
<img src="../../../assets/images/statistics/2023-11-04-Gaussian distrubtion/Gaussian 15.jpg" alt="Gaussian 15" style="zoom:80%;" />   
상기와 같이 스털링 근사를 이용하여 $g(r)$을 나타낼 수 있습니다. 이어서 추후에 $g'(r)$과 $g''(r)$이 사용되기 때문에, 미리 두 개의 함수에 대해서도 정의해놓도록 하겠습니다.   
<img src="../../../assets/images/statistics/2023-11-04-Gaussian distrubtion/Gaussian 16.jpg" alt="Gaussian 16" style="zoom:80%;" />   
<img src="../../../assets/images/statistics/2023-11-04-Gaussian distrubtion/Gaussian 17.jpg" alt="Gaussian 17" style="zoom:80%;" />   

함수 $g(r)$을 r=np에서의 테일러 근사로 표현하면 하기와 같이 나타낼 수 있습니다.   
<img src="../../../assets/images/statistics/2023-11-04-Gaussian distrubtion/Gaussian 18.jpg" alt="Gaussian 18" style="zoom:80%;" />   
그럼 이제 r 대신 np를 대입한 $g(np), g'(np), g''(np)$를 나타내보겠습니다.   
<img src="../../../assets/images/statistics/2023-11-04-Gaussian distrubtion/Gaussian 19.jpg" alt="Gaussian 19" style="zoom:80%;" />   
<img src="../../../assets/images/statistics/2023-11-04-Gaussian distrubtion/Gaussian 20.jpg" alt="Gaussian 20" style="zoom:80%;" />    
<img src="../../../assets/images/statistics/2023-11-04-Gaussian distrubtion/Gaussian 21.jpg" alt="Gaussian 21" style="zoom:80%;" />       
대입을 통해 구해낸 $g(np), g'(np), g''(np)$를 테일러 근사식에 대입해보겠습니다.   
<img src="../../../assets/images/statistics/2023-11-04-Gaussian distrubtion/Gaussian 22-169917331811725.jpg" alt="Gaussian 22" style="zoom:80%;" />   
여기서 이전에 n &rarr; $\infty$라고 가정을 두었습니다. 따라서, 분모에 n이 들어가서 훨씬 크게 증가하는 $\frac{p-q}{2npq}(r - np)$와 $\frac{p^2 + q^2}{2 n^2 p^2 q^2} (r - np)^2$들은 0으로 수렴하게 됩니다. 그럼 남아 있는 값인 $g(r) = ln ( \frac{1}{\sqrt{2 \pi n p q}}) - \frac{1}{2npq}(r - np)^2$이 됩니다. 이제 $g(r)$은 구했으니, $f(r)$을 구해보도록 하겠습니다.   
<img src="../../../assets/images/statistics/2023-11-04-Gaussian distrubtion/Gaussian 23.jpg" alt="Gaussian 23" style="zoom:80%;" />   
$ln$을 빼준 후 $f(r)$을 전개하면 상기와 같이 나옵니다. 여기서 이항 분포의 평균과 분산은 $E(X) = \mu = np, V(X) = \sigma^2 = npq$이니 치환을 해주면 우리가 알고 있던 정규 분포 식의 형태가 나옵니다.   

따라서 최종적으로 정규 분포 수식인 $f(x) = \frac{1}{\sqrt{2 \pi \sigma^2}} e^{- \frac{(x - \mu)^2}{2 \sigma^2}}$을 얻을 수 있습니다.   
<img src="../../../assets/images/statistics/2023-11-04-Gaussian distrubtion/Gaussian 24.jpg" alt="Gaussian 24" style="zoom:80%;" />

### Appendix

스털링 근사(Stirling’s approximation)에 대해 간략하게 알아보도록 하겠습니다. 스털링 근사는 매우 큰 n에 대해 성립합니다.   
$n! \approx \sqrt{2 \pi n}(n / e)^n$   
이를 구체적으로 나타내면 하기와 같습니다.   
$lim_{n -> \infty} \frac{1}{n!} \sqrt{2 \pi n}(n / e)^n = 1$   
이제 스털링 근사를 일반화하여 스털링 급수로 나타내보겠습니다.   
$n! \; \~ \; \sqrt{2 \pi n}(\frac{n}{e})^n (1 + \frac{1}{12n} + \frac{1}{288 n^2} + ...)$   
양 변에 ln을 취해주겠습니다.   
$ln(n!) \approx n ln(n) - n + \frac{1}{2}ln(2 \pi n) + \frac{1}{12n} - \frac{1}{360n^3} + ...$   
여기서 n &rarr; $\infty$로 간다면 뒷 부분의 상수항들은 0으로 수렴하여 제거한다면 하기와 같이 나타낼 수 있습니다.   
$ln(n!) \approx n ln(n) - n + \frac{1}{2}ln(2 \pi n)$
