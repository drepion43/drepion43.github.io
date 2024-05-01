---
title:  "최적화 문제란?"
categories: ConvexProblem
tag: [theory, math]
tags: [Jekyll, MathJax]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: 용어 정리
comments: true
date: 2024-04-11
toc_sticky: true
---

## 최적화 문제란?
최적화 문제는 최종적으로 우리가 얻는 목적 함수(Object Function)의 함수값을 최적화 시키는 파라미터(변수)를 찾는 문제입니다.    
예를 들어, 만약 Object Function인 2차 함수 $y = (x - 3)^2 + 3$이 있다고 하겠습니다. 이때 이 Object Function의 최적화인 최소값은 어떤 것 일까요? 딱 보시면 직관적으로 $x=3$일때 최소값을 가진다는 것을 알 수 있습니다. 여기서 $x$는 위에서 설명했던 파라미터가 되고, $y$는 Object Function이 됩니다. 즉, **Object Function의 최적화를 시켜주는 파라미터를 찾는 문제**가 바로 최적화 문제입니다. 이 Object Function은 다양한 함수가 될 수 있습니다. 예를 들어, 머신러닝이나 딥러닝에서는 Error 함수가 되기도 하며 또한 Probability 함수가 되기도 합니다. Error의 경우는 최소화가 목적이 될 것이고, Probability 함수에서는 확률을 최대화하는 것이 목적이 될 것입니다. 참고로 보통 딥러닝이나 머신러닝에서의 학습은 목적 함수인 Cost Function을 최적화 시키는 방향으로 학습을 진행합니다.

## 용어
\begin{aligned}    
Object Function:& \; Min \; f(x) \newline   
subject \; to :& \; h(x) = 0, \; g(x) \leq 0
\end{aligned}   

상기의 식이 보통 우리가 풀고자 하는 최적화 문제입니다. 여기서 $f(x)$가 바로 object function이 됩니다. subject to는 s.t로 앞으로 표기를 할 예정이고, 주어진 constraint입니다.   
여기서 object function을 최적화 시키는 파라미터인 $x$만 아니라 constraint를 만족하는 모든 $x$들을 **feasible solution**이라고 합니다. 즉, object function을 최소화 시켜주는 후보들이라는 뜻입니다. 또한 이런 feasible solutions들을 모아놓은 집합을 **feasible set**이라고 하며, 이 중 우리가 찾고자하는 최적화를 시켜주는 파라미터를 **optimal solution**이라고 합니다. ($x^{\*} \triangleq argmin_{x} f(x)$)   

## Local Minima & Local Maxima
Local Minima는 우리가 알고있는 극소를 의미합니다. 그럼 당연히 Local Maxima는 극대를 의미합니다.    
저는 고등학생 때, 극대와 극소를 구할 때 미분해서 0이되는 지점을 극대나 극소로 계산을 했었습니다. 하지만, 극대, 극소의 실질적인 의미는 **미분 가능과 관계없이 어떤 지점 주변에서 이 함수를 가장 작거나 크게 만들어주는 지점**입니다.    
<img src="../../../assets/images/ConvexProblem/2024-04-11-Optimization Problem/Optimization Problem 1.jpg" alt="Optimization Problem 1" style="zoom:80%;" />    
상기의 이미지의 두 그래프 모두 극소점을 파란색으로 나타내고 있습니다. 방금 알아본 극소의 개념을 적용하면, 파란색 지점이 주변 지점에서 가장 작은 값이 되니 모두 극소입니다.   
이번에는 어떤 함수를 예시로 설명을 이어가보겠습니다.   
<img src="../../../assets/images/ConvexProblem/2024-04-11-Optimization Problem/Optimization Problem 2.jpg" alt="Optimization Problem 2" style="zoom:80%;" />    
[a, b]구간에서의 극소값과 극대값을 찾아보겠습니다. [a, b]는 이전에 설명했던 최적화를 시켜주는 값들이 존재하는 feasible solution구간입니다. 상기의 그래프에서 주황색 점은 극대를 표현한 것이고, 파란색 점은 극소를 표현한 것입니다. 우선 특인한 3번점을 먼저 살펴보겠습니다. 3번은 극대의 개념에 맞게 주변 값들 중에 가장 큰 값을 가지니 극대가 됩니다. 또 특이한 7번점을 보겠습니다. 7번점 같은 경우에는 7번점 바로 좌측은 끊긴 그래프이며 더 작은 값이 존재합니다. 우측을 보면 큰값이 존재하지만, 즉, 7번즘은 극대나 극소 그 어느 것도 아닌점이 됩니다. 그럼 이 그래프에서 우리가 찾고자하는 optimal solution은 minima일 때는 1번점, maxima일 때는 8번점이 됩니다. 즉 1번점은 Global Minimum이고 8번점은 Global Maximum이 됩니다. 