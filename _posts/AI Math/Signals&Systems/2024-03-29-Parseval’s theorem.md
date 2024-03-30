---
title:  "Parseval 정리(Parseval’s theorem)"
categories: SystemsAndSignals
tag: [theory, Systems, Signals]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: Parseval’s theorem
comments: true
date: 2024-03-29
toc_sticky: true
---

## 푸리에 변환(Fourier Transform)과 푸리에 역변환(Inverse Fourier Transform)
이전 포스팅에서 푸리에 변환과 푸리에 역변환의 유도를 해보았습니다. 푸리에 변환을 간단하게 말하면 **푸리에 급수의 한계인 Periodic Signal에 대한 한정적인 부분을 APeriodic Signal까지로 확장**한 것 입니다. 또한, 푸리에 계수인 $a_k$와 주기인 $T_0$의 곱인 $T_0 a_k$값을 관찰한 것 입니다. 여기서 $x(t) \Leftrightarrow X(\omega)$와 같이 1대1 대응이 되기 때문에 반대로 역변환도 구할 수 있게됩니다. 그럼 FT(Fourier Transform)와 IFT(Inverse Fourier Transform)의 수식을 하기에 정의하겠습니다.  
\begin{aligned}    
FT : X(\omega) =& \int_{- \infty}^{\infty} x(t) e^{- j \omega t} dt \newline   
IFT : x(t) =& \frac{1}{2 \pi} \int_{- \infty}^{\infty} X(\omega) e^{j \omega t} d \omega \newline   
x(t) \Leftrightarrow& X(\omega)
\end{aligned}  

## Parseval's theorem
우선 Parseval 정리의 식부터 정의해보겠습니다. Parseval 정리는 **시간 축에서 제곱한 것의 적분과 $\omega$축에서 제곱하여 적분한 것과 같다**는 것입니다.   
\begin{aligned}    
\int_{-\infty}^{\infty} |x(t)|^2 dt = \frac{1}{2 \pi} \int_{- \infty}^{\infty} | X(\omega) |^2 d \omega 
\end{aligned}  

### 증명
증명을 위해, $x_1(t)$dhk $x_2^{\*}(t)$ 두 개의 곱에 대한 적분으로 증명해보겠습니다. 그 이유는 만약, $x_1(t)=x_2^{\*}(t)$라면, 제곱이 되기 때문입니다.   
\begin{aligned}    
x_1(t) =& \frac{1}{2 \pi} \int_{- \infty}^{\infty} X_1(\omega) e^{j \omega t} d\omega \newline   
x_2^{\*}(t) =& \frac{1}{2 \pi} \int_{- \infty}^{\infty} X_2^{\*}(\omega') e^{- j \omega' t} d\omega' \newline   
\int_{-\infty}^{\infty} x_1(t) x_2^{\*} dt =& \frac{1}{2 \pi} \int_{- \infty}^{\infty} X_1(\omega) e^{j \omega t} d\omega \frac{1}{2 \pi} \int_{- \infty}^{\infty} X_2^{\*}(\omega') e^{- j \omega' t} d\omega' \newline   
=& \frac{1}{4 \pi^2} \int_{-\infty}^{\infty} X_1(\omega) \int_{-\infty}^{\infty} X_2^{\*}(\omega') \int_{-\infty}^{\infty} e^{j \omega t} e^{- j \omega' t} dt d\omega' d\omega   \newline   
=& \frac{1}{4 \pi^2} \int_{-\infty}^{\infty} X_1(\omega) \int_{-\infty}^{\infty} X_2^{\*}(\omega') 2 \pi \delta(\omega' - \omega) d \omega' d\omega \newline   
=& frac{1}{2 \pi} \int_{-\infty}^{\infty} X_1(\omega) X_2^{\*}(\omega') d\omega
\end{aligned}  

이제 전개한 식을 살펴보겠습니다. 제일 안쪽에 있는 적분인 $\int_{-\infty}^{\infty} e^{j \omega t} e^{- j \omega' t} dt$ 해당 식은 $e^{j \omega t}$를 푸리에 변환하는 식으로 볼 수 있습니다. 즉, $2 \pi \delta(\omega' - \omega)$의 값을 가진다는 것을 알 수 있습니다. 그럼 이제 $\int_{-\infty}^{\infty} X_2^{\*}(\omega') 2 \pi \delta(\omega' - \omega) d \omega'$을 구해보겠습니다. 해당 식은, $\omega'$에 대해 적분을 하는 것이니, $\omega$는 상수로 생각을 하면 됩니다. 그럼 $\delta(\omega' - \omega)$여서 $\omega$ 축에서만 값을 가집니다. 그리고 적분 값은 1이 됩ㄴ디ㅏ. 따라서 $\int_{-\infty}^{\infty} X_2^{\*}(\omega') 2 \pi \delta(\omega' - \omega) d \omega = 2 \pi X_2^{\*}(\omega')$가 됩니다.    
최종적으로 $\int_{-\infty}^{\infty} x_1(t) x_2^{\*} dt = frac{1}{2 \pi} \int_{-\infty}^{\infty} X_1(\omega) X_2^{\*}(\omega') d\omega$라는 결과를 얻을 수 있습니다. 이전에 $x_1(t)=x_2^{\*}(t)$라고 가정을 했습니다. 그럼 푸리에 변환한 결과인 $x_1(\omega)=X_2^{\*}(\omega)$가 됩니다. 따라서 $\int_{-\infty}^{\infty} | x_1(t)|^2 dt = frac{1}{2 \pi} \int_{-\infty}^{\infty} | X_1(\omega)
|^2 d\omega$을 얻으며, Parseval 정리가 증명됩니다.