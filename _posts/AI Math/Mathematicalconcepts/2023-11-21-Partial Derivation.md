---
title:  "편미분(Partial Derivation)"
categories: MathematicalConcepts
tag: [theory, math]
tags: [Jekyll, MathJax]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: 편미분의 의미
comments: true
date: 2023-11-21
toc_sticky: true
---
## 선행 개념
\- **독립 변수** : 다른 변수에 영향을 받지 않는 독립적인 변수   
\- **종속 변수** : 다른 변수에 영향을 받는 변수   
\- **다변수 함수** : 함숫값을 결정할 때 2개 이상의 독립 변수가 필요한 함수   

다변수 함수는 $f(x, y, z, w, ...)$와 같은 형태로 함수를 나타내는데, 이 다변수 함수를 미분하려면 어떻게 해야할까요? 이 때 필요한 것이 **편미분**과 **전미분**입니다. 

## 편미분
편미분은 다변수 함수에서 내가 미분하고 싶은 변수 한 개만 변수를 생각하고 나머지는 모두 상수로 취급한 후 미분하는 방법입니다.   
\begin{aligned}
f_x^{\'}(x, y, z, w, ...) = \frac{\partial f}{\partial x} = \frac{\partial}{\partial x}f(x,y,z,w,....) 
\end{aligned}

함수에 대해 미분한 함수를 도함수라고 부릅니다. 그럼 편미분을 수행한 함수는 **편도함수**라고 부릅니다. 편도함수는 해당 변수에 대해 미분을 했다는 의미로 $f_x^{\'}$로 보통 표기합니다. 기존에 일변수 함수에 대한 미분의 경우에는 $\frac{df}{dx}$로 표기했지만, 다변수 함수의 미분에 대해서는 $\frac{\partial f}{\partial x}$로 표기합니다.   
<img src="../../../assets/images/Mathematicalconcepts/2023-11-21-Partial Derivation/partial derivation 2.png" alt="Graph 1" style="zoom:50%;" />    
상기의 이미지는 일변수 함수에 대한 접선을 나타낸겁니다. 상기의 이미지에서 볼 수 있는 것 처럼, 일변수 함수의 경우 1개의 점에 대해 1개의 접선만 존재합니다. 이 한개의 접선의 기울기가 **순간 변화율**을 의미합니다.   
<img src="../../../assets/images/Mathematicalconcepts/2023-11-21-Partial Derivation/partial derivation 1.png" alt="Graph 2" style="zoom:80%;" />    
하지만, 상기의 이미지는 다변수 함수에 대한 접선을 나타낸겁니다. 다변수 함수를 보면 무수한 접선들이 존재합니다. 이 수많은 접선 중 하나의 접선의 기울기를 구하기 위해, 한 변수를 제외한 나머지 변수들을 상수취급하는 것입니다. 그럼 한 점에서의 **순간 변화율**을 구할 수 있습니다. 

상기의 이미지의 그래프는 $f(x, y) = z = x^2 + xy + y^2$의 함수의 그래프입니다. 이 그래프의 $y=1$로 두었을 때의 접선이 빨갠선 곡선이됩니다.   
\begin{aligned}
f_x^{\'}(x, y) = \frac{\partial f}{\partial x} = 2x + y
\end{aligned}

## 전미분
편미분은 다변수함수에서 1개의 변수의 값이 변화할 때의 변화율을 알기 위해 활용됩니다. 예를 들어, $f(x, y, z)$의 함수에서 $x$에 대해 편미분을 수행하면 $x$의 변화에 따라 $f(x,y,z)$의 변화를 확인한다는 뜻입니다. 만약, 이 모든 변수 $x,y,z$의 변화에 따라 $f(x,y,z)$의 변화를 확인하고 싶다면 어떻게 하면 될까요? 전미분을 이용하면 됩니다.   
\begin{aligned}
\partial f(x, y, z) = \frac{\partial f}{\partial x}dx + \frac{\partial f}{\partial y}dy + \frac{\partial f}{\partial z}dz
\end{aligned}

상기와 같이 전미분은 **각각의 변수에 대해 모두 편미분을 수행한 후 더해주면** 됩니다. 여기서 $dx, dy, dz$는 **증분**이라고 하며, 변화량을 의미합니다. 일변수 함수에 대해 미분을 수행한 함수는 도함수, 편미분은 편도함수, 전미분읜 경우에는 **전도함수**라고 합니다. 
