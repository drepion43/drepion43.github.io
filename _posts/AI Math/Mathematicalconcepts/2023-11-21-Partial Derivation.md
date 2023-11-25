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
## 개요
\- **독립 변수** : 다른 변수에 영향을 받지 않는 독립적인 변수   
\- **종속 변수** : 다른 변수에 영향을 받는 변수   
\- **다변수 함수** : 함숫값을 결정할 때 2개 이상의 독립 변수가 필요한 함수   

다변수 함수는 $f(x, y, z, w, ...)$와 같은 형태로 함수를 나타내는데, 이 다변수 함수를 미분하려면 어떻게 해야할까요? 이 때 필요한 것이 **편미분**과 **전미분**입니다. 

## 편미분
편미분은 다변수 함수에서 내가 미분하고 싶은 변수 한 개만 변수를 생각하고 나머지는 모두 상수로 취급한 후 미분하는 방법입니다.   
\begin{aligned}
f'(x, y, z, w, ...) = \frac{\partial f}{\partial x} = \frac{\partial}{\partial x}f(x,y,z,w,....) 
\end{aligned}

