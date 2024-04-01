---
title:  "DTFS(Discrete Time Fourier Series)와 DTFT(iscrete Time Fourier Transform)"
categories: SystemsAndSignals
tag: [theory, Systems, Signals]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: 이산 시간 푸리에 급수와 변환
comments: true
date: 2024-03-31
toc_sticky: true
---

## CTFS와 DTFS
이전 포스팅까지 배웠던 푸리에들은 CTF(Continous Time Foureir)로 연속 시간에 대한 푸리에 대한 것 이었습니다. 기억을 되짚어볼겸 연속시간 푸리에 급수의 수식을 하기에 전개하겠습니다. 
\begin{aligned}    
x_{T_0}(t) =& \ldots + a_{-2}e^{j(-2)\omega_0 t} + a_{-1}e^{j(-1)\omega_0 t} + a_0 + a_{1}e^{j\omega_0 t} + a_{2}e^{j2\omega_0 t} + \ldots \newline   
=& \sum_{k= -\infty}^{\infty} a_ke^{jk\omega_0 t}
\end{aligned}    

그럼 이번에는 이산 시간 푸리에 급수(CTFS)의 수식을 전개해보겠습니다.   
\begin{aligned}    
x_N[n] = \sum_{k = \<N \>} a_k e^{jk \frac{2 \pi}{N} n}
\end{aligned}    

연속 시간에서 $\omega_0 = \frac{2 \pi}{T}$였습니다. 그럼 이산 시간으로 변경을 하면 $\omega_0 = \frac{2 \pi}{N}$가 됩니다. 또한, $\sum$의 범위가 $\< N \>$입니다.($\<N \>$ : N개를 고른다.) 그 이유는 만약 $k=1$과 $k=N+1$일때의 예를 들어 설명하겠습니다. $k=1$이라면, $e^{j \frac{2 \pi}{N} n}$이 됩니다. $k=N+1$이라면, $e^{j 2\pi n} e^{j \frac{2 \pi}{N} n}$이 됩니다. 근데 $e^{j 2\pi n} = 1$이 때문에 결국 $k=1=N+1$이 성립합니다. 즉, N을 주기로 똑같은 basis가 반복이 됩니다.($k : 0 ~ N-1$ 또는 $k : 1 ~ N$ 또는 $k : 2 ~ N+1$ 등)
<br>
<br>
<span style='color:blue'>**$a_k$ 비교**</span>   
연속 시간 푸리에 급수에서 $a_k$를 구하는 식은 하기와 같습니다.   
\begin{aligned}    
a_r =& \frac{1}{t_0} \int_{T_0} x_{T_0}(t) e^{-jr \omega_0 t} dt \newline   
a_r =& \frac{1}{T_0} <x_{T_0}(t), e^{jr \omega_0 t}> \newline  
\end{aligned}   

연속 시간 푸리에 급수를 구할 때와 동일하게, N개들 중에서 k번째와 r번째 간의 Inner Product를 통해 Contributor를 구합니다.    
\begin{aligned}    
x_N[n] =& \sum_{k = \<N \>} a_k e^{jk \frac{2 \pi}{N} n} \newline
\sum_{n = \<N \>} x_N[n] e^{- jr \frac{2 \pi}{N} n} =& \sum_{n = \<N \>} \sum_{k = \<N \>} a_k e^{jk \frac{2 \pi}{N} n}  e^{- jr \frac{2 \pi}{N} n} \newline   
=& \sum_{n = \<N \>} a_r (k = r) = N a_r \newline   
a_r =& \frac{1}{N} \sum_{n = \<N \>} x_N[n] e^{- jr \frac{2 \pi}{N} n}
\end{aligned}    

$k\neq r$이라면 $e^{j(k - r) \frac{2 \pi}{N} n} = 0$이 됩니다. 따라서 $k=r$일 때만 값이 살아남게 되며 $e^{j(k - r) \frac{2 \pi}{N} n} = 1$이 됩니다. 그럼 최종적으로 $a_k = \frac{1}{N} \sum_{n = \<N \>} x_N[n] e^{- jk \frac{2 \pi}{N} n}$을 얻게 됩니다. 이전에 basis가 반복되어 동일하다고 했었는데, 이에 대해서도 한번 확인해보겠습니다. $a_k$와 $a_{N+k}$가 동일하지 확인해보겠습니다.   
\begin{aligned}    
a_k =& \frac{1}{N} \sum_{n = \<N \>} x_N[n] e^{- jk \frac{2 \pi}{N} n} \newline   
a_{N+k} =& \frac{1}{N} \sum_{n = \<N \>} x_N[n] e^{- j(k+N) \frac{2 \pi}{N} n}  
=& \frac{1}{N} \sum_{n = \<N \>} x_N[n] e^{- jk \frac{2 \pi}{N} n} e^{- j 2 \pi n} (e^{- j 2 \pi n} = 1)
\end{aligned}    

상기의 수식으로 통해 $a_k$ = a_{N+k}$인 것을 확인할 수 있습니다. 

## CTFT와 CTIFT와 DTFT와 DTIFT
이전 포스팅까지 배웠던 푸리에들은 CTF(Continous Time Foureir)로 연속 시간에 대한 푸리에 대한 것 이었습니다. 연속시간 푸리에 변환은 비주기 함수에 대해서도 푸리에 급수를 나타낸 것입니다. 기억을 되짚어볼겸 연속시간 푸리에 변환과 역변환의 수식을 하기에 전개하겠습니다. 
\begin{aligned}    
FT : X(\omega) =& \int_{- \infty}^{\infty} x(t) e^{- j \omega t} dt \newline   
IFT : x(t) =& \frac{1}{2 \pi} \int_{- \infty}^{\infty} X(\omega) e^{j \omega t} d \omega \newline   
x(t) \Leftrightarrow& X(\omega)
\end{aligned}  

이산 시간 푸리에 변환도 동일하게 비주기 함수에 대해 푸리에 급수를 나타내보자 하는 것이 주목적입니다. 그럼 이산 시간 푸리에 변환을 유도해보겠습니다.   
### 이산 시간 푸리에 변환(DTFT)
연속 시간 푸리에 변환의 유도와 비슷하게 $N a_k$에 대해 전개해보겠습니다.   
\begin{aligned}    
N a_k =& \sum_{n = \<N \>} x_N[n] e^{- jk\frac{2 \pi}{N} n} \newline   
lim_{N \rightarrow \infty} \; N a_k =& lim_{N \rightarrow \infty} \sum_{n = \<N \>} x_N[n] e^{- jk\frac{2 \pi}{N} n}
\end{aligned}   

여기서 $N \rightarrow \infty$ 갈 때의 각각의 값들이 어떻게 되는지 살펴보고 간단하게 명시하기 위해 치환을 해보겠습니다.   
\begin{aligned}    
x_n[N] \rightarrow& x[n] \newline   
a_k N \rightarrow& X(k \Omega_0) \newline   
k \frac{2 \pi}{N} \rightarrow& k \Omega_0 \newline   

\end{aligned}   

여기서 $\Omega_0 = \frac{2 \pi}{N}$이며, $k : 0 ~ N-1$의 범위를 가집니다. 그럼 $N \rightarrow \infty$라면 $x_n[N]$은 더 이상 주기함수가 아니게 되어 $x[n]$으로 치환 됩니다. $k \frac{2 \pi}{N}$의 경우에는 $0 ~ 2 \pi$범위의 값을 가지는 실수가 됩니다. 그럼 $N \rightarrow \infty$에 따른 $N a_k$의 식이 어떻게 변하는지 하기에 전개해보겠습니다.   
\begin{aligned}    
lim_{N \rightarrow \infty} \; N a_k =& lim_{N \rightarrow \infty} \sum_{n = \<N \>} x_N[n] e^{- jk\frac{2 \pi}{N} n} \newline   
X(\Omega) =& \sum_{- \infty}^{\infty} x[n] e^{-j \Omega n}
\end{aligned}   

이전 연속 시간 푸리에 변환에서는 $lim$와 $\sum$이 만나 $\int$로 변환됬지만, 이는 구분부적분이 가능할 경우에만 변환이 됩니다. 하지만 이산 시간에서는 $lim_{N \rightarrow \infty} \sum_{n = \<N \>} = \sum_{- \infty}^{\infty}$로 변환되는데, $N$이 무한대로 간다고 구분부적분이 되는 것이 아닌, 기존에 N개를 선택하는 개수가 무한개의 선택으로 변하게되니 범위가 바뀌게 됩니다.    

### 이산 시간 푸리에 역변환(IDTFT)
이전에 구했던 이산 시간 푸리에 급수 수식을 하기에 전개하겠습니다.   
\begin{aligned}    
x_N[n] = \sum_{k = \<N \>} a_k e^{jk \frac{2 \pi}{N} n}
\end{aligned}   

그럼 좌측 변에 $\frac{2 \pi}{N}$ 곱해주고 나눠주는 트릭을 해보겠습니다.   
\begin{aligned}    
x_N[n] =& \frac{1}{2 \pi} \sum_{k = \<N \>} N a_k e^{jk \frac{2 \pi}{N} n} \frac{2 \pi}{N} \newline   
lim_{N \rightarrow \infty} x_N[n] =& lim_{N \rightarrow \infty} \frac{1}{2 \pi} \sum_{k = \<N \>} N a_k e^{jk \frac{2 \pi}{N} n} \frac{2 \pi}{N} \newline   
=& lim_{N \rightarrow \infty} \frac{1}{2 \pi} \sum_{k = \<N \>} X(k \Omega_0) e^{jk \Omega_0 n} \frac{2 \pi}{N}
\end{aligned}   

상기의 수식을 확인해보면, 밑변의 길이가 $k\Omega_0$이며 높이는 $X(k \Omega_0)$을 곱하는 사각형의 넓이가 됩니다. 이 넓이가 무한대로 가니 점점 얇아지며, 구분구적분이 가능해집니다. 즉, $lim$와 $\sum$을 합쳐 $\int$를 만들 수 있다는 의미가 됩니다. 근데, 여기서 $k$의 범위가 $0 ~ N-1$까지이기 때문에 범위는 $0 ~ 2\pi$가 됩니다.    
\begin{aligned}    
lim_{N \rightarrow \infty} x_N[n] =& lim_{N \rightarrow \infty} \frac{1}{2 \pi} \sum_{k = \<N \>} X(k \Omega_0) e^{jk \Omega_0 n} \frac{2 \pi}{N} \newline   
x[n] =& \frac{1}{2 \pi} \int_{2\pi} X(\Omega) e^{j \Omega n} d\Omega
\end{aligned}  

이제 이산 시간 푸리에 역변환과 변환을 정리하겠습니다.   
\begin{aligned}    
IDTFT \; : \; x[n] =& \frac{1}{2 \pi} \int_{2\pi} X(\Omega) e^{j \Omega n} d\Omega \newline   
DTFT \; : \; X(\Omega) =& \sum_{- \infty}^{\infty} x[n] e^{-j \Omega n}
\end{aligned}  