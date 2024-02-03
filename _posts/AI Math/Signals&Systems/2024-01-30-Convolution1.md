---
title:  "Convlution 1"
categories: SystemsAndSignals
tag: [theory, Systems, Signals]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: Signal 개념, Discrete Time Convolution
comments: true
date: 2024-1-30
toc_sticky: true
---

## LTI System($y(t) = f(x(t), h(t))$
이전 포스팅에서 LTI system이 무엇인지 알아보았습니다. LTI System이란 것을 알게된다면, 우리는 **어떤 다른 입력에 대해 출력이 예측 가능**하다는 점이 가장 중요합니다.  

그럼 LTI 시스템에 대해 수식으로 정의해보겠습니다.   
\begin{aligned}    
y(t) = f(x(t), h(t))   
\end{aligned}   

이전 포스팅인 LTI System 포스팅에서 "500원을 자판기에 넣으면 콜라가 나온다."의 예시를 기억하실 겁니다. 이 자판기가 시스템이되고 또 그냥 시스템이 아닌 LTI 시스템이라고 해보겠습니다. 그럼 2500원을 넣어도 출력이 어떤 것인지 알 수 있습니다. 즉, 자판기 시스템은 상기의 수식에서 $y(t)$에 해당합니다. 그럼 예시 중에 하나인 "500원을 넣어 콜라가 나왔다" 이 것이 $h(t)$에 해당하게 됩니다. 즉, **경험을 통해 어떤 입력을 넣었을 때, 어떤 출력이 나오는지**가 됩니다. 그럼 여기서 $x(t)$는 임의의 돈을 넣는 것을 의미하게됩니다. 그럼 출력은 $y(t)$가 나올 것 입니다.    

### Impulse Response
상기의 수식에서 $h(t)$가 바로 Impulse Response가 됩니다. **Impulse Response는 Impulse를 시스템에 넣었을 때 나오는 출력**입니다. 그럼 여기서 가장 중요한 Impulse가 무엇인가에 대해 알아보겠습니다.   

Impulse를 알기위해 **Impulse function**을 살펴보겠습니다.   
Impulse function은 **시스템에 Impulse function을 넣어주면 출력으로 Impulse Response**를 나오게 해주는 곧 Impulse input이 됩니다. Impulse function의 정의는 **아주 짧은 순간($t \rightarrow 0$)에 시스템에 가해지며, 크기는 1인 단위 입력을 뜻합니다.    
우선 <span style='color:red'>**Discrete Domain(이산 시간)**</span>에서의 Impulse function에 대해 살펴보겠습니다.    
<img src="../../../assets/images/Signals&Systems/2024-01-30-Convolution1/Dirac Delta 1.jpg" alt="Dirac Delta 1" style="zoom:80%;" />    
Discrete Domain에서 Impulse function은 상기와 같은 그래프입니다. 이 그래프는 Dirac Delta Function의 그래프입니다. 상기의 그래프를 수식으로 나타내면 하기와 같습니다.   
\begin{aligned}    
\delta[n] = \begin{cases}   
1 & \text{if } n=0 \newline   
0 & \text{else } n \neq 0(otherwise)
\end{cases}
\end{aligned}   

Impulse function이 하기의 2가지 조건을 만족해야합니다.   
① $\sum_{n = - \infty}^{\infty} \delta [n] = 1$   
② $\sum_{n = - \infty}^{\infty} f[n] \delta[n] = f[0]$   
참고로 ② 조건이 **sampling property**라고 합니다.   

<br>
그럼 이번에는 Continuous Domain에서 Impulse function에 대해 알아보겠습니다. 수식은 하기와 같습니다.   
\begin{aligned}    
\delta(t) = \begin{cases}   
\infty & t=0 \newline   
0 & t = o.w(otherwise)
\end{cases}
\end{aligned}   

그럼 Discrete Domain에서와 동일하게 하기의 2가지 조건을 만족해야합니다.   
① $\int_{t = - \infty}^{\infty} \delta(t) dt = 1$   
② $\int_{t = - \infty}^{\infty} f(t) \delta(t) dt = f(0)$    

그럼 상기의 조건을 만족하는 다양한 Impulse function에 대해 알아보겠습니다.   
<img src="../../../assets/images/Signals&Systems/2024-01-30-Convolution1/Dirac Delta 2.jpg" alt="Dirac Delta 2" style="zoom:80%;" />    
상기의 이미지에 있는 그래프들에서 $\Delta \rightarrow 0$이라고 해보겠습니다.    
그럼 ① 조건인 $\int_{t = - \infty}^{\infty} lim_{\Delta \rightarrow 0}g(t) dt = 1$인 것을 확인할 수 있습니다.   
그럼 ② 조건인 $\int_{t = - \infty}^{\infty} f(t) lim_{\Delta \rightarrow 0} g(t) dt = f(0) \int_{t = - \infty}^{\infty} \text{lim}_{\Delta \rightarrow 0} g(t) = f(0)$인 것을 확인할 수 있습니다. 따라서 상기의 이미지 그래프 두개는 모두 Dirac Delta 함수를 만족하게됩니다. 따라서 두 그래프 모두 Impulse function이 됩니다.   

그럼 이제 Impulse Response이 어떤 것인지 알아보겠습니다.   
<img src="../../../assets/images/Signals&Systems/2024-01-30-Convolution1/Impulse Response 1.jpg" alt="Impulse Response 1" style="zoom:80%;" />    
상기의 그림처럼 입력 $x_1(t)$를 넣으면 출력$y_1(t)$가 나오는 시스템에서 입력으로 $\delta(t)$을 넣으면 출력 $h(t)$가 나오는데, 이 때의 $h(t)$를 Impulse Response라고 합니다. 즉, $y_1(t)$를 그냥 일반적인 입력에 대한 출력이라고 한다면, $h(t)$는 Dirac Delta 함수인 $\delta(t)$가 입력일 때의 출력입니다.   

여기서 가장 중요한 것은 <span style='color:red'>**어떤 Signal이든 Impulse의 합으로 나타낼 수 있다는 점**</span>입니다.

### Convolution
상기에서 정의했던 LTI System의 수식을 다시 확인해보겠습니다.   
\begin{aligned}    
y(t) = f(x(t), h(t))   
\end{aligned}   

여기서 방금까지 $x(t)$와 $h(t)$가 어떤 것인지 알았습니다. 그럼 함수 $f$가 어떤 것인지 알면 출력을 예측할 수 있게 될 것 입니다. 여기서 이 $f$가 바로 **Convolution**을 뜻합니다.   
그럼 Convolution의 사전적 정의부터 살펴보겠습니다.   
> 합성곱, 또는 convolution은 하나의 함수와 또 다른 함수를 반전 이동한 값을 곱한 다음, 구간에 대해 적분하여 새로운 함수를 구하는 수학 연산자입니다.   

조금 더 쉽게 설명을 해보겠습니다. 방금전에 Impulse function(Input)에 대해 설명을 했었습니다. 만약, Impulse Input이 한번이 아니라 시간의 흐름에 따라 지속적으로 시스템에 입력이 되는 상황이라고 가정을 해보겠습니다. $t=0, t=1, t=2, t=3$마다 Impulse Input을 주었다면 시스템의 출력은 어떻게 될까요? $t=0$에서의 Impulse Input에 대한 시스템의 출력이 아직 다 사라지지 않은 상황에서 $t=1$에서 또 다시 Impulse Input을 주게되면 LTI System에서는 각각 출력에 대해 영향을 주게 될 것입니다. 즉, 모두 중첩이 되어 시스템의 출력이 나타날 것 입니다. 즉, <span style='color:red'>**Convolution은 쉽게 말해 누적 된 반응에 대해 계산하는 것**</span>이라고 말할 수 있습니다.   
LTI System을 가진 베개가 있다고 가정을 하고, 이 베개를 누르는 것이 Impulse Input이라고 해보고 예시를 들어 추가적으로 설명해보겠습니다.   
$t=0$일 때, 베개를 눌렀을 때의 그래프를 확인해보겠습니다.   
<img src="../../../assets/images/Signals&Systems/2024-01-30-Convolution1/LTI Convolution 1.jpg" alt="LTI Convolution 1" style="zoom:80%;" />    
상기와 같은 형태로 y축은 베개가 눌린 상태를 의미하며 상기와 같은 그래프의 형태로 나타낼 수 있을 것입니다.   

그럼 베개를 누르는 것인 Impulse Input을 방금전과 다르게 2배로 크게 눌렀다면 어떻게 될까요?   
<img src="../../../assets/images/Signals&Systems/2024-01-30-Convolution1/LTI Convolution 2.jpg" alt="LTI Convolution 2" style="zoom:80%;" />    
상기와 같이 눌린정도가 똑같이 2배가 될 것 입니다. 베개는 LTI를 따르기 때문에 LTI의 Scaling Property를 만족하기 때문입니다.   

그럼 이번에는 $t=0, t=1, t=2$에서 똑같은 크기의 Impulse Input으로 베개를 찔러보겠습니다.   
<img src="../../../assets/images/Signals&Systems/2024-01-30-Convolution1/LTI Convolution 3.jpg" alt="LTI Convolution 3" style="zoom:80%;" />    
우선 베개를 찔렀을 때의 각각의 그래프는 왼쪽의 그래프와 같이 나타날 것 입니다. 하지만, 베개는 LTI를 따르기 때문에 3개의 Input이 모두 중첩된 출력이 나타날 것 입니다. 즉, 오른쪽의 그래프와 같은 출력의 형태가 나타날 것 입니다. 즉, 0초에서 눌러서 베개가 깊에 눌렸다가 다시 원상태로 복구될려고 할 때, 1초가 되어 또 다시 베개를 눌르면 이전 상태에 눌린정도와 다시 추가적으로 눌린 상태가 더해져 더 깊게 눌려지는 현상이 나타납니다. 이런 현상에 대한 것이 **Convolution**이 됩니다.    

Impulse를 설명할 때, 가장 중요한 점인 **Signal은 Impulse의 합으로 나타낼 수 있다고**했던 것을 기억하실 것입니다. 그럼 Impulse을 알면 Impulse Response를 알 수 있습니다. Impulse Response는 $t=0$일 때의 Impulse에 대한 출력을 나타내니, 그럼 $t=1$인 경우는 Impulse을 shift 시키면, $t=1$에 대한 Impulse Response 또한 구할 수 있게됩니다. 즉, <span style='color:red'>**Impulse들로 Signal을 나타낼 수 있으니 Signal에 대한 출력을 나타내는 것 또한 매우 간단해진다**</span>는 의미가 됩니다. <span style='color:blue'>**$h(t)$로 출력 $y(t)$를 표현하면 된다**</span>는 의미가 됩니다.   

#### Discrete Time Domain Convolution
<img src="../../../assets/images/Signals&Systems/2024-01-30-Convolution1/Signal Input 1.jpg" alt="Signal Input 1" style="zoom:80%;" />    
우선 상기의 우측 이미지에서 입력인 $x[n]$을 좌측의 $\delta[n]$을 통해서 표현을 해보겠습니다.   
즉, $x[n]$인 함수를 $\delta[n]$의 함수로 표현을 해보겠다는 의미가 됩니다. 함수를 함수로 표현하는 **Series**를 뜻합니다.   
우선 $n=0$인 지점에서의 $x[n]$을 표현해보겠습니다. $x[0]=3\delta[n]$인 것을 바로 알 수 있습니다. 그럼 $n=1$인 지점인 $x[n]$은 어떻게 나타내면 될까요? $\delta[n]$을 shift시키면 됩니다. 즉, $x[1]=2\delta[n-1]$이 됩니다. 즉, $x[n]$을 $\delta[n]$으로 나타내면 하기와 같이 나타내집니다.   
\begin{aligned}    
x[n] = 3 \delta[n] + 2 \delta[n-1] + \delta[n-2] 
\end{aligned}   

그럼 $x[n]$을 입력으로 넣었을 때 출력이 어떻게 되는지 알아보겠습니다. 우선 방금까지 $x[n]$을 $\delta[n]$으로 나타내었습니다. $\delta[n]$는 Impulse Input이기 때문에, $\delta[n]$의 출력은 Impulse Response가 됩니다. 즉, $\delta[n]$의 출력은 $h[n]$이 됩니다. 그럼 $\delta[n-1]$의 출력은 $h[n-1]$이 될 것입니다. LTI System이기 때문입니다. 즉 ,하기와 같이 나타내집니다.   
<img src="../../../assets/images/Signals&Systems/2024-01-30-Convolution1/Signal Input 2.jpg" alt="Signal Input 2" style="zoom:80%;" />    
\begin{aligned}    
y[n] = 3h[n] + 2h[n-1] + h[n-2] 
\end{aligned}   

$x[n]$에 대한 출력이 $y[n]$이라고 한다면 $y[n]$은 상기와 같은 수식으로 나타낼 수 있습니다. 즉, 이것이 **Convolution**입니다. $x[n]$과 $\delta[n]$과 $h[n]$을 알 때, 출력 $y[n]$을 $h[n]$으로 표현한 것 입니다.    

<br>
<img src="../../../assets/images/Signals&Systems/2024-01-30-Convolution1/Signal Input 3.jpg" alt="Signal Input 3" style="zoom:80%;" />    
상기와 같은 임의 입력인 $x[n]$이 있을 때, 0초에서의 입력 값이 $x[0]$라고 해보겠습니다. 그럼 $x[n]$을 $\delta[n]$으로 나타내면 하기와 같이 나타낼 수 있습니다.   
\begin{aligned}    
x[n] = \ldots + x[-2]\delta[n+2] + x[-1]\delta[n+1] + x[0]\delta[n] + x[1]\delta[n-1] + x[2]\delta[n-2] + \ldots
\end{aligned}   

그럼 이 $x[n]$에 대한 출력은 어떻게 나타나면 될까요? $\delta$를 모두 $h$로 바꾸면 그것이 바로 출력이 됩니다.   
\begin{aligned}    
y[n] = \ldots + x[-2]h[n+2] + x[-1]h[n+1] + x[0]h[n] + x[1]h[n-1] + x[2]h[n-2] + \ldots
\end{aligned}   

<span style='color:red'>**이 $y$를 $h$로 표현한 것이 바로 Convolution**</span>이 됩니다. 그럼 이것을 정리하면 **Discrete Convolution**의 공식은 하기와 같이 정의됩니다.   
\begin{aligned}    
y[n] = \sum_{k= - \infty}^{\infty} x[k] h[n-k]
\end{aligned}   

이 Convolution의 공식에 모두 LTI의 의미가 담겨있습니다. $x[k]$가 <span style='color:blue'>**Scaling**</span>을 의미하고, $n-k$는 <span style='color:blue'>**Time-invariant**</span>을 담고 있으며, $\sum$이 <span style='color:blue'>**Additivity**</span>을 담고 있습니다. 즉, <span style='color:red'>**Convlution이 LTI의 의미를 담고 있으며, Convolution으로 나타내지는 것은 모두 LTI System이라는 것이며, LTI System은 모두ㅏ Convolution으로 나타날 수 있다는 동치를 의미합니다.**</span>
