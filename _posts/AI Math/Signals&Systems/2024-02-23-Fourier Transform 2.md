---
title:  "푸리에 변환(Fourier Transform)2"
categories: SystemsAndSignals
tag: [theory, Systems, Signals]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: FT와 IFT를 내적으로 유도 및 FT 예시
comments: true
date: 2024-02-24
toc_sticky: true
---

## 푸리에 변환(Fourier Transform)과 푸리에 역변환(Inverse Fourier Transform)
이전 포스팅에서 푸리에 변환과 푸리에 역변환의 유도를 해보았습니다. 푸리에 변환을 간단하게 말하면 **푸리에 급수의 한계인 Periodic Signal에 대한 한정적인 부분을 APeriodic Signal까지로 확장**한 것 입니다. 또한, 푸리에 계수인 $a_k$와 주기인 $T_0$의 곱인 $T_0 a_k$값을 관찰한 것 입니다. 여기서 $x(t) \Leftrightarrow X(\omega)$와 같이 1대1 대응이 되기 때문에 반대로 역변환도 구할 수 있게됩니다. 그럼 FT(Fourier Transform)와 IFT(Inverse Fourier Transform)의 수식을 하기에 정의하겠습니다.  
\begin{aligned}    
FT : X(\omega) =& \int_{- \infty}^{\infty} x(t) e^{- j \omega t} dt \newline   
IFT : x(t) =& \frac{1}{2 \pi} \int_{- \infty}^{\infty} X(\omega) e^{j \omega t} d \omega \newline   
x(t) \Leftrightarrow& X(\omega)
\end{aligned}  

### 푸리에 변환 내적 해석
예전 푸리에 급수 포스팅에서 $e^{j \omega t}$는 vector라고 할 수 있다고 한 것을 기억하실겁니다. 또한, inner product(내적)의 정의도 기억하실 겁니다. 그럼 푸리에 변환의 식을 하기와 같이 내적으로 표현할 수 있을 것 입니다.    
\begin{aligned}    
<x(t), e^{j \omega t}> =& \int_{- \infty}^{\infty} x(t) e^{- j \omega t} dt \newline   
\end{aligned}   

상기와 같이 내적으로 표현하면 $x(t)$가 모든 실수에 대한 $e^{j \omega t}$와 내적한 값을 다 더한 결과가 됩니다. 즉 하기의 그림을 보겠습니다.   
<img src="../../../assets/images/Signals&Systems/2024-02-23-Fourier Transform 2/FT 1.jpg" alt="FT 1" style="zoom:80%;" />    
$\omega = 2 \pi f$가 되니 여기서 $f$는 주파수를 의미하면 1초동안 얼마나 많이 돌았는가를 의미하게됩니다. 즉, $f$값이 크면 빠르게 돌게 되고, 작으면 느리게 돈다고 생각하시면 됩니다. 이어서 상기의 그림에서 $e^{j 2\pi (3)t}$는 느리게 도는 애, $e^{j 2\pi (100)t}$는 빠르게 도는 애가 됩니다. 이런 주파수를 가진 애들과 내적을 수행합니다. 여기서 $e^{j \omega t}$의 크기는 항상 1입니다. 또한 $x(t)$의 크기도 항상 고정되어 있습니다. 그럼 이 둘의 내적은 곧 **이 두 vector가 얼마나 닮았는가($cos \theta$)를 나타**내게 됩니다. 그런 닮은 정도의 값을 구한 후 다 더해준 결과값이 곧 $X(\omega)$가 됩니다. 그럼 FT(Fourier Transform)을 구한 하기의 그래프를 분석해보겠습니다.   
<img src="../../../assets/images/Signals&Systems/2024-02-23-Fourier Transform 2/FT 2.jpg" alt="FT 2" style="zoom:80%;" />    
$\omega$의 값이 작으면 주파수가 작다는 뜻이니 즉, 느리게 도는 애라고 생각하시면 됩니다. 상기의 그래프는 $\omega$가 작은 부위에서 $X(\omega)$가 높은 값을 나타냅니다. 즉, 주파수가 작은(느린 애) 저주파 성분이 많이 들어갔다고 할 수 있습니다. 이렇게 신호를 주파수 측면에서 해석해볼 수 있습니다. 

### 푸리에 계수
푸리에 급수는 $x_{T_0}(t) = \sum_{k= -\infty}^{\infty} a_ke^{j \omega t}$이며, $e^{j \omega t}$가 $a_k$를 통해 얼마나 사용되는지를 알 수 있었습니다. 하지만 FT의 수식에는 이 $a_k$가 존재하지 않아 얼마나 기여를 하는지를 수식을 통해 확인할 수 없습니다. 하지만 알 수 있는 방법이 있습니다. 이번에는 $\omega$축이 아닌 $\omega = 2 \pi f$이니 주파수($f$)축으로 나타내보겠습니다.   
<img src="../../../assets/images/Signals&Systems/2024-02-23-Fourier Transform 2/FT 3.jpg" alt="FT 3" style="zoom:80%;" />    
상기의 그림은 주파수축에서 나타내었습니다. $X(\omega) = lim_{T_0 \rightarrow \infty} a_k T_0$이었습니다. 그럼 여기서 푸리에 계수인 $a_k$를 구하려면 $a_k = X(\omega) \times f_0$를 통해 구할 수 있을 것 입니다. 그럼 $f_0$를 알아야 합니다. 그럼 상기의 이미지에서 $100Hz$를 k번째 $f_0$라고 해보겠습니다. 그럼 $a_k = X(100) \times f_0$가 됩니다. 근데 여기서 $T_0 \rightarrow \infty$가 되니 $f_0 \rightarrow 0$으로 가게됩니다. 즉, $a_k \rightarrow 0$으로 가게 될 것 입니다. 즉, 한 위치에서의 값으로 $a_k$를 표현할 수는 없다는 말이 됩니다. 비슷한 예시로 pdf를 생각해보시면 됩니다. pdf(확률 밀도 함수)의 경우 만약 특정 x축에서의 확률을 구하라고 하면 그 확률은 항상 0이됩니다. 하지만 특정 x축의 범위가 주어지면 확률을 나타낼 수 있습니다. 이와 동일하게 $100Hz$에서의 기여도인 $a_k$를 구하면 0이됩니다. 하지만, $100Hz ~ 500 Hz$까지의 $a_k$를 구하라하면 표기할 수 있다는 의미가 됩니다. 즉, **기여 밀도 함수**라고 말하기도 합니다.    

### 예시
<img src="../../../assets/images/Signals&Systems/2024-02-23-Fourier Transform 2/FT 4.jpg" alt="FT 4" style="zoom:80%;" />    
상기의 함수를 푸리에 변환을 해보겠습니다.   
\begin{aligned}    
X(\omega) =& \int_{ - \frac{T}{2}}^{\frac{T}{2}} 1 \times e^{- j \omega t} dt \newline   
=& \frac{e^{- j \omega \frac{T}{2}} - e^{j \omega \frac{T}{2}}}{- j \omega} \newline   
=& \frac{- 2j sin(\omega \frac{T}{2})}{- j \omega} \newline   
=& \frac{T sin(\omega \frac{T}{2})}{ \frac{\omega}{2} T} \quad (sinc(x) = \frac{sin(x)}{x}) \newline   
=& T sin(\frac{\omega T}{2})
\end{aligned}   

상기의 결과와 같이 $sinc$함수가 됩니다. 

## 푸리에 변환 예시들
### $\delta (t)$
$\delta (t)$의 푸리에 변환을 해보겠습니다.   
\begin{aligned}    
X =& \int_{- \infty}^{\infty} \delta (t) e^{- j \omega t} dt \newline   
=& 1 \int_{- \infty}^{\infty} \delta (t) dt \newline   
=& 1
\end{aligned}  

### $x(t - t_0)$
Inverse Fourier Transform인 $x(t) = \frac{1}{2 \pi} \int_{- \infty}^{\infty} X(\omega) e^{j \omega t} d \omega$의 식을 이용하여 표현해보겠습니다.   
\begin{aligned}    
x(t) =& \frac{1}{2 \pi} \int_{- \infty}^{\infty} X(\omega) e^{j \omega t} d \omega \newline   
x(t - t_0) =& \frac{1}{2 \pi} \int_{- \infty}^{\infty} X(\omega) e^{- j \omega t_0} e^{j \omega t} d \omega \newline   
\end{aligned}  

IFT에서 $x(t)$의 FT는 $X(\omega)$입니다. 그럼 IFT로 표현한 $x(t - t_0)$의 FT는 $X(\omega) e^{- j \omega t_0}$이 된다는 것을 한번 생각해볼 수 있습니다. 즉, 기존의 IFT의 형상을 유지하면서 $X(\omega)$부분이 $X(\omega) e^{- j \omega t_0}$로 바뀌었다고 생각하면 됩니다. 이 이유는 **푸리에 변환은 1대1 대응**이기 때문에 가능한 것 입니다.   

### $x(t) \* h(t)$
이번에는 Convolution에 대한 푸리에 변환을 구해보겠습니다.   
우선 Convolution은 $x(t) \* h(t) = \int_{- \infty}^{\infty} x(\tau) h(t - \tau) d \tau$입니다. 이 Convolution을 푸리에 변환식에 넣어주겠습니다.   
\begin{aligned}    
X =& \int_{- \infty}^{\infty} \int_{- \infty}^{\infty} x(\tau) h(t - \tau) d \tau \; e^{- j \omega t} dt \newline   
=& \int_{- \infty}^{\infty} x(\tau) \int_{- \infty}^{\infty} h(t - \tau) e^{- j \omega t} dt \; d\tau \newline  
=& \int_{- \infty}^{\infty} x(\tau) H(\omega) e^{-j \omega \tau} \; d\tau \newline  
=& H(\omega) \int_{- \infty}^{\infty} x(\tau) e^{-j \omega \tau} \; d\tau \newline   
=& H(\omega) X(\omega) \newline   
\end{aligned}  

상기의 전개식에 대해 살펴보겠습니다. 우선 푸리에 변환에 Convolution식을 풀어서 대입을 했습니다. 그런 후 **적분의 순서를 바꿔 $t$도메인들의 적분끼리 합치고 $\tau$ 도메인의 적분들끼리 합쳐**주었습니다. 그러면 자연스럽게 $\int_{- \infty}^{\infty} h(t - \tau) e^{- j \omega t} dt$이 나타나는데, 이 푸리에 변환식은 방금 위에서 구했던 $x(t - t_0)$와 같은 꼴이 됩니다. 따라서 $\int_{- \infty}^{\infty} h(t - \tau) e^{- j \omega t} dt = H(\omega) e^{-j \omega \tau}$인 것을 알 수 있습니다. 그런 후, $H(\omega)$는 적분 도메인과 맞지 않으니 밖으로 빼주니 가장 기본적인 푸리에 변환식이 나타납니다. 따라서 최종적으로 **$x(t) \* h(t)$의 푸리에 변환은 $H(\omega)X(\omega)$**인 것을 알 수 있습니다.   
즉, <span style='color:red'>**Convolution은 입력에 대해 필터링을 수행해주는데, 이를 푸리에 변환을 통해 주파수 도메인에서 확인해보니 Convolution은 그냥 곱셈이다라는 의미가 됩니다.**</span>

### $x(-t)$
이번에는 IFT를 이용해보겠습니다.   
\begin{aligned}    
x(t) =& \frac{1}{2 \pi} \int_{- \infty}^{\infty} X(\omega) e^{j \omega t} d \omega \newline   
x(-t) =& \frac{1}{2 \pi} \int_{- \infty}^{\infty} X(\omega) e^{- j \omega t} d \omega \newline   
x(-t) =& \frac{1}{2 \pi} \int_{- \infty}^{\infty} X(- \omega) e^{j \omega t} d \omega
\end{aligned}  

상기의 전개식을 살펴보겠습니다. $t \rightarrow -t$가 되니 그럼 $\omega \rightarrow - \omega$로 변경해주면 됩니다. 그럼 $x(-t) = \frac{1}{2 \pi} \int_{- \infty}^{\infty} X(- \omega) e^{j \omega t} d - \omega$가 됩니다. 근데 적분의 구간이 $\omega$축에서 $- \infty \~ \infty$까지이니 $\omega$나 $-\omega$나 적분 구간이 동일해집니다. 따라서 $x(-t)$의 푸리에 변환은 $X(- \omega)$가 됩니다.   

### $x^{\*}(t)$
이번에도 역시 IFT를 이용해보겠습니다.   
\begin{aligned}    
x(t) =& \frac{1}{2 \pi} \int_{- \infty}^{\infty} X(\omega) e^{j \omega t} d \omega \newline   
x^{\*}(t) =& \frac{1}{2 \pi} (\int_{- \infty}^{\infty} X(\omega) e^{- j \omega t} d \omega)^{\*} \newline   
x^{\*}(t) =& \frac{1}{2 \pi} \int_{- \infty}^{\infty} X^{\*}(\omega) (e^{j \omega t})^{\*} d \omega \newline   
x^{\*}(t) =& \frac{1}{2 \pi} \int_{- \infty}^{\infty} X^{\*}(\omega) e^{- j \omega t} d \omega \newline   
x^{\*}(t) =& \frac{1}{2 \pi} \int_{- \infty}^{\infty} X^{\*}(- \omega) e^{j \omega t} d \omega 
\end{aligned}  

상기의 전개식을 살펴보겠습니다. 우선 $(e^{j \omega t})^{\*} = e^{-j \omega t}$가 된다는 것을 알 수 있습니다. 그 후, 이전에 구했던 $x(-t)$의 푸리에 변환과 동일하게 구해주면 최종적으로 $X^{\*}(- \omega)$를 구할 수 있습니다.   
즉, 하기와 같이 FT와 IFT를 간략하게 정리해볼 수 있습니다.   
\begin{aligned}    
x(t) \Leftrightarrow& X(\omega) \newline   
x(-t) \Leftrightarrow& X(-\omega) \newline   
x^{\*}(t) \Leftrightarrow& X^{\*}(- \omega) \newline   
x^{\*}(-t) \Leftrightarrow& X^{\*}(\omega) 
\end{aligned}  

### Duality
푸리에 변환에서 Duality는 이전에 $x(t)$를 푸리에 변환을 하면 $X(\omega)$가 된다고 했습니다. 그럼 이 $X(\omega)$로 나타난 식이 시간 도메인($t$)에 있다고 했을 때, 이를 다시 푸리에 변환하면 어떻게 되는지입니다. 즉, $x(t) \Leftrightarrow X(\omega)$일 때, $X(t) \Leftrightarrow \?$을 구한다고 보면 됩니다.   
우선 IFT를 전개해보겠습니다.    
\begin{aligned}    
x(t) =& \frac{1}{2 \pi} \int_{- \infty}^{\infty} X(\omega) e^{j \omega t} d \omega \newline   
x(\omega) =& \frac{1}{2 \pi} \int_{- \infty}^{\infty} X(t) e^{j \omega t} dt
\end{aligned}   

여기서 $t$를 $\omega$로 치환을 했습니다. 이제 $x(\omega)$식을 푸리에 변환식의 꼴로 만들어 보겠습니다. $2\pi$를 넘겨주겠습니다.   
\begin{aligned}    
x(\omega) =& \frac{1}{2 \pi} \int_{- \infty}^{\infty} X(t) e^{j \omega t} dt \newline   
2 \pi x(- \omega) =& \int_{- \infty}^{\infty} X(t) e^{- j \omega t} dt \newline   
\end{aligned}   

즉, 푸리에 변환식의 꼴로 나타났습니다. $X(t)$를 푸리에 변환을 하면 $2 \pi x(- \omega)$나온다는 의미가 됩니다. 즉, 이전에 수행했던 네모 그래프를 푸리에 변환하면 $sinc$가 나왔습니다. 그럼 이 $sinc$를 시간 도메인에 놓고 다시 푸리에 변환을 하면 축을 뒤집은 후 $2\pi$가 곱해진 네모 그래프가 나온다는 의미가 됩니다. 

### $x(t)h(t)$
이번에는 수식을 전개하지 않고 방금 배웠던 Duality를 이용하여 구해보겠습니다.   
$x(t) \* h(t) \Leftrightarrow X(\omega)H(\omega)$인 것을 알고 있으실 겁니다. 그럼 $X(t)H(t)$의 푸리에 변환은 $2\pi x(-\omega) \* h(- \omega)$가 될 것 입니다. 이것도 같은 결과지만 조금 다르게도 표현해보겠습니다. 통상 푸리에 변환표를 보시면 모두 $x(t)h(t)$로 표현일 겁니다.  
\begin{aligned}    
X(t)H(t) \Leftrightarrow& 2\pi x(-\omega) \* h(- \omega) \newline   
\Leftrightarrow& \frac{2\pi x(-\omega) \* 2 \pi h(- \omega)}{2 \pi} \newline   
x(t)h(t) \Leftrightarrow& \frac{X(\omega) \* H(\omega)}{2 \pi}
\end{aligned}   
