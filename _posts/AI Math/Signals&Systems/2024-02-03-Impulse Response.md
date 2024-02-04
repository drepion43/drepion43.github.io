---
title:  "Impulse Response"
categories: SystemsAndSignals
tag: [theory, Systems, Signals]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: 시스템 분석(Impulse Response를 통해)
comments: true
date: 2024-02-03
toc_sticky: true
---

## Impulse Response
Convolution을 설명할 때, Impulse Response가 어떤 것인지 알아보았습니다. 간단하게 말하면, Impulse Response은 Impulse에 대한 출력이며, $h(t)$로 표기합니다. 그럼 이전까지 배웠던 것을 종합해보면, Impulse Response을 안다면, 출력인 $y(t)$를 Convolution을 통해 구할 수 있습니다. 하지만, $h(t)$에는 시스템에 대한 많은 정보를 내포하고 있기 때문에 $h(t)$를 안다면 그 시스템이 어떤 시스템인지 분석을 할 수 있습니다. 이번 포스팅에서는 $h(t)$가 시스템에 어떤 정보들을 담고 있는지 알아보도록 하겠습니다.   
### System Analysis
우선 $h(t)$는 Impulse가 들어온 출력이 됩니다. 하기의 그림을 통해 알아보겠습니다.   
<img src="../../../assets/images/Signals&Systems/2024-02-03-Impulse Response/Impulse Response Analysis 1.jpg" alt="Impulse Response Analysis 1" style="zoom:80%;" />    
상기의 그래프에서 처럼 딱 0초에서 입력이 들어왔다고 하면, 우측의 주황색 선인 출력은 입력이 들어온 후의 출력이 됩니다. 하지만, 파란색선의 출력의 경우에는, 0초에 입력이 들어오는데, 입력이 들어오기 전에 출력이 나타난 현성압니다. 즉, 입력이 들어오기전에 미리 출력이 나타난 현상입니다. 예를 들어, 복싱을 했을 때, 주먹이 어디로 날라올지 예측을 하고 미리 몸을 날라오는 방향과 반대 방향으로 피하는 것과 비슷하다고 생각하시면 이해가 빠르게 되실 것 같습니다. 즉, **파란색은 미래의 입력에 의해 영향을 받았다**고 불 수가 있습니다. 빨간색 출력의 경우에는 Impulse를 준 순간인 0초에 바로 출력이 나타나게 됩니다. 

<br>
<span style='color:blue'>**Memoryless**</span>   
우선 **Memoryless**을 해석해보면 기억이 없다가 됩니다. 즉, 시스템에 입력이 들어왔을 때, 그 순간에 출력이 나오고 그 이외에는 어떤 출력이 나오지 않는다는 의미가 됩니다. 즉, **입력이 들어간 순간마다만 출력이 나온다**입니다.   
그럼 이 3개의 출력 중에 Memoryless는 0초에 입력이 들어왔을 때, 0초에서 출력이 나타나고 사라진  빨간색 출력이 될 것 입니다.   
<br>
<span style='color:blue'>**Causal System**</span>   
그럼 이번에는 **Causal System**이 어떤 것인지 상기의 $h(t)$의 출력을 통해 알아보겠습니다. 다들 아시겠지만, Causal을 다시 간단하게 정의하면 과거나 현재의 입력에 의해서만 영향을 받은 시스템을 말합니다. 그럼 과거의 입력에 의해 영향을 받은 시스템은 빨간색 출력과 주황색 출력이 될 것입니다. 0초인 과거나 현재의 입력에 의해 출력이 나타난 형태이기 때문입니다.   
<br>
<span style='color:blue'>**BIBO stable System**</span>   
이번에는 그럼 BIBO(Bounded Input Bounded Output)인 stable system인지에 대해 알아보겠습니다. BIBO stable하다는 것은 입력인 $|x(t)| < \infty$과 같이 발산하지 않고 수렴하는 입력이 System에 들어갔을 때, 출력 또한 발산하지 않고 수렴하는 $|y(t)| < \infty$인 System을 말합니다. 여기서 중요한 점은 Bouned한 Input이 들어갔을 때만에 대해 정의할 수 있습니다. 만약 입력 자체가 발산하는 입력이 들어간다면 BIBO System을 얘기할 수가 없습니다. 그럼 이제 LTI System에서 어떻게 판별하는지를 알아보겠습니다.   
\begin{aligned}    
| y(t) | =& | \int_{\tau = - \infty}^{\infty} x(\tau) h(t - \tau) d\tau | \newline   
\ge& \int_{\tau = - \infty}^{\infty} | x(\tau) h(t - \tau) | d\tau \newline   
=& \int_{\tau = - \infty}^{\infty} | x(\tau) | | h(t - \tau) | d\tau
\end{aligned}   

여기서 $| x(\tau) h(t - \tau) |$이 $| x(\tau) | | h(t - \tau) |$와 같은 이유를 증명해보겠습니다. 오일러 공식을 생각하면 $|A_1e^{j \theta_1} A_2 e^{j \theta_2}| = |A_1 A_2 e^{j (\theta_1 + \theta_2)}| = A_1A_2$가 됩니다. 이 식이 전체에 절댓값을 취해준 결과가 되고, 그럼 각각에 절댓값을 취해준 것은 $|A_1e^{j \theta_1}| | A_2 e^{j \theta_2}| = A_1 A_2$의 결과가 나와 동일하게 됩니다.   
그럼 이어서 BIBO에 대해 알아보겠습니다.   
\begin{aligned}    
| y(t) | =& | \int_{\tau = - \infty}^{\infty} x(\tau) h(t - \tau) d\tau | \newline   
\ge& \int_{\tau = - \infty}^{\infty} | x(\tau) h(t - \tau) | d\tau \newline   
=& \int_{\tau = - \infty}^{\infty} | x(\tau) | | h(t - \tau) | d\tau \newline   
<& B \int_{\tau = - \infty}^{\infty} | h(t - \tau) | d\tau \newline   
\end{aligned}   

입력인 $x(t)$들 중 가장 큰 입력보다 더 큰 것을 $B$라고 한다면 상기와 같이 대소관계가 나타날 것 입니다. 여기서 $t$는 상수로 되고 적분은 $\tau$에 대해 하는 것이니 만약 $\int_{- \infty}^{\infty} |x(t)| dt < \infty$라고 한다면 $B \int_{\tau = - \infty}^{\infty} | h(t - \tau) | d\tau$도 수렴을 하게 될 것 입니다. 즉, 제일 큰값이 수렴을 하게 되니 그 보다 작은 값들도 당연히 수렴을 하게 되어 Bounded Input이라는 것이 증명이 됩니다.   
\begin{aligned}    
\int_{- \infty}^{\infty} |x(t)| dt < \infty \rightarrow BIBO
\end{aligned}   

상기와 같은 것을 알 수 있습니다. 하지만, 이 둘은 서로 **동치**입니다. 이번에는 반대로인 $\int_{- \infty}^{\infty} |x(t)| dt < \infty \leftarrow BIBO$인 것을 증명해보겠습니다.   
대우인 $\int_{- \infty}^{\infty} |x(t)| dt = \infty$라면 BIBO가 아니다가 맞다면 증명이 될 것 입니다. 즉, **Bounded Input에 대해 UnBounded Output인 것이 하나라도 존재한다면, 대우 증명**이 됩니다.   
<img src="../../../assets/images/Signals&Systems/2024-02-03-Impulse Response/BIBO System 1.jpg" alt="BIBO System 1" style="zoom:80%;" />    
상기의 그림과 같은 $x(\tau)$라면, $h(t- \tau)$값이 음수 일 때, -1을 곱해줘 항상 양수로 만들어 줄 것 입니다. 따라서 하기와 같이 표현할 수 있습니다.   
\begin{aligned}    
|y(t)| =& | \int_{- \infty}^{\infty} x(t) h(t - \tau )d\tau | \newline   
=& \int_{- \infty}^{\infty} |h(t - \tau )| d\tau = \infty
\end{aligned}   

$\int_{- \infty}^{\infty} |h(t - \tau )| d\tau$는 $\int_{- \infty}^{\infty} |h(t)| dt$와 같다고 상기에서 증명을 통해 알아보았습니다. 근데, $\int_{- \infty}^{\infty} |h(t)| dt = \infty$라고 대우의 증명을 위해 가정을 했으니, 결과적으로 $y(t)$도 발산을 하게 된다는 것을 알 수 있습니다. 즉, Bounded Input인 $x(\tau)$을 넣어도 Unbounded Input인 $y(t)$가 나오는 경우를 찾을 수 있으니 두 개가 동치인 것을 알 수 있습니다.   
\begin{aligned}    
\int_{- \infty}^{\infty} |x(t)| dt < \infty \Leftrightarrow BIBO
\end{aligned}   
