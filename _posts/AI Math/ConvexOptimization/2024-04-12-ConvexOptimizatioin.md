---
title:  "Convex Optimization Problem"
categories: ConvexProblem
tag: [theory, math, Optimization]
tags: [Jekyll, MathJax]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: Convex Problem이란?
comments: true
date: 2024-04-12
toc_sticky: true
---

## Convex Optimization Problem
우선 가장 중요한 점인 Convex Optimization Problem을 왜 푸는지에 대해 먼저 말씀드리겠습니다. 어떤 Problem을 Convex Problem으로 바꿀만 수 있다면, <span style='color:red'>**Local Minimum이 Global Minimum**</span>이 됩니다. 즉, local minimum을 구하면 해당 목적 함수(Object Function)의 최적화 값을 찾을 수 있다는 의미가 됩니다.   
일단 Convex Optimization Problem의 정의를 알아보겠습니다.   
<span style='color:red'>**목적 함수(Object Function)인 $f(x)$가 Convex이고 feasible set이 Convex이라면 이 Problem은 Convex Optimization Problem**</span>입니다.    
\begin{aligned}    
Min& \; f(x) \newline   
s.t : \; h(x) =& 0, \; g(x) \leq 0
\end{aligned}    

그럼 일단 Function에서의 Convex의 정의와 Set에서의 Convex정의를 알아보겠습니다.   

<span style='color:blue'>**Convex Set**</span>   
<img src="../../../assets/images/ConvexProblem/2024-04-12-ConvexOptimizatioin/Convex set 1.jpg" alt="Convex set 1" style="zoom:80%;" />    
Convex Set은 모든 점들인 $\underline{x_1}, \underline{x_2} \in S$이고 $\alpha \underline{x_1} + (1 - \alpha)\underline{x_2} \in S$이라면 Convex Set입니다. 즉, **어떤 임의 점이 집합 $S$에 들어가 있고, 또 그 점들의 weighted sum($\alpha \in [0,1]$)도 집합 $S$에 들어가 있다면, 그 집합은 Convex Set**이 됩니다.   
<img src="../../../assets/images/ConvexProblem/2024-04-12-ConvexOptimizatioin/Convex set 2.jpg" alt="Convex set 2" style="zoom:80%;" />    
그럼 상기와 같이 $\alpha \underline{x_1} + (1 - \alpha)\underline{x_2}$이 왜 weighted sum인지 한번 알아보겠습니다.   
$\alpha \underline{x_1} + (1 - \alpha)\underline{x_2}$를 전개해보면 $\underline{x_2} + \alpha (\underline{x_1} - \underline{x_2})$인 것을 알 수 있습니다. 
<img src="../../../assets/images/ConvexProblem/2024-04-12-ConvexOptimizatioin/Convex set 3.jpg" alt="Convex set 3" style="zoom:80%;" />    
상기의 그림에서 확인할 수 있듯이 $\alpha (\underline{x_1} - \underline{x_2})$는 $\alpha$값에 따라 직선에서의 점의 위치가 바뀝니다. 만약 $\alpha=1$이라면, $\underline{x_2}$에서 $\underline{x_1}$까즹 점을 의미합니다. 즉, 0~1사이의 모든 $\alpha$값들을 대입해서 다 묶어보면 하나의 $x_2$와 $x_1$을 잇는 직선을 모두 표현할 수 있습니다.   

<span style='color:blue'>**Convex function**</span>    
이번에는 Convex Function의 정의에 대해 알아보겠습니다.    
<img src="../../../assets/images/ConvexProblem/2024-04-12-ConvexOptimizatioin/Convex function 1.jpg" alt="Convex function 1" style="zoom:80%;" />    
상기의 그림은 간단한 Convex function인 2차함수로 에를들어 설명하겠습니다. 그럼 이 2차함수에서 $x_1, x_2$가 모두 정의역 중에 하나이고 $\alpha \in [0, 1]$이면서, $f(\alpha x_1 + (1-\alpha)x_2) \ leq \alpha f(x_1) + (1-\alpha)f(x_2)$을 만족한다면 $f(x)$는 Convex function입니다.    
<img src="../../../assets/images/ConvexProblem/2024-04-12-ConvexOptimizatioin/Convex function 2.jpg" alt="Convex function 2" style="zoom:80%;" />    
상기의 그림 중 하기의 그래프에서 삼각형 비를 그려 확인해보면, 주황색 지점의 y축 값이 $f(\alpha x_1 + (1-\alpha)x_2)$가 되고, 하늘색 지점의 값은 $(1 - \alpha)(f(x_2) - f(x_1)) = \alpha f(x_1) + (1-\alpha)f(x_2)$을 알 수 있습니다. 즉, 두 점에 대해서 직선을 그은 값들의 집합들이 각 함수 값들의 집합보다 크거나 같아야 Convex function의 조건을 만족하게됩니다.    

추가적으로 Convex Function인지 쉽게 알아보는 방법에 대해서도 말씀드리겠습니다.    
일단 만약, $f(x)$가 전구간에서 미분이 가능하고, $f''(x) \geq 0$이라면, Convex Function과 동치입니다. 즉, 모든 $x$에 대해 $f''(x) \geq 0 \Leftrightarrow Convex \; Function$이라는 의미입니다.    
이 정의는 vector에 대해서도 가능합니다. 즉, scalr를 vector로 두번 미분을 한다는 뜻이되고 이 결과를 **Hessian Matrix**라고 부릅니다. 즉, <span style='color:red'>**Hessian Matrix가 p.s.d(Positive Semi- Definition)을 만족한다면 Convex function과 동치**</span>입니다. p.s.d는 이전 선형대수학 쪽에서 약간의 개념을 짚긴했지만, 다시 되새겨보면 임의 실수를 담은 vector $\underline{z}$가 있다고 했을 때, Hessian Matrix $H$와 $\underline{z}^T H \underline{z} \geq 0$의 조건을 만족하면 p.s.d입니다.    

## Local Minimum = Global Minimum
방금전에 <span style='color:red'>**Local Minimum이 Global Minimum**</span>라고 했습니다. 즉, Convex Problem으로 정의만 할 수 있다면, Global Minimum을 찾는 것은 아주 쉬워진다는 의미입니다.    
근데 진짜 Local Minimum이 Global Minimum이 되는지에 대해 의문을 가지실 수 있습니다. 이것에 대해 왜 그런지 증명을 해보겠습니다.    
먼저 Local Minimum인 $x^{\*}$을 찾았다고 해보겠습니다. 그럼 이에 대해 귀납적 추론을 이용하여, $x^{\*}$보다 더 작은 Local Minimum인 $x'$가 있다고 가정을 해본후 증명해보겠습니다. 우선 $x'$과 $x^{\*}$은 모두 feasible solution이기 때문에, 두 solution 모두 feasible set에 포함됩니다.    
\begin{aligned}    
f(x') <& f(x^{\*}), \; \alpha x^{\*} + (1 - \alpha)x' \in Feasible \; set \newline   
f(\alpha x^{\*} + (1 - \alpha) x') \leq& \alpha f(x^{\*}) + (1 - \alpha)f(x') \newline   
\alpha f(x^{\*}) + (1 - \alpha)f(x') =& f(x^{\*}) + (1 - \alpha) (f(x') - f(x^{\*})), \quad (f(x') - f(x^{\*}) < 0) \newline   
f(x^{\*}) + (1 - \alpha) (f(x') - f(x^{\*})) < f(x^{\*}) \newline   
\end{aligned}    

상기의 증명과정에서 만약 $\alpha \rightarrow 1$로 매우 근접한 값이라고 생각해보겠습니다. 그럼 $f(\alpha x^{\*} + (1 - \alpha) x') \triangleq f(\alpha x^{\*})$가 됩니다. 근데, $x^{\*}$는 Local Minimum이라고 했습니다. Local Minimum의 정의는 해당 값 주위에서 가장 작은 값이라고 했는데, 근데 local minimum인 $f(\alpha x^{\*} + (1 - \alpha) x') < f(x^{\*})$이 나오게 된다는 의미는 Local Minimum의 정의와 맞지 않게됩니다. 그럼 이전에 전제인 $f(x') < f(x^{\*})$에 의해서 모순이 됩니다. 
