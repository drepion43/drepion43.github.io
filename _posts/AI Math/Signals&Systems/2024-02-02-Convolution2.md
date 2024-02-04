---
title:  "Convolution 2"
categories: SystemsAndSignals
tag: [theory, Systems, Signals]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: Continuous Time Convolution
comments: true
date: 2024-2-02
toc_sticky: true
---

## Convolution
이전까지 Discrete Time Domain에서의 Convolution에 대해 알아보았습니다. 이번 포스팅에서는 Continuous Time Domain에서의 Convolution에 대해 알아보겠습니다.   
Discrete Time Domain에서의 Convlution은 Discrete Time의 Signal을 Impulse들의 합으로 나타내면 되었습니다. 이와 똑같이 Continuous Time Domain에서의 Convolution은 Continuous Time의 Signal을 Impulse의 합으로 나타내면 됩니다.    

### Continuous Time Domain Convolution
이전 포트싱에서 Continus Time Domain에서의 Impulse function을 기억하실겁니다. 하기와 같은 모양의 그래프이며 수식은 $lim_{\Delta \rightarrow 0} \delta_{\Delta}(t) = \delta(t)$였습니다.   
<img src="../../../assets/images/Signals&Systems/2024-02-02-Convolution2/CT Impulse function 1.jpg" alt="CT Impulse function 1" style="zoom:80%;" />    

그럼 이 Impulse Input을 통해 입력인 $x(t)$를 Impulse들의 합으로 나타내보겠습니다. 하기의 입력인 $x(t)$를 $\delta$들로 표현해보겠습니다.   
<img src="../../../assets/images/Signals&Systems/2024-02-02-Convolution2/CT Impulse 1.jpg" alt="CT Impulse 1" style="zoom:80%;" />    
\begin{aligned}    
x(t) =& \ldots + x(0)\delta_{\Delta}(t)\Delta + x(\Delta)\delta_{\Delta}(t - \Delta)\Delta + x(2\Delta)\delta_{\Delta}(t - 2\Delta)\Delta + \ldots \newline   
=& lim_{\Delta \rightarrow 0} \; \ldots + x(0)\delta_{\Delta}(t)\Delta + x(\Delta)\delta_{\Delta}(t - \Delta)\Delta + x(2\Delta)\delta_{\Delta}(t - 2\Delta)\Delta + \ldots
\end{aligned}   

상기의 수식에서 $x(t)$ 그래프의 위의 점들을 $\delta$로 표현했습니다. 그럼 $\Delta \rightarrow 0$으로 간다면 더 세밀해게 $x(t)$를 표현할 수 있으며, 극한을 취해주면 거의 $x(t)$와 흡사해질 것 입니다. 그럼 상기의 수식을 공식화해보겠습니다.   
\begin{aligned}    
x(t) =& lim_{\Delta \rightarrow 0} \; \ldots + x(0)\delta_{\Delta}(t)\Delta + x(\Delta)\delta_{\Delta}(t - \Delta)\Delta + x(2\Delta)\delta_{\Delta}(t - 2\Delta)\Delta + \ldots \newline   
=& lim_{\Delta \rightarrow 0} \; \sum_{k= - \infty}^{\infty} x(k \Delta) \delta_{\Delta}(t - k\Delta)\Delta \newline   
\end{aligned}   

여기서 $lim$와 $\sum$가 만나 **리만합**으로 인해 적분으로 표현할 수 있습니다. 근데 여기서 중요한점은 $x(t)$그래프의 넓이를 나타내는게 아닌, 상기의 수식은 **$x(t)$의 값들을 나타낸 점**이란 것을 잊어버려서는 안됩니다. 상기의 공식에서 어떤 변수가 변하는지 확인해보겠습니다.   
$lim$와 $\sum$에 의해 변하는 변수들은 $k\Delta$가 됩니다. 하지만, $k$의 경우는 무한한 점이고 $\Delta$는 0으로 가고있습니다. 따라서 $lim_{\Delta \rightarrow 0} k \Delta$는 어떠한 값으로 표현할 수 있게됩니다. 즉, $k$와 $\Delta$는 모두 Discrete Variable인데, <span style='color:blue'>**$k\Delta$가 Continuous Variable**</span>이 됩니다. 그럼 $lim_{\Delta \rightarrow 0} k \Delta \triangleq \tau$라고 하겠습니다. 그럼 $x(t)$는 $\tau$의 Domain에서 하기와같이 표현할 수 있습니다.   
<img src="../../../assets/images/Signals&Systems/2024-02-02-Convolution2/CT tau domain 1.jpg" alt="CT tau domain 1" style="zoom:80%;" />    
따라서, 리만합의 적분이 $\tau$에 대한 적분으로 바뀌게 됩니다.   
\begin{aligned}    
x(t) =& lim_{\Delta \rightarrow 0} \; \sum_{k= - \infty}^{\infty} x(k \Delta) \delta_{\Delta}(t - k\Delta)\Delta \newline   
=& \int_{\tau = - \infty}^{\infty} x(\tau) \delta(t - \tau) d\tau \newline
\end{aligned}   

이제 입력을 $\delta$인 Impulse의 합으로 표현했습니다. 그럼 출력은 이전에 Discrete Time Domain에서의 Convolution과 동일하게, Impulse Input을 Impulse Response로 바꾸면 됩니다. 즉, $\delta$를 $h$로 바꿔주면 Continous Time Domain Convolution의 식이 나타납니다.   
\begin{aligned}    
y(t) =& \int_{\tau = - \infty}^{\infty} x(\tau) h(t - \tau) d\tau \newline   
=& x(\tau) \* h(t)
\end{aligned}   

Discrete과 동일하게 이 Convolution의 공식에 모두 LTI의 의미가 담겨있습니다. $x(\tau)$가 <span style='color:blue'>**Scaling**</span>을 의미하고, $t-\tau$는 <span style='color:blue'>**Time-invariant**</span>을 담고 있으며, $\int$이 <span style='color:blue'>**Additivity**</span>을 담고 있습니다. 즉, <span style='color:red'>**Convlution이 LTI의 의미를 담고 있으며, Convolution으로 나타내지는 것은 모두 LTI System이라는 것이며, LTI System은 모두 Convolution으로 나타날 수 있다는 동치를 의미합니다.**</span>

### $\tau-Domain$
이전까지 $x(t)$를 표현하고 $t-Domain$에서 바라봤습니다. 하지만, $x(t)$를 Impulse들의 합으로 표현하고 Convolution으로 표현하면 적분 Domain은 $\tau-Domain$에서 나타납니다. 그럼 이 $\tau-Domain$에서 해석을 해보겠습니다.   
\begin{aligned}    
y(t) =& \int_{\tau = - \infty}^{\infty} x(\tau) h(t - \tau) d\tau \newline   
=& x(\tau) \* h(t)
\end{aligned}   

상기의 Convolution의 공식을 풀기위해서는 $\tau-Domain$에 대해 $x(\tau)h(t - \tau)$ 그래프를 나타내어 해석하여 해결해야합니다. 즉, 하기의 순서를 통해 해결해야합니다.   
<span style='color:red'>
① $x(t) \rightarrow x(\tau)$로 치환해줍니다.   
② $h(t) \rightarrow h(\tau)$로 치환해줍니다.   
③ $h(\tau) \rightarrow h(- \tau)$로 뒤집어줍니다.   
④ $h(- \tau) \rightarrow h(- (\tau - t))$로 $t$만큼 shift시켜줍니다.   
</span>   

한 번 상기의 순서를 예제를 통해 알아보겠습니다.   
<img src="../../../assets/images/Signals&Systems/2024-02-02-Convolution2/CT Convoltuion ex1.jpg" alt="CT Convoltuion ex1" style="zoom:80%;" />    
상기의 그래프 두 개에서 좌측을 $x(t)$, 우측을 $h(t)$라고 가정하겠습니다. 그럼 현재 $t-Domain$이니 $x$축인 $t$을 $\tau$로 바꿔주면, ①,②인 $\tau-Domain$으로의 변경이 끝났습니다. ③은 우측 그래프를 y축으로 대칭해주면 되지만, 대칭 그래프이기 때문에 뒤집어도 그대로 같은 그래프가 나타납니다. 따라서 현재 그래프의 상태로 ③까지는 끝났습니다. 이제 $t$만큼 shift를 시켜주는 ④을 수행하면됩니다.   
<img src="../../../assets/images/Signals&Systems/2024-02-02-Convolution2/CT Convoltuion ex2.jpg" alt="CT Convoltuion ex2" style="zoom:80%;" />    
$h(t - \tau)$그래프를 이동시키면서 $x(\tau)$와 겹치는 부분에 대해 적분값을 구해주면 됩니다. $h(\tau)$를 $t$만큼 shift시켜주면, $\tau=0$인 지점이 $t$로 변하고 양 끝은 $t-1,t+1$이 됩니다. 그럼 좌측에서 우측으로 이동시키면서 $x(\tau)$ 그래프와 겹치는 지점을 확인할 때, 가장 먼저 만나는 지점은 $x(\tau)$에서는 -1지점과 $h(t - \tau)$에서는 $t+1$지점이 될 것입니다. 즉, $-1=t+1$지점부터 만나기 시작합니다. 즉, $t= -2$지점부터 겹치기 시작합니다. 이때의 넓이는 $t+2$가 됩니다. 그 후 $t=0$일 때는, 완전히 겹쳐지며 넓이는 $2$가 됩니다. 그 후 $t=0$인 지점을 지나 우측으로 이동하면 $t-1$지점이 1을 만날 때까지의 넓이가 $2-t$가 됩니다. 하기의 그래프의 이미지와 결과를 나타내겠습니다.   
\begin{aligned}    
y(t) = \begin{cases}   
t+2 & 0 > t > -2 \newline   
2 & t=0 \newline   
2 - t & 2 > t > 0
\end{cases}
\end{aligned}   

<img src="../../../assets/images/Signals&Systems/2024-02-02-Convolution2/CT Convoltuion ex3.jpg" alt="CT Convoltuion ex3" style="zoom:80%;" />    
따라서 Convolution 결과는 상기의 삼각형 그래프가 나오게 됩니다.