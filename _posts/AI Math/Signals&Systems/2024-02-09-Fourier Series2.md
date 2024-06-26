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

## 의문
### 모든 주기함수 표현
상기와 같이 서로 Orthogonal한 무하한 vector들로 이루어진 basis를 표현하고 있기 때문에, 거의 대부분의 모든 주기함수를 다 표현할 수 있습니다.   
하지만, 이전에 vector space를 정의할 때, $\sum_{k= -\infty}^{\infty} a_k e^{j k \omega_0 t}$로 만들어질 수 있는 모든 vector들을 가지고 set을 만들어 vector space를 정의했습니다. 하지만, $\sum_{k= -\infty}^{\infty} a_k e^{j k \omega_0 t}$을 가지고 만들 수 없는 것들은 set에 들어가지 않기 때문에, 그런 것들은 표현이 불가능하게 됩니다.   
### 1대1 대응
\begin{aligned}    
x_{T_0}(t) \Leftrightarrow a_k
\end{aligned}    

상기는 $x_{T_0}(t)$를 표현하기 위해서는 단 하나의 $a_k$만 존재하냐는 1대1 대응인지를 나타냅니다. 하지만 상기의 수식에서는 1대1 대응이라고 할 수 가 없습니다. 왜냐하면 상기의 수식에 더해 특정 조건이 주어지지 않았을 때는 1대1 대응이라고 얘기할 수 가 없습니다. 상기의 수식이 왜 1대1 대응이 아닌지 확인해보겠습니다.   
만약 $a_1=1$이고 나머지는 다 0이라고 한다면, $e^{j \omega_0 t}$를 나타냅니다. 근데 여기서 만약 $f_0$가 바뀐다면 $x_{T_0}(t)$는 다른 주기함수가 됩니다. 즉, $f_0$의 값에 따라 $1Hz$의 sinusoid를 표현한 것일수도 있고, $2Hz$짜리 sinusoid를 표현한 것일 수도 있어 1대1 대응이 되지 못합니다.    
하지만, 여기서 **$T_0$가 주어진다면 1대1 대응**이 맞게 됩니다.    
정리 해보겠습니다.    
만약 <span style='color:red'>**푸리에 급수에서 $T_0$가 주어진다면 1대1 대응이 되지만, 주어지지 않는다면 푸리에 급수는 1대1 대응이 아닙니다.**</span>  

## Contributor $a_k$
이전까지 푸리에 급수에 대해 알아보았습니다. 푸리에 급수는 하기와 같이 정의됩니다.   
\begin{aligned}    
x_{T_0} (t) = \sum_{k= -\infty}^{\infty} a_k e^{j k \omega_0 t}
\end{aligned}   

그럼 여기서 주기함수 $x_{T_0}(t)$를 잘 표현하려면 Contributor인 $a_k$가 얼마나 잘 쓰여야 잘 표현할 수 있는지를 알아야합니다. 그럼 이번에는 $a_k$를 어떻게 구하는지에 대해 알아보겠습니다.   

<br>
푸리에 급수 수식 양번에 $e^{jr \omega_0 t}$를 inner product의 내적을 해보겠습니다.   
\begin{aligned}    
<a(t), b(t)> =& \int_{T_0} a(t)b^{\*}(t) dt \newline   
\int_{T_0} x_{T_0}(t) e^{-jr \omega_0 t} dt =& \int_{T_0} \sum_{k = - \infty}^{\infty} a_k e^{j(k - r) \omega_0 t} dt \newline   
\end{aligned}   

이전에 $e^{jk \omega_0 t}$들은 서로 Orthogonal하다고 했던 것을 기억하실 겁니다. 그럼 $\sum_{k = - \infty}^{\infty} a_k e^{j(k - r) \omega_0 t}$에서 $k \neq r$인 경우에는 서로 적분한 값이 모두 0으로 변할테고, $k = r$인 경우에만 0이 아닌 값이 나타날 것 입니다. 하지만 $k=r$인 경우에는 $e^{j(k - r) \omega_0 t} = 1$이 도고 결국에 우변의 수식은 $\int_{T_0} a_r dt = a_r T_0$만 남게될 것 입니다.그럼 $a_r$을 구하기 위해 좌변으로 $T_0$을 넘겨주면 됩니다. 하기에 수식을 전개해보겠습니다.   
\begin{aligned}    
\int_{T_0} x_{T_0}(t) e^{-jr \omega_0 t} dt =& \int_{T_0} \sum_{k = - \infty}^{\infty} a_k e^{j(k - r) \omega_0 t} dt \newline   
=& \int_{T_0} a_r dt \newline  
=& a_r T_0 \newline   
a_r =& \frac{1}{t_0} \int_{T_0} x_{T_0}(t) e^{-jr \omega_0 t} dt
\end{aligned}   

최종적으로 Contributor를 구하는 식은 $a_r = \frac{1}{t_0} \int_{T_0} x_{T_0}(t) e^{-jr \omega_0 t} dt$이 됩니다. 그럼 $a_r$ 식을 inner prodcut로 표현해보겠습니다.   
\begin{aligned}    
a_r =& \frac{1}{t_0} \int_{T_0} x_{T_0}(t) e^{-jr \omega_0 t} dt \newline   
a_r =& \frac{1}{T_0} <x_{T_0}(t), e^{jr \omega_0 t}> \newline  
\end{aligned}   

한번 inner product의 내적으로 생각을 해보겠습니다. 그러면 $x_{T_0}(t)$의 vector를 $e^{jr \omega_0 t}$의 vector로 projection을 시켜주는 것 입니다. 즉, <span style='color:blue'>**$x_{T_0}(t)$의 vector가 $e^{jr \omega_0 t}$의 vector의 방향 성분과 얼마나 닮았는가**</span>를 나타내게 됩니다. 그럼 $e^{jk \omega_0 t}$와 inner product를 한다고 한다면, $x_{T_0}(t)$가 $e^{jk \omega_0 t}$와 얼마나 닮았는지를 나타낸다는 것 입니다. 근데 여기서 $e^{jr \omega_0 t} \bot e^{jk \omega_0 t}$이며 basis이기 때문에 크기는 둘다 1입니다. 즉, <span style='color:red'>**$a_k$나 $a_r$은 $x_{T_0}(t)$와 각각의 basis인 $e^{jk \omega_0 t}$와 $e^{jr \omega_0 t}$와 얼마나 닮았는지를 나타내는 값**</span>이 됩니다. 


## 의문2
이제 마지막으로 **왜 푸리에 급수는 Complex Sinusoids로 나타냈는가**에 대해 알아보겠습니다. 이전에 이에 대한 답변으로 Complex 주기 신호까지 표현하고 싶기 때문이라고 말했습니다. 하지만, 추가적으로 사용하는 이유는 $e^{j \omega_0 t}$는 <span style='color:red'>**Eigen Function**</span>이기 때문입니다.   

한번 예시를 들어 설명해보겠습니다. $e^{j \omega_0 t }$가 입력이라고 가정을 하고 입력을 Impulse Response인 $h(t)$에 넣어보았다고 가정해보겠습니다. 그럼 Convolution 연산이 일어나 $e^{j \omega_0 t} \* h(t) = \int h(\tau) e^{j \omega_0 (t - \tau)} d\tau$과 같이 나타납니다. 하기에 수식으로 정리해보겠습니다.   
\begin{aligned}    
e^{j \omega_0 t} \* h(t) =& \int h(\tau) e^{j \omega_0 (t - \tau)} d\tau \newline   
=& e^{j \omega_0 t} \int h(\tau) e^{-j \omega_0 \tau} d\tau \newline   
\end{aligned}   

여기서 $e^{j \omega_0 t}$를 $x(t)$라고 했을 때, 입력을 넣어서 입력이 그대로 나오는 현상을 상기의 수식에서 확인할 수 있습니다. 그럼 뒤에 있는 $\int h(\tau) e^{-j \omega_0 \tau} d\tau$부분은 자연스럽게 $t$에 대한 함수가 아니게되니 상수로 취급이 되며 Eigen Function 꼴이 나타나게됩니다. 즉, eigen vector는 $e^{j \omega_0 t}$가 되고 eigen value는 $\int h(\tau) e^{-j \omega_0 \tau} d\tau$가 됩니다.  

그럼 이번에는 입력을 $\sum_{k = - \infty}^{\infty} a_k e^{j k \omega_0 t}$로 바꿔 Impulse Response에 넣어보겠습니다.   
\begin{aligned}    
\sum_{k = - \infty}^{\infty} a_k e^{j k \omega_0 t} \* h(t) =& \sum_{k = -\infty}^{\infty} \int h(\tau) e^{- j \omega_0 \tau} d\tau e^{j k \omega_0 t} \newline   
=& \sum_{k= - \infty}^{\infty} a_k H(k \omega_0) e^{jk \omega_0 t} \quad (\int h(\tau) e^{- j \omega_0 \tau} d\tau = H(k \omega_0)) \newline      
\end{aligned}    

이번에는 입력에서 contributor가 $a_k$인 것을 집어 넣었더니 contributor가 $a_k H(k \omega_0)$로 바뀌어 나타났습니다. 즉, $H(k \omega_0)$를 잘 조절해서 내가 원하는 주기함수로 나타낼 수 있다는 의미가 됩니다. 

### 예시
방금 설명한 이 $H(k \omega_0)$가 어떻게 실생활에서 이용되고 있는지 통신 예시를 들어 설명해보겠습니다. 저희는 현재 TV, 라디오, 핸드폰등을 동시에 사용할 수 있습니다. 하지만, 이런 제품들을 통신이 필수로 필요한데 이 통신에는 주파수가 들어와야합니다. 그럼 이 3개의 제품을 동시에 사용할려면 각각의 제품들 간의 주파수에서 간섭이 일어나지 않아야합니다. 이렇게 간섭이 일어나지 않으면서 동시에 쭈욱 이용할 수 있는 방법이 각 제품들이 서로 다른 주파수 대역을 사용하는 방법입니다. 이 주파수 대역 낮으며 사람이 들을 수 있는 대역이라면 우리는 항상 무슨 소리를 들으면서 살아야할 것 입니다. 이런경우는 매우 불편하기 때문에 사람이 들을 수 없으면 불가시할 수 있는 주파수 대역으로 이런 통신이 현재 지금도 이루어지고 있습니다. 그럼 여기서 의문을 가질 수 있을 겁니다. 각 제품들읠 주파수가 다 흐르고 있는데 어떻게  TV는 TV에 대한 하나의 주파수만 받을 수 있을까라는 생각을 하실 겁니다. 그 이유가 바로 $H(k \omega_0)$에 있습니다. $H(k \omega_0)$를 통해 저희가 TV 리모컨을 통해 TV를 켰을 때, TV에 대한 주파수대역만 제외하고 나머지는 모두 0으로 필터링을 하는 $H_{TV}(k \omega_0)$가 있기 때문이라고 설명할 수 있습니다. 즉, 이퀄라이저를 통해 불필요한 주파수는 죽이고 필요한 주파수만 이용을 하는 방법을 사용한다고 생각하시면 됩니다.