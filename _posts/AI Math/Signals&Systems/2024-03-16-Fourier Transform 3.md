---
title:  "푸리에 변환(Fourier Transform)3"
categories: SystemsAndSignals
tag: [theory, Systems, Signals]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: 다양한 함수의 FT
comments: true
date: 2024-03-16
toc_sticky: true
---

## 푸리에 변환(Fourier Transform)과 푸리에 역변환(Inverse Fourier Transform)
이전 포스팅에서 푸리에 변환과 푸리에 역변환의 유도를 해보았습니다. 푸리에 변환을 간단하게 말하면 **푸리에 급수의 한계인 Periodic Signal에 대한 한정적인 부분을 APeriodic Signal까지로 확장**한 것 입니다. 또한, 푸리에 계수인 $a_k$와 주기인 $T_0$의 곱인 $T_0 a_k$값을 관찰한 것 입니다. 여기서 $x(t) \Leftrightarrow X(\omega)$와 같이 1대1 대응이 되기 때문에 반대로 역변환도 구할 수 있게됩니다. 그럼 FT(Fourier Transform)와 IFT(Inverse Fourier Transform)의 수식을 하기에 정의하겠습니다.  
\begin{aligned}    
FT : X(\omega) =& \int_{- \infty}^{\infty} x(t) e^{- j \omega t} dt \newline   
IFT : x(t) =& \frac{1}{2 \pi} \int_{- \infty}^{\infty} X(\omega) e^{j \omega t} d \omega \newline   
x(t) \Leftrightarrow& X(\omega)
\end{aligned}  

## 주기함수 FT
이번에는 주기함수를 푸리에 변환하는 것에 대해 알아보겠습니다.
### $cos(\omega_0 t)$
$cos(\omega_0 t)$의 주기함수를 푸리에 변환을 하려면 우선 의문점이 크게 2가지를 가지실 것 입니다. <span style='color:blue'>① **푸리에 변환은 비주기 함수에 대해 가능하다** ② **푸리에 변환을 하려면 디리클레 조건을 만족해야한다** </span>   
① 조건의 경우 $cos(\omega_0 t)$는 주기함수인데 어떻게 푸리에 변환을 할 수 있는지 생각해보겠습니다. 만약 $cos$의 함수꼴을 가지는데, 특정 시점인 $T_0$에서부터 쭉 0의 값을 가지는 함수가 있다고 해보겠습니다. 그럼 이 함수는 비주기 함수가 됩니다. 근데 만약 $T_0 \rightarrow \infty$라고 가정을 해보면, 기존 $cos$함수가 되며 주기함수가 됩니다. 즉, $T_0 \rightarrow \infty$의 전제 조건이 없다면 비주기 함수라는 뜻이되니, 푸리에 변환을 할 수 있는 ① 조건을 만족하게 됩니다.   
② 조건의 경우, 디리클레 조건은 $\int_{- \infty}^{\infty} | x(t) | dt < \infty$을 만족해야합니다. 하지만, $cos$ 함수의 경우 무한대 구간에서 적분을 하면, 수렴하는 것이 아닌 발산을 합니다. 따라서, 디리클레 조건을 만족하지는 못합니다. 하지만, 이전에 푸리에 변환의 조건에서 디리클레 조건은 **충분 조건**이었던 것을 기억하실 겁니다. 디리클레 조건을 만족한다면 무조건 푸리에 변환이 가능한 것이고, 푸리에 변환인 가능한 함수가 무조건 디리클레 조건을 만족하지는 않는다는 의미가 됩니다. 즉, 이런 경우들의 대표적인 함수가 주기함수가 됩니다.   
<br>
이제 $cos(\omega_0 t)$가 푸리에 변환이 가능하다는 것을 증명했으니, 푸리에 변환을 해보겠습니다. 우선 $e^{j \omega_0 t} = cos(\omega_0 t) + jsin(\omega_0 t)$를 이용하여 $cos(\omega_0 t)$를 표현해보겠습니다.   
\begin{aligned}    
cos(\omega_0 t) = \frac{e^{j \omega_0 t} + e^{j-  \omega_0 t}}{2}
\end{aligned}  

그럼 $e^{j \omega_0 t}$에 대한 푸리에 변환을 구해주면 바로 주기함수인 $cos(\omega_0 t)$의 푸리에 변환을 구할 수 있을 것 같다는 느낌을 느끼셨을 겁니다. 한번 IFT를 이용하여 구해보겠습니다.   
\begin{aligned}    
e^{j \omega_0 t} =& \frac{1}{2 \pi} \int_{- \infty}^{\infty} 2\pi \delta(\omega - \omega_0) e^{j \omega t} d \omega \newline   
e^{- j \omega_0 t} =& \frac{1}{2 \pi} \int_{- \infty}^{\infty} 2\pi \delta(\omega + \omega_0) e^{j \omega t} d \omega \newline   
cos(\omega_0 t) =& \frac{e^{j \omega_0 t} + e^{j-  \omega_0 t}}{2} \newline   
=& \pi \delta(\omega - \omega_0) + \pi \delta(\omega + \omega_0) 
\end{aligned}   

상기의 전개식을 살펴보겠습니다. IFT에서 $e^{j \omega_0 t}$의 결과가 나오게 되려면 한번 델타함수를 넣어보면 $\omega_0$위치에서ㅔ 그대로 $e^{j \omega_0 t}$값이 나올 수 있습니다. 그럼 앞에 $\frac{1}{2 \pi}$를 제거해주기 위해 $2\pi$를 곱해주면 됩니다. 그럼 $cos(\omega_0 t)$의 푸리에 변환은$\pi \delta(\omega - \omega_0) + \pi \delta(\omega + \omega_0)$인 것을 알 수 있습니다.

<br>
여기서 중요한 점이 있습니다. 푸리에 변환은 1대1 대응이라고 했습니다. 그럼 양변에 $\omega_0$가 아닌 $k\omega_0$을 넣어주면, $e^{k j \omega_0 t}$의 푸리에 변환은 $2 \pi \delta(\omega - k\omega_0)$가 됩니다. 그럼 푸리에 급수를 푸리에 변환 해보겠습니다.   
\begin{aligned}    
\sum_{k= -\infty}^{\infty} a_k e^{jk \omega_0 t} \Leftrightarrow \sum_{k = - \infty}^{\infty} 2 \pi a_k \delta(\omega - k \omega_0) 
\end{aligned}   

다음과 같이 푸리에 급수의 푸리에 변환을 일반화할 수 있습니다. 즉, 주기함수에 대한 푸리에 변환은 delta 함수로 나타나집니다. 그럼 우선 푸리에 시리즈의 $a_k$를 구한 후 푸리에 변환을 해주면, 주기함수의 푸리에 변환을 구해줄 수 있다는 의미가 됩니다. 
### delta train
delta train의 함수는 주기를 가진 delta 함수들의 합입니다. 즉, 만약 주기가 $T_0$라고 한다면, $\sum_{k = -\infty}^{\infty} \delta(t - k T_0)$가 됩니다.   
그럼 delta train함수도 주기함수 이니 푸리에 급수로 나타낼 수 있을 겁니다. $a_k$를 구해보겠습니다.   
\begin{aligned}    
a_k = \frac{1}{T_0} \int_{- \frac{T_0}{2}}^{\frac{T_0}{2}} \delta(t) e^{- j k\omega_0 t} dt = \frac{1}{T_0} \; (for \; all \; k)   
\end{aligned}   

$a_k$를 구했으니 이제 푸리에 변환을 해주면 됩니다. 즉, delta train의 푸리에 변환은 $\sum_{k = -\infty}^{\infty} \frac{2 \pi}{T_0} \delta(\omega - k \omega_0)$가 된다는 의미가 됩니다. 