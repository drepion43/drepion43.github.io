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

## 미분함수 FT
기존 푸리에 역변환의 수식은 하기와 같이 정의됩니다. 여기서 좌측은 시간축에 대한 푸리에 변환전 함수이고, 우측의 $\omega$축에 대한 $X(\omega)$는 푸리에 변환을 한 함수가 됩니다. 그럼 좌측 시간축에 대한 함수를 시간에 대해 미분을 한 결과는 하기와 같이 $j\omega X(\omega)$가 됩니다.   
\begin{aligned}    
x(t) =& \frac{1}{2 \pi} \int_{- \infty}^{\infty} X(\omega) e^{j \omega t} d \omega \newline    
\frac{d}{dt}x(t) =& \frac{1}{2 \pi} \int_{- \infty}^{\infty} j \omega X(\omega) e^{j \omega t} d \omega \newline    
\end{aligned}    

한가지 예를 들어보겠습니다. 1에 대한 푸리에 변환은 방금 전에 배웠던 $e^{j k\omega_0 t}$을 푸리에 변환한 결과에서 $\omega_0$이 0에 위치한 디렉 델타 함수이면 됩니다. 즉, $2 \pi \delta(\omega)$가 됩니다. 그럼 1을 미분한 결과인 0의 푸리에 변환은, 방금 배웠던 미분함수 푸리에 변환을 적용하면, $j\omega 2 \pi \delta(\omega)$인 것을 바로 알 수 있습니다. 
### Impulse Response($h(t)$) 구하기(from FT)
이전 미분방정식 포스팅에서 LCCDE를 배웠던 것을 기억하실 겁니다. 약간 기억을 되짚어 보기 위해 LCCDE의 수식을 하기에 정의해보겠습니다. $x(t)$는 입력, $y(t)$는 출력입니다.   
\begin{aligned}    
\sum_{k=0}^{N} a_k \frac{d^k y(t)}{dt^k} = \sum_{k=0}^{M} b_k \frac{d^k x(t)}{dt^k}
\end{aligned}    

한 번 예시를 들어 풀어보겠습니다.   
\begin{aligned}    
y' + 2y = 3x' + x
\end{aligned}    

상기와 같은 미분 방정식을 푸리에 변환을 통해 Impulse Response($h(t)$)를 구해보겠습니다.    
\begin{aligned}    
y' + 2y =& 3x' + x \newline   
j\omega Y(\omega) + 2Y(\omega) =& 3 j\omega X(\omega) + X(\omega) \newline   
\end{aligned}    

$y(t)$는 출력이니 원래 $y(t)$를 구하기 위해선, 입력과 Impulse Response에 대해 convolution을 취해주면 구할 수 있었습니다. 즉, $y(t) = x(t) \* h(t)$입니다. 근데, 이 식을 푸리에 변환으로 바꿔주면, convolution은 그냥 곱으로 나타나집니다. 따라서, $Y(\omega)= X(\omega)H(\omega)$가 됩니다. 그럼 $H(\omega) = \frac{Y(\omega)}{X(\omega)}$가 됩니다.    
\begin{aligned}    
j\omega Y(\omega) + 2Y(\omega) =& 3 j\omega X(\omega) + X(\omega) \newline   
H(\omega) = \frac{Y(\omega)}{X(\omega)} =& \frac{3 j\omega + 1}{j\omega + 2} \newline   
=& 3 - \frac{5}{j\omega + 2} \newline   
h(t) =& 3\delta(t) - 5 e^{-2t}u(t)
\end{aligned}    

$H(\omega)$를 구해준 후, 그 구한 식에서 다시 역푸리에 변환을 해주면, Impulse Response인 $h(t)$를 구할 수 있습니다.    

## Unit Step Function($u(t)$) FT
단위 계단 함수는 다들 한번씩은 들어보셨을 겁니다. 우선 수식과 함수의 모양은 어떻게 되는지 알아보겠습니다.   
<img src="../../../assets/images/Signals&Systems/2024-03-16-Fourier Transform 3/unit step function.png" alt="unit step function" style="zoom:80%;" />    
만약, $u(t - a)$의 경우, 해당 그래프를 a만큼 평행 이동 시키면 됩니다.   
그럼 이제 $u(t)$에 대해 푸리에 변환을 해보겠습니다.   
근데 그냥 $u(t)$에 대해 하는 것이 아닌 $lim_{a \rightarrow 0} e^{-at}u(t)$에 대해 푸리에 변환을 해보겠습니다.($a$는 양수) 그 이유는, $lim_{a \rightarrow 0} e^{-at}$를 곱해주어도 $a \rightarrow 0$이기 때문에, 결국에 $u(t)$와 유사하게 나타나기 때문입니다.    
\begin{aligned}    
\int_{- \infty}^{\infty} lim_{a \rightarrow 0} e^{-at}u(t) e^{- j \omega t} dt =& lim_{a \rightarrow 0} \int_{0}^{\infty} e^{-at} e^{- j \omega t} dt \newline   
=& lim_{a \rightarrow 0} \frac{e^{-(a + j \omega)t} |_{0}^{\infty}}{-a -j\omega} \newline   
\end{aligned}  


여기서 $e^{-at} e^{-j \omega t}$를 살펴보겠습니다. $a$는 항상 양수이며 $t$값이 증가하면 $e^{-at}$는 0으로 수렴합니다. $e^{-j \omega t}$는 $t$의 값과 상관없이 항상 크기가 1입니다. 따라서, $e^{-at}$에 의해 결국 0으로 수렴하게 됩니다. 계속해서 전개해보겠습니다.   
\begin{aligned}    
lim_{a \rightarrow 0} \frac{e^{-(a + j \omega)t}}{-a -j\omega} \newline   
= lim_{a \rightarrow 0} \frac{1}{a + j\omega} \newline
\end{aligned}  

근데 여기서 $a \rightarrow 0$이지만, $\omega = 0$인 지점에서 $a \rightarrow 0$으로 보내면, 좌극한과 우극한 값이 다르므로, 극한 값이 존재하지 않게됩니다. 따라서 분모, 분자에 $a-j \omega$를 곱해주는 트릭을 취해보겠습니다.    
\begin{aligned}    
lim_{a \rightarrow 0} \frac{1}{a + j \omega} =& lim_{a \rightarrow 0} \frac{1 (a-j \omega)}{(a + j\omega)(a-j \omega)} \newline   
=& lim_{a \rightarrow 0} \frac{a^2}{a^2 + \omega^2} - \frac{j \omega}{a^2 + \omega^2} \newline   
=& f(\omega) + \frac{1}{j \omega}
\end{aligned}    

$lim_{a \rightarrow 0} \frac{a^2}{a^2 + \omega^2}$는 $f(\omega)$라 두고, $a = 0$을 대입했습니다. 그럼 $f(\omega) = f(- \omega)$인 것을 바로 알 수 있습니다.   
따라서, $u(t)$의 푸리에 변환은 $f(\omega) + \frac{1}{j \omega}$인 것을 알 수 있습니다. 그럼 이제 $f(\omega)$를 구해봐야할 것 같습니다.    
\begin{aligned}    
u(t) 's \; FT =& f(\omega) + \frac{1}{j \omega} \newline   
u(-t) 's \; FT =& f(- \omega) - \frac{1}{j \omega} \newline   
u(t) + u(-t) =& f(\omega) + f(- \omega) = 2f(\omega) \newline   
1 =& 2 \pi \delta(\omega) =& 2f(\omega) \newline   
f(\omega) = \pi \delta(\omega)
\end{aligned}    

상기의 수식에서 $u(t) + u(-t) = 1$이라고 정의를 했지만, 사실은 $t=0$인 지점에서는 값을 갖고 있지 않습니다. 근데, $t=0$에서 0.5를 갖는다고 가정을 해보겠습니다. 그럼 $u(t)$의 푸리에 변환식은 $\pi \delta(\omega) + \frac{1}{j \omega}$가 됩니다.   

## 적분의 FT
$\int_{- \infty}^{t} x(\tau) d\tau$에 대해 푸리에 변환을 해보겠습니다.   
우선 $\int_{- \infty}^{t} x(\tau) d\tau$을 풀어보겠습니다. 해당 식을 보면, 음의 무한대에서 $\tau$까지에 대해 적분을 합니다. 그럼 $x(\tau)$ 함수에서 $\tau$ 시점 이전까지는 $x(\tau)$값을 가지고, 이후부터는 0을 가진다면 해당 식을 만족할 수 있을 것 같습니다. 이런 함수는 $u(\tau)$를 평행이동 시키면 나타낼 수 있을 것 같습니다. 한번 $u(t - \tau)$와 $x(\tau)$를 곱해주면, 음의 무한대에서 $\tau$까지의 값만 가지는 것을 알 수 있습니다. 
\begin{aligned}    
\int_{- \infty}^{t} x(\tau) d\tau =& \int_{- \infty}^{\infty} x(\tau)u(t - \tau) d\tau
\end{aligned}   

근데, $\int_{- \infty}^{\infty} x(\tau)u(t - \tau) d \tau$ 이 식은 $x(t) \* u(t)$와 동일합니다.   
\begin{aligned}    
\int_{- \infty}^{t} x(\tau) d\tau =& \int_{- \infty}^{\infty} x(\tau)u(t - \tau) d\tau \newline   
=& x(t) \* u(t)
\end{aligned}  

즉, convolution에 대해 푸리에 변환을 취해주면 됩니다. $X(\omega)U(\omega)$가 되니, 이전에 $U(\omega) = \pi \delta(\omega) + \frac{1}{j \omega}$을 알아보았습니다. 여기에 $X(\omega)$만 곱해주면 되니, $X(\omega)U(\omega) = \pi \delta(\omega)X(\omega) + \frac{X(\omega)}{j \omega}$를 알 수 있습니다.