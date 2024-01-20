---
title:  "오일러 공식(Euler's Formula)"
categories: SystemsAndSignals
tag: [theory, Systems, Signals]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: 오일러 공식
comments: true
date: 2024-1-19
toc_sticky: true
---

## 신호 및 시스템이란?

## Euler's Formula
오일러 공식은 하기와 같습니다.   
\begin{aligned}    
e^{j \theta} = cos (\theta) + j sin(\theta)
\end{aligned}   

이 오일러 공식을 증명하려면, 테일러 급수를 이용하면 됩니다. 근데, 테일러 급수 중에 특이 케이스인 매클로린 급수를 이용하여 유도할 수 있습니다. 그럼 $sin(\theta)$와 $cos(\theta)$를 매클로린 급수를 이용해 나타내보겠습니다.   
\begin{aligned}    
f(x) =& \sum_{n=0}^{\infty} \frac{f^{(n)}(0)}{n\!}x^n \quad (Maclaurin's \; Series) \newline   
sin(x) =& sin(0) + cos(0)x + \frac{-sin(0)}{2}x^2 + \frac{-cos(0)}{3!}x^3 + \ldots \newline   
=& 0 + x - \frac{x^3}{3} + \ldots \newline   
=& \sum_{n=0}^{\infty} (-1)^n \frac{x^{2n+1}}{(2n + 1)\!} \newline  
cos(x) =& cos(0) + (-sin(0))x + \frac{-cos(0)}{2}x^2 + \frac{sin(0)}{3!}x^3 + \ldots \newline   
=& 1 - \frac{x^2}{2} + \frac{x^4}{4} + \ldots \newline   
=& \sum_{n=0}^{\infty} (-1)^n \frac{x^{2n}}{(2n)\!} \newline  
\end{aligned}   

테일러 급수에 대해 좀 더 알고 싶으시다면, 이전에 포스팅한 <https://drepion43.github.io/statistics/Talyer-expansion/>에 들어가시면 테일러 급수의 증명과정 또한 확인해보실 수 있습니다.   
그럼 이번엔 Euler's Formula을 매클로린 급수꼴로 나타내보겠습니다. 근데 여기서 $e^{j\theta}$를 1번 미분, 2번 미분 3번 미분, 4번 미분 결과는 순서대로 $f^{1}(\theta) = je^{j\theta}, f^{2}(\theta) =-e^{j\theta}, f^{3}(\theta) =-je^{j\theta}, f^{4}(\theta) =e^{j\theta}$로 나타나며, 미분 결과가 반복되는 것을 확인할 수 있습니다.      
\begin{aligned}    
e^{j \theta} =& cos (\theta) + j sin(\theta) \newline   
=& 1 + j\theta - \frac{1}{2!}\theta^2 - j\frac{1}{3!}\theta^3 + \frac{1}{4!}\theta^4 - j\frac{1}{5!}\theta^5 \newline   
=& cos(\theta) + jsin(\theta)
\end{aligned}    

여기서 오일러 공식의 매클로린 급수로 표현한 결과가 방금 위에서 매클로린 급수로 표현한 $sin(\theta), cos(\theta)$로 나타낼 수 있다는 것을 확인하실 수 있습니다.    
그럼 이 $e^{j\theta}$를 복소평면(실수부와 허수부의 좌표, Complex Domain)로 나타내면 하기와 같이 표현할 수 있습니다.   
<img src="../../../assets/images/Signals&Systems/2024-01-19-EulerFormula/Euler Coordinate 1.jpg" alt="Euler Coordinate 1" style="zoom:80%;" />    
그럼 $Ae^{j\theta}$에서 $A$는 어떤 역할을 하는지 알아보겠습니다. 쉽게 이해하기 위해 $e^{j\theta}$가 $(cos(\theta), sin(\theta))$의 vector라고 생각을 해보겠습니다. 그럼 이 vector에 $A$을 곱해주면, 방향은 동일하며 크기가 $A$배수만큼 늘어나는 vector가 됩니다. 즉,  $(Acos(\theta), Asin(\theta))$에 위치한 vector가 되는 것입니다. 즉, 하기의 그림과 같이 같은 1차원 선 내에서 왔다갔다는 한다는 뜻이 됩니다.   
<img src="../../../assets/images/Signals&Systems/2024-01-19-EulerFormula/Euler Coordinate 2.jpg" alt="Euler Coordinate 2" style="zoom:80%;" />    
그럼 이번에는 $\theta$를 바꿔보겠습니다. 만약 $\theta=0$이라면, $(cos(0), sin(0) = (1, 0))$의 좌표에 위치하게 됩니다. 그럼 점점 $\theta$를 증가시키면, 크기는 1인 상태로 고정하며, 우리가 알고있는 원의 형태를 그리며 나타내지게 됩니다.    
<img src="../../../assets/images/Signals&Systems/2024-01-19-EulerFormula/Euler Coordinate 3.jpg" alt="Euler Coordinate 3" style="zoom:80%;" />    
그럼 이전에 말했던 $Ae^{j\theta}$에서 $A$는 크기를 바꾸며, $\theta$는 방향을 바꾸게 되니, $Ae^{j\theta}$는 2차원 좌표를 모두 나타낼 수 있는 값이 된다는 의미가 됩니다. 복소수는 실수부보다 더 넓은 수들을 포함하고 있습니다. 따라서, 오일러는 **$Ae^{j\theta}$의 형태인 지수 함수꼴로 우리가 알고 있는 모든 수들을 표현할 수 있다**고 표현을 했습니다.   

### 예제
한번 $(-1)^x$를 $Ae^{j\theta}$로 표현을 해보겠습니다. 우선 -1은 크기가 곧 1이라는 의미가 되니 $A=1$을 의미합니다. 그럼 $x=1$일 때인 $e^{j\theta}=-1$이 되려면, $\theta=\pi$가 되어야 합니다. 그럼 $x=2$라고 한다면, $e^{j\theta}=1$이 되려면, $\theta=2 \pi$가 되어야합니다. 즉 $\pi$앞에 계수로 $x$기 붙으면 된다는 의미가 됩니다. 따라서 $(-1)^x=(e^{j\pi})^x$가 되는데, 여기서 $x$가 자연수라고 한다면, $e^{j\pi x}$가 됩니다.   
만약, $x$가 자연수가 아니라고 한다면, 생기는 문제가 $(e^{j 2\pi})^{\frac{1}{2}}$의 경우, $e^{j\pi}$라면 -1이 되고, $e^{j2\pi} = 1$이니 $1^{\frac{1}{2}}$이 될 수도 있습니다.   
