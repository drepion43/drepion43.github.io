---
title:  "푸리에 급수(Fourier Series)2"
categories: SystemsAndSignals
tag: [theory, Systems, Signals]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: Fourier Series
comments: true
date: 2024-02-09
toc_sticky: true
---

## 푸리에 급수(Fourier Series)
이번 포스팅에서는 크게 2가지에 의문에 대해 다뤄보겠습니다.   
① <span style='color:red'>**푸리에 급수로 모든 주기함수를 표현할 수 있는가?**</span>  
② <span style='color:red'>**주기함수와 푸리에 급수는 1대1 대응인가?**</span>  

## $e^{jk \omega_0 t}$
$e^{jk \omega_0 t}$가 vector인지 알아보겠습니다. 그럼 먼저 vector의 정의에 대해 알아보겠습니다.   
vector란 vector space내에 존재하는 element들이 vector입니다. 그럼 여기서 vector space란 무엇일까요? **vector space는 더하기와 스칼라배가 정의되어 있고 정의된 더하기와 스칼라배에 대하여 닫혀있으며(closed) 위 8가지 공리(axiom)를 만족하는 집합**입니다. 여기서 vector의 더하기와 스칼라배에 대해 닫혀있다는 것은 하기와 같습니다.   
① $\underline{v_1} + \underline{v_2} \in Set$   
② $a \underline{v_1} \in Set$   
여기서 8가지의 공리는 하기와 같습니다.   
vector 더하기에 대해 하기의 ①~④을 만족해야합니다.   
① 교환법칙이 성립합니다. ($\underline{x} + \underline{y} = \underline{y} + \underline{x}$)    
② 결합법칙이 성립합니다. ($(\underline{x} + \underline{y}) + \underline{z} = \underline{y} + (\underline{x} + \underline{z})$)  
③ 항등원이 존재합니다.($\underline{x} + \underline{0} = \underline{x}$)   
④ 역원이 존재합니다. ($\underline{x} + (-\underline{x}) = \underline{0}$)   
vector의 스칼라배에 대해 하기의 ⑤~⑧을 만족해야합니다.   
⑤ 분배법칙이 성립합니다. ($k(\underline{x} + \underline{y}) = k\underline{y} + k\underline{x}$)   
⑥ 분배법칙이 성립합니다. ($(k + l)\underline{x} = k\underline{x} + l\underline{x}$)  
⑦ 결합법칙이 성립합니다. ($k (l\underline{x}) = (kl)\underline{x}$) 
⑧ 항등원이 존재합니다. ($1\underline{x}= \underline{x}$)  

<br>
그럼 $e^{jk \omega_0 t}$가 vector space의 더하기와 스칼라배에 대해 닫혀있는지 확인해보겠습니다. 우선 $k_1 e^{jk_1 \omega_0 t} + k_2 e^{jk_2 \omega_0 t}$와 $k_1 e^{jk_1 \omega_0 t}$가 모든 복소수 $k_1, k_2$에 대한 것들을 모두 하나의 set으로 집어넣보겠습니다. 즉, 모든 복소수 $a_k$와 $\omega_0$에서 $f_0$도 모든 실수에 대하여 $\sum_{k = - \infty}^{\infty} a_k e^{j k \omega_0 t}$으로 만들 수 있는 모든 집합을 만들어 보겠습니다.    
<img src="../../../assets/images/Signals&Systems/2024-02-09-Fourier Series2/Vector Space 1.jpg" alt="Vector Space 1" style="zoom:80%;" />    
그럼 이렇게 모은 집합을 Vector Space라고 할 수 있습니다. 그리고 $e^{jk \omega_0 t}$은 Vector space에 존재하는 element가 되니, Vector라고 할 수 있습니다.   

### Inner Product Sapce
근데 여기서 이 **Vector Space는 Inner Product Space**라고 할 수 있습니다. Inner Product Space는 Vector Space에서 Inner Product를 정의할 수 있으면, Inner Product Space라고 합니다. 그럼 Inner Product에 대해 알아보겠습니다. 위키피디아를 참고하여 설명해보겠습니다.   

Vector space $V$에 대하여 함수   
\begin{aligned}    
<. , .> : V \times V \rightarrow C
\end{aligned}    

가   
1. Conjugate symmetry:    
    \begin{aligned}    
    \<x,y\> = \overline{<y, x>}
    \end{aligned}   
   
2. Linearity in the first argument :   
    \begin{aligned}    
    <ax + by, z> = a<x,z> + b<y,z>   
    \end{aligned}   
   
3. Positive-definiteness :    
    \begin{aligned}    
    <x,x> \quad \>& \quad 0 \newline   
    <x,x> =& 0 \Leftrightarrow x=0  
    \end{aligned}   

<br>

이렇게 inner product set을 얘기할 수 있으면 가장 좋은 점은 **길이와 각도를 애기할 수 있다**입니다. 즉, 주기함수인 $e^{jk \omega_0 t}$인 sinusoid에 대해 크기와 길이를 정의할 수 있습니다.   

### Orthognoality of Sinusoids   
이번에는 $e^{jk \omega_0 t}$인 sinusoid들이 inner product space에 들어가니 각도를 알 수 있습니다. 각도를 알면 sinusoids들끼리 수직인지도 살펴볼 수 있습니다. 이 sinusoid들끼리 수직이라면 서로 **independent**한 것을 확인할 수 있습니다. 우선 inner product의 정의인 수식은 하기와 같습니다.   
\begin{aligned}    
<a(t), b(t)> \triangleq \int_{T_0} a(t)b^{\*}(t) dt
\end{aligned}   

그럼 임의의 두 개의 sinusoids를 뽑아 inner product를 취해보겠습니다.   
\begin{aligned}    
<e^{jk \omega_0 t}, e^{jr \omega_0 t}> =& \int_{T_0} e^{j(k-r) \omega_0 t} dt \newline   
=& \int_{T_0} cos(k-r)\omega_0 t + j sin(k-r) \omega_0 t dt \newline   
=& \begin{cases}   
0 & k \neq r \newline   
T_0 & k=r
\end{cases}
\end{aligned}   

상기의 inner product를 Euler's Formula를 통해 적분을 해주면 됩니다. 여기서 $k$와 $r$은 모두 정수이며, 서로 다를 때는 $k-r$도 정수가 될 것 입니다. 여기서 $T_0$는 주기를 나타내기 때문에, 만약 $k-r =1$일 경우 하기와 같은 파란색 그래프, $k-r=2$일 경우 하기와 같은 주황색 그래프가 나타나 똑같이 적분을 하게되면 0이 나오게됩니다.    
<img src="../../../assets/images/Signals&Systems/2024-02-09-Fourier Series2/Fourier Series 1.jpg" alt="Fourier Series 1" style="zoom:80%;" />    
그럼 $k \neq r$일 때, 내적값이 0이라는 것은 두 개가 <span style='color:red'>**Orthogonal**</span>하다는 점입니다.
<img src="../../../assets/images/Signals&Systems/2024-02-09-Fourier Series2/Fourier Series 2.jpg" alt="Fourier Series 2" style="zoom:80%;" />    

<br>
\begin{aligned}    
\sum_{k= -\infty}^{\infty} a_k e^{j k \omega_0 t}
\end{aligned}   

상기의 수식은 $e^{j k \omega_0 t}$들이 서로 수직하니 서로 indepedent합니다. 그럼 $k$가 무한대이니 이런 무한대 차원들을 무한대로 표현하는 상기의 수식은 무한한 차원에 대해 모두 표현할 수 있게 된다는 의미입니다. 근데 여기서 만약 특정한 점을 상기의 수식으로 표현하려고 한다면 $e^{j k \omega_0 t}$가지고는 표현할 수 없습니다. 앞에 달려있는 Contributor인 $a_k$가 기여할만큼의 값을 줘야 특정한 좌표를 표현할 수 있게됩니다.   
즉, 서로 Orthogonal한 두 vector $\begin{bmatrix} 1 \newline 0 \end{bmatrix}, \begin{bmatrix} 0 \newline 1 \end{bmatrix}$이 있을 때, $(3, 5)$의 좌표를 나타내려면 각각의 vector앞에 3과 5만큼의 contributor가 필요합니다. 즉 상기의 vector $\begin{bmatrix} 1 \newline 0 \end{bmatrix}, \begin{bmatrix} 0 \newline 1 \end{bmatrix}$가 basis라고 생각을 한다면 이 두 vector $e^{j k \omega_0 t}$와 빗대어 생각을 하시면 됩니다. 즉, $e^{j k \omega_0 t}$가 basis vector가 되고 $a_k$가 contributor가 되어 모든 점을 표현할 수 있게 됩니다. 
