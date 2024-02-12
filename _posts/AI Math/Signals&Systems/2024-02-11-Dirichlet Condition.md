---
title:  "디리클레 조건(Dirichlet Condition)"
categories: SystemsAndSignals
tag: [theory, Systems, Signals]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: Fourier Series로 보는 Dirichlet Condtition
comments: true
date: 2024-02-11
toc_sticky: true
---

## Convergene of CTFS(Continuous Time Fourier Series)
CTFS의 Convergence에는 Dirichlet Condtition과 Energy Condition이 있습니다. 

### Energy
\begin{aligned}    
\int_{T_0} | x_{T_0}(t) |^2 dt <& \infty \newline   
\tilde{x_{T_0}(t)} =& \sum_{k = -N}^{N} a_k e^{jk \omega_0 t} \newline   
\int_{T_0} | x_{T_0}(t) - \tilde{x_{T_0}(t)} |^2 dt & \quad (N \rightarrow \infty) \rightarrow 0 
\end{aligned}  

표현하고 싶은 주기함수의 "에너지가 유한하다"고 해보겠습니다. 그럼 $k$를 일부인 $-N ~ N$까지만 가지고 만든 주기함수를 $\tilde{x}$라고 하겠습니다. 그럼 이 일부만 가지고 만든것과의 차의 에너지는 $N \rightarrow \infty$일 경우 0으로 가게됩니다. 근데 여기서 에너지의 차(gap)이 0으로 간다는 것이 $x_{T_0}(t) = \tilde{x_{T_0}}(t)$을 의미하지는 않는 것 입니다.    

### Dirichlet Condition
상기의 설명에 이어서 $x_{T_0}(t) = \tilde{x_{T_0}}(t)$은 아니라고 설명을 했습니다. 근데 Dirichlet Condtition은 $x_{T_0}(t) = \tilde{x_{T_0}}(t)$이 되는 조건을 알아보는 것 입니다. 모든 점 $t$에서 $x(t)$와 $\tilde{x(t)}$가 같아지는 조건에 대해 알아보는 것 입니다. 

<br>
푸리에 급수는 cos과 sin 함수의 무한한 합으로 이루어져 있습니다. cos과 sin은 언제나 주기성을 가지고, 연속적으로 매끄럽게 이어진(불연속점이 없는) 연속함수입니다. 하지만, 모든 주기함수들이 연속함수이지는 않습니다. 만약 불연속 주기함수도 푸리에 급수에 따르면 정의가 되어야하는데, 계단함수와 같은 불연속 주기함수는 sin과 cos인 연속 주기함수들의 선형결합으로 표현이 가능한지를 알아봐야합니다. 또한, 모든 구간에서 연속인 주기함수라고 해도 sin과 cos의 급수표현 방식인 이산적인 선형결합으로 어떻게 불규칙한 주기 파동을 나타낼 수 있는지도 알아봐야합니다.    
이번에 어떤 함수 $f(x)$를 어떻게 이산적인 값들의 합으로 나타낼 수 있는지에 대한 의문이 들 수 있습니다. 그럼 여기서 Dirichlet Condition은 $f(x)$를 푸리에 급수로 전개했을 때와 기존 $f(x)$와 같기 위한 조건이 됩니다.    

주기가 $2\pi(-\pi, \pi)$인 함수 $f(x)$가 다음 조건을 모두 만족한다면 $f(x)$는 상기의 $x_{T_0}(t) = \tilde{x_{T_0}}(t)$에서 모든 $t$점들과 같다는 것을 의미하게됩니다. 즉, $f(x)$를 Piecewise regular라고 부르며 $f(x)$를 푸리에 급수로 표현할 수 있게됩니다.    
① 구간$(-\pi, \pi)$에서 (다가함수가 아니라) 일가함수(Single-valued)이다.
② 불연속적인 점의 개수와 최대값 및 최소값의 개수가 유한해야 합니다.    
③ $\int_{-\pi}^{\pi} | f(x) | dx < \infty$입니다. 즉, 에너지가 유한하다의 조건이어야합니다.   

이 조건들을 '디리클레 조건(Dirichlet condtion)'이라 부릅니다.
<br>

①은 다가함수가 아니어야한다 입니다. 다가함수란 한개의 $x$값에 대해 $y$값이 2개이상인 것을 의미합니다. 우리가 알던 함수의 정의에 다가함수는 부합하진 않지만, 푸리에 급수는 복소수 영약까지 표현을 하기 때문에, 복소수 영역에서는 다가함수가 존재하기도 합니다. 따라서, 다가함수를 배제하는 조건이 필요합니다.    
②은 최대나 최소가 무한대 혹은 음의 무한대로 발산하는 장소가 생기면 안된다는 겁니다. 푸리에 급수는 sin과 cos 함수의 선형결합으로 만드는데, 발산 점근선 지점처럼 무한한 곳으로 발산하는 지점을 만들 수는 없을 것입니다. 즉, 발산 점근선처럼 발산하는 지점이 구간 내에 포함되면 그건 기본 주기함수 재료인 사인과 코사인을 합쳐도 제작해낼수가 없다는 것는 의미입니다.   
③도 동일하게 절댓값을 붙여 적분한 함수의 면적이 발산하면 안된다는 것입니다. 유한하다는 뜻은 결국 발산하면 안된다는 것 입니다.   
이러한 조건들을 Dirichlet Condition이라고 부르며, **Dirichlet Condition을 만족하면 푸리에 급수로 나타낸 급수와 원래 주기함수와 같다는 것이 증명**됩니다.