---
title:  "푸리에 변환(Fourier Transform)1"
categories: SystemsAndSignals
tag: [theory, Systems, Signals]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: Fourier Transform
comments: true
date: 2024-02-16
toc_sticky: true
---

## 푸리에 급수
푸리에 변환을 다루기 전에 이전 포스팅에서 다뤘던 푸리에 급수에 대해 간략하게 다뤄보겠습니다.   
푸리에 급수는 주기함수를 sinusoid들의 합으로 나타낸 것 입니다. 이 sinusoid들을 contributor를 통해 잘 조절해서 우리가 나타내고 싶은 주기함수 꼴로 나타내었습니다. 수식은 하기와 같이 됩니다.   
\begin{aligned}    
\omega_0 =& 2 \pi f_0 \quad(f_0 : Frequency) \newline   
x_{T_0}(t) =& \sum_{k= -\infty}^{\infty} a_ke^{jk\omega_0 t} \quad (Fourier \; Series) \newline   
a_k =& \frac{1}{T_0} \int_{-\frac{T_0}{2}}^{\frac{T_0}{2}} x_{T_0}(t) e^{-jk \omega_0 t} dt \quad (Contributor)
\end{aligned}   

## 푸리에 변환(Fourier Tranform)
푸리에 변환은 간단하게 말하면 푸리에 급수의 확장 버전이라고 생각하면 됩니다. 푸리에 급수는 **Periodic Signal을 Periodic Signal의 합**으로 나타냈습니다. 그럼 비주기 신호(Aperiodic Signal)은 어떻게 나타내면 될까요? 이 의문을 해결하기 위해 푸리에 변환이 나타났습니다. 근데 여기서 Aperiodic Signal을 나타내기 위해서는 **1대1 대응**이 필요합니다.   

그럼 우선 비주기 신호(Aperiodic Signal)이 무엇인지 알아보겠습니다.   
<img src="../../../assets/images/Signals&Systems/2024-02-16-Fourier Transform 1/aperiodic signal 1.jpg" alt="aperiodic signal 1" style="zoom:80%;" />    
상기의 이미지는 현재 주기가 4인 주기함수입니다. 이 주기함수를 일반화를 해보겠습니다.   
<img src="../../../assets/images/Signals&Systems/2024-02-16-Fourier Transform 1/aperiodic signal 2.jpg" alt="aperiodic signal 2" style="zoom:80%;" />    
상기의 이미지는 주기가 $T_0$인 주기함수가 됩니다. 그럼 만약 이 $T_0 \rightarrow \infty$인 주기가 무한대인 함수로 나타내면 어떻게 될까요? 이 함수가 바로 **비주기 함수**가 됩니다. 즉, **비주기 함수는 주기가 무한대인 함수**를 의미합니다.   

그럼 한번 푸리에 급수에서 주기인 $T_0$가 계속 커질수록 푸리에 계수인 $a_k$가 어떻게 변화하는지 식으로 알아보겠습니다. 푸리에 계수는 $a_k = \int_{-\frac{T_0}{2}}^{\frac{T_0}{2}} x_{T_0}(t) e^{-jk \omega_0 t} dt$가 되는데 여기서 주기인 $T_0$가 무한대로 커진다면 $a_k$값은 0으로 수렴하게 될 것 입니다. 하지만, 여기서 $T_0 a_k$의 값으로 $T_0 \rightarrow \infty$갈 때 주파수인 $f_0$축에서 확인을 해보면 해당값이 $f_0$는 $T_0$의 역수여서 $f_0 \rightarrow 0$으로 가기 때문에 어떤 특정한 값으로 수렴을 하게 됩니다. 여기서 **푸리에 변환은 $T_0 a_k$의 값을 관찰**하는 것이됩니다.   

### Fourier Tranform Derivate(유도)
\begin{aligned}    
a_k T_0 =& \int_{-\frac{T_0}{2}}^{\frac{T_0}{2}} x_{T_0}(t) e^{-jk \omega_0 t} dt \newline   
lim_{T_0 \rightarrow \infty} \; a_k T_0 =& lim_{T_0 \rightarrow \infty} \int_{-\frac{T_0}{2}}^{\frac{T_0}{2}} x_{T_0}(t) e^{-jk \omega_0 t} dt
\end{aligned}   

여기서 $T_0 \rightarrow \infty$ 갈 때의 각각의 값들이 어떻게 되는지 살펴보고 간단하게 명시하기 위해 치환을 해보겠습니다.   
\begin{aligned}    
lim_{T_0 \rightarrow \infty} x_{T_0}(t) \rightarrow& x(t) \newline   
lim_{T_0 \rightarrow \infty} a_k T_0 \triangleq& X(k \omega_0) \newline   
lim_{T_0 \rightarrow \infty} K \omega_0 \triangleq& \omega (K: -\infty ~ \infty \; integer, \; \omega_0 : 0 \rightarrow K \omega_0 = Real Number)
\end{aligned}   

치환을 적용하여 다시 $a_k T_0$를 표현해보겠습니다.   
\begin{aligned}    
a_k T_0 =& \int_{-\frac{T_0}{2}}^{\frac{T_0}{2}} x_{T_0}(t) e^{-jk \omega_0 t} dt \newline   
X(\omega) =& \int_{- \infty}^{\infty} x(t) e^{- j \omega t} dt 
\end{aligned}   

상기의 수식이 곧 푸리에 변환입니다.   

### Inverse Fourier Transform
이번에는 Inverse Fourier Transform을 유도해보겠습니다.   
이번에는 푸리에 급수의 수식을 정의해보겠습니다.   
\begin{aligned}    
x_{T_0}(t) =& \sum_{k= -\infty}^{\infty} a_ke^{jk\omega_0 t} \newline   
x_{T_0}(t) =& \frac{1}{2 \pi} \sum_{k= -\infty}^{\infty} T_0 a_ke^{jk\omega_0 t} \frac{2 \pi}{T_0} \newline   
lim_{T_0 \rightarrow \infty} x_{T_0}(t) =& lim_{T_0 \rightarrow \infty} \frac{1}{2 \pi} \sum_{k= -\infty}^{\infty} T_0 a_ke^{jk\omega_0 t} \frac{2 \pi}{T_0}
\end{aligned}   

상기의 수식에서 $lim$와 $\sum$이 같이 있으니 리만합으로 합쳐보겠습니다.     
\begin{aligned}    
lim_{T_0 \rightarrow \infty} x_{T_0}(t) =& \frac{1}{2 \pi} lim_{T_0 \rightarrow \infty} \sum_{k= -\infty}^{\infty} T_0 a_ke^{jk\omega_0 t} \frac{2 \pi}{T_0} \newline   
\end{aligned}   

그럼 여기서 축의 도메인은 $k \omega_0$가 되며 $\omega_0$는 $\frac{2 \pi}{T_0}$이니 $T_0 \rightarrow \infty$일 때 $\omega_0 \rightarrow 0$으로 가며 $k \omega_0$가 바뀌게 됩니다. 즉, $d \omega$가 된다는 의미가 됩니다. 그럼 여기서 $k$는 무한대의 정수 값을 가지니 $k \omega_0$는 모든 실수를 표현할 수 있게 됩니다.   
<img src="../../../assets/images/Signals&Systems/2024-02-16-Fourier Transform 1/inverse fourier transform 1.jpg" alt="inverse fourier transform 1" style="zoom:80%;" />    
그럼 리만합 적분으로 나타내면 하기와 같이 나타내집니다.   
\begin{aligned}    
lim_{T_0 \rightarrow \infty} x_{T_0}(t) =& \frac{1}{2 \pi} \int_{- \infty}^{\infty} a_k T_0 e^{jk \omega_0 t} dk \omega_0 \newline   
=& \frac{1}{2 \pi} \int_{- \infty}^{\infty} X(k \omega_0) e^{jk \omega_0 t} dk \omega_0 \newline   
=& \frac{1}{2 \pi} \int_{- \infty}^{\infty} X(\omega) e^{j \omega t} d \omega
\end{aligned}  


