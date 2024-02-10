---
title:  "푸리에 급수(Fourier Series)1"
categories: SystemsAndSignals
tag: [theory, Systems, Signals]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: Fundamental Frequency and Fourier Series
comments: true
date: 2024-02-09
toc_sticky: true
---

## 기본 주파수(Fundamental Frequency)
우선 주파수가 어떤 것인지 알아봐야하는데, 주파수는 **한 주기동안 발생한 사이클의 수**를 뜻하며, Hz의 단위로 표현합니다. 그럼 주기는 **한 사이클이 진행되는데 걸리는 시간**을 의미합니다. 즉, 주파수를 $F$라고 한다면, 주기는 $T=\frac{1}{F}$가 되며, 서로 **역수**관계를 띕니다.   

그럼 기본 주파수(Fundamental Frequency)를 알려면 기본 주기(Fundamental Period)를 알아야합니다. 
주파수에 들어가기전에 우선 주기함수란 것을 많이보게 될 것 입니다. 그럼 우선 주기함수가 어떤 것인지 뭔저 알아보겠습니다. 위키피디아에 나와있는 주기함수의 정의는 하기와 같습니다.   
> 수학에서 주기 함수(periodic function)는 함숫값이 일정 주기마다 되풀이되는 함수입니다. 일상적인 예로, 시계 시간은 시간에 대한 함수로서 주기 함수입니다. 

주기함수를 수식으로 나타내면 하기와 같습니다.   
\begin{aligned}    
x(t) = x(t + T) \quad for \quad all \quad t
\end{aligned}    

여기서 $T$가 주기를 나타냅니다. 즉, 같은 사이클이 T마다 반복된다는 뜻을 의미합니다. 대표적인 주기함수는 cosine 함수와 sin 함수가 있습니다. cosine의 경우로 한번 생각해보겠습니다. cosine 함수는 $2\pi$마다 같은 함수를 반복합니다. 그럼 이때 주기인 $T= 2\pi$라고 해도 되고, 또 $4\pi$마다도 같은 그래프가 반복됩니다. 그럼 주기를 $T=4\pi$라고 해도 됩니다. 하지만, 같은 그래프가 반복되는 것은 최소가 $2\pi$부터입니다. 여기서 **주기가 가장 작은 값을 Fundamental Period**라고 합니다.    
그럼 추가적으로 상수함수는 주기함수인지 확인해보겠습니다. 상수함수 $x(t)=2$는 상기의 주기함수의 수식 조건을 만족합니다. 따라서, 주기함수가 됩니다. 주기의 경우는 $T=1$이나 $T=2$이거나 모두 다 가능합니다. 하지만, 이 **주기의 최소값은 존재하지 않습니다.**즉, 최소 주기를 정의할 수 없기 때문에 **Fundamental Period를 얘기할 수는 없습니다.**

## 푸리에 급수(Fourier Series)
우선 급수(Series)를 다시 한번 더 짚어보겠습니다. Series는 함수를 함수로 표현한 것 입니다. 대표적인 Series 중 Talyor Series를 기억하실 겁니다. Talyor Series는 함수를 다항함수의 합으로 표현한 것 입니다.   
그럼 Fourier Series는 주기함수를 함수로 표현하는 것 입니다. 즉, Fourier Series는 **어떤 주기함수(Periodic Function)을 정형파(Sinusoid)의 합으로 표현**한 것 입니다. 주기가 $T_0$인 주기함수를 표현해보겠습니다.   
\begin{aligned}    
x_{T_0}(t) =& \ldots + a_{-2}e^{j(-2)\omega_0 t} + a_{-1}e^{j(-1)\omega_0 t} + a_0 + a_{1}e^{j\omega_0 t} + a_{2}e^{j2\omega_0 t} + \ldots \newline   
=& \sum_{k= -\infty}^{\infty} a_ke^{jk\omega_0 t}
\end{aligned}    

상기의 수식을 한 번 살펴보겠습니다. 우선 $e^{j 2\pi t}$를 살펴보겠습니다. Euler's Formula 포스팅에서 $Ae^{j \theta}$를 기억하실 겁니다. $Ae^{j \theta}$는 Imaginary와 Real의 좌표계에서 크기가 길이가 A이며 $\theta$의 각도를 가지는 점의 좌표를 나타냈습니다. 그럼 $e^{j 2\pi t}$에서 $t$는 0에서 1까지 변한다고 했을 때, Imaginary와 Real의 좌표계에서 1의 크기로 원이 그려질 것 입니다. 그 다음 $t$가 1에서 2까지 변한다고 해도 똑같은 원이 그려질 것 입니다. 1초의 주기를 가지는 주기함수란 것을 알 수 있습니다.    
그럼 상기의 말을 이해했다고 가정하고 $e^{j 2\pi f t}$에 대해 살펴보겠습니다. 해당 값은 $f$가 곱해진 형태로 1초의 주기에서 $\frac{1}{f}$의 주기를 가지는 것을 알 수 있습니다. 즉, 1초동안에 $f$번을 돌 것 입니다. 즉, 진동수로 표현하면 $fHz$가 될 것 입니다.   

그럼 상기의 수식에서 $\omega_0$는 치환이며 $\omega_0 \triangleq 2 \pi f_0$가 됩니다. 여기서 $f_0$는 $x_{T_0}$가 매칭되는 주파수입니다. 근데 여기서 $f_0$는 **하모닉(Harmonics) 주파수**를 가집니다. 하모닉 주파수란 기본 주파수의 정수배가 되는 주파수를 의미합니다. 예를 들어 $T_0$의 기본 주파수가 $3Hz$라고 한다면, 정수배인 $0Hz, 3Hz, 6Hz, 9Hz, \ldots$가 된다는 것 입니다. 즉, **$x_{T_0}(t)$를 하모닉 주파수로 구성된 정현파(Sinusoids)로 표현**한 것 입니다. 
만약, $x_{T_0}(t)$의 진동수가 $3Hz$라고 했다면, $e^{jk\omega_0 t}$들의 합으로 표현했으니, 1초에 0번 회전하는 값과 3번 회전하는 값, 6번 회전하는 값, 9번 회전하는 값 등 들의 합으로 나타낸 것 입니다.    

근데 여기서 푸리에 급수는 왜 Complex Exponential로 표현했는지에 대해 알아보겠습니다. 우선 주기함수 자체가 Complex 주기함수일 수도 있기 때문입니다. 하지만, 만약 Complex 주기함수가 아닌 실수 주기함수일 경우에는 어떻게 되는지 알아보겠습니다. $a_ke^{jk\omega_0 t} = a_k(cos + jsin)$로 나타내집니다. 그럼 $a_{-k}e^{-jk\omega_0 t} = a_{-k}(cos - jsin)$으로 나타냅니다. 만약 Conjugate를 취해준 결과인 $a_k = a_{-k}^{\*}$라면 두개를 더하면 Complex부분이 사라지면 Real인 실수부분만 남아 표현이 가능해집니다.   


### 깁스 현상(Gibbs Phenomena)
깁스 현상은 불연속을 포함하는 파형이 푸리에 합성되었을때 불연속 값 근처에서 나타나는 불일치 현상을 의미합니다. 푸리에 합성에 고주파수 성분이 많을수록 불일치는 줄어드나 완전히 없애지는 못합니다. 그런 현상을 깁스 현상이라고 합니다. 
상기의 영상은 Sinusoids들을 하나씩 계속해서 더했을 때, 검은색 그래프와 닮아가는 것을 알 수 있습니다. 하지만, 약간 튀어나는 부분이 있는데, 이 부분은 잘 안줄어들며 대략 9%정도 된다고 합니다. 보면, 저주파의 성분들을 더할 때는 대략적인 모양을 금방 나타내나 완전히 비슷하게 못하지만, 고주파의 성분까지 더해주면 조금 더 검은색 그래프와 닮아가는 것을 확인할 수 있습니다.    
<img src="../../../assets/images/Signals&Systems/2024-02-09-Fourier Series1/Gibbs1.png" alt="Gibbs1" style="zoom:80%;" />    