---
title:  "Rank와 Null Sapce"
categories: LinearAlgebra
tag: [theory, math]
tags: [Jekyll, MathJax]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: Rank(행렬의 계수)와 Null Sapce(영공간)이란
comments: true
date: 2023-12-01
toc_sticky: true
---

## Rank(계수)
우선 rank의 정의에 대해 알아보겠습니다.   
> 선형대수학에서, 선형 변환의 계수(階數, 영어: rank)는 선형 변환의 비(非) 퇴화 정도를 나타내는 기수입니다.   

즉, Rank란 **행렬이 가지는 Independent한 Column의 수(=Column space의 Dimension)**입니다. 이전에 span을 설명할 때, **Independent한 vector들의 수가 곧 Dimension**이라고 했었습니다. 그 이유가 Independent한 vector들을 span하면 그 개수만큼의 Dimension 전체를 표현할 수 있기 때문이었습니다. 따라서 Column space가 Column들이 span하는 space이니 곧 그 개수가 Dimension이 됩니다.   
이제 가장 중요한 Rank의 성질에 대해 알아보겠습니다.   
\- **Independent한 Column의 수 = Independent한 row의 수**, $rank(A) = rank(A^T)$   
&rarr; rank는 Independent한 column의 수인 동시에 Independent한 row의 수이기도 합니다. 따라서 row space의 Dimension도 rank라는 의미입니다.   

이제 예시를 통해 좀 더 알아보겠습니다.   
\begin{aligned}    
A=   
\begin{bmatrix} 
1 & 2 & 3  \newline   
0 & 0 & 0  \newline   
\end{bmatrix}, rank=1   
\quad    
B = 
\begin{bmatrix}    
1 & 0 & 1  \newline   
0 & 1 & 1  \newline  
\end{bmatrix}, rank=2   
\quad
\end{aligned}   

B를 확인해보겠습니다. B의 rank는 2입니다. 즉, Independent한 column vector가 2개 존재하며, 동시에 row vector 또한 2개가 존재한다는 뜻입니다. row space로 관점을 바꿔본다면, row vector는 3차원 공간을 표현합니다. 3차원 공간에서 2차원인 평면만을 span할 수 있다는 말이 됩니다. 또한, B의 크기가 2 x 3인 row의 수가 2이니 3차원을 표현할 수는 없습니다. B의 경우 2 x 3이며 rank는 2이니 row 개수만큼 rank가 꽉 차있다고 할 수 있습니다. 따라서 **full row rank**라고 부릅니다. A의 경우에는 2 x 3인데, rank는 1입니다. rank가 크기보다 부족하니 **rank-deficient**라고 부릅니다.   
그럼, 3 x 2에 rank가 2라면, **full column rank**라고 부릅니다. 또한, 3 x 3인 정방 행렬일 때, rank가 3이라면 **full rank**라고 부릅니다. 즉, full rank는 matrix가 가질 수 있는 최대 rank를 의미합니다.  

## Null Space
이전에 계속해서 Column space를 다뤘습니다. Null Space는 Column space와는 완전히 다른 sub space입니다. Null space의 정의는 하기와 같습니다.   
> 선형 방정식 $A\underline{x}=\underline{b}$에서 b가 zero vector(=0 vector, Null vector)일 때, 식을 만족시키는 모든 가능한 해 $\underline{x}$에 대한 집합입니다.   
즉, 선형 방정식 $Ax=0$들이 이루는 공간이 Null Space입니다.   

<img src="../../../assets/images/LinearAlgebra/2023-12-01-Rank and NullSpace/Null Space1.JPG" alt="Null Space1" style="zoom:80%;" />    

즉, Null Space란 **$A\underline{x} = \underline{0}$을 만족하는 $\underline{x}$의 집합**을 의미합니다. column들의 linear combination이 0이 되게 하는 그 계수 $\underline{x}$의 집합입니다.   
예시를 들어 설명해보겠습니다.   
\begin{aligned}    
A=   
\begin{bmatrix} 
1 & 0 & 1  \newline   
0 & 1 & 1  \newline   
\end{bmatrix}   
\quad    
A\underline{x} = 
x_1   
\begin{bmatrix}    
1 \newline   
0 \newline  
\end{bmatrix}   
+x_2   
\begin{bmatrix}    
0 \newline   
1 \newline  
\end{bmatrix}   
+x_3   
\begin{bmatrix}    
1 \newline   
1 \newline  
\end{bmatrix}   
=   
\begin{bmatrix}    
0 \newline   
0 \newline  
\end{bmatrix}   
\end{aligned}    

여기서 $\underline{x} = \begin{bmatrix} 0 \newline 0 \newline 0 \end{bmatrix}, \quad \begin{bmatrix} 1 \newline 1 \newline -1 \end{bmatrix}, \quad \begin{bmatrix} 2 \newline 2 \newline -2 \end{bmatrix}$들이 Null Space에 들어갑니다. 여기서 $\begin{bmatrix} 0 \newline 0 \newline 0 \end{bmatrix}$은 무조건 0 vector로 만들어줍니다. 따라서 trival solution이며 무조건 Null Space에 포함되는 vector입니다. 다음과 같이 Scalar배를 한 vector들이 모두 Null space로 들어갑니다. 따라서, Null Space에는 하기를 만족합니다.  
\begin{aligned}    
c \times A\underline{x} =& c \times \underline{0} \newline   
\underline{x_n} =& c \begin{bmatrix} 1 \newline 1 \newline -1 \end{bmatrix}
\end{aligned}    

위의 A Matrix에서의 column space는 2차원이 됩니다. 하지만 Null Space의 경우에는 3차원이 됩니다. 따라서, Null Space와 Column Space는 다른 sub space에 존재한다고 할 수 있습니다.   

이제 일반화를 해보겠습니다.   
<span style='color:red'>**Matrix A가 m x n일 때, $dim(N(A)) = n - r$**</span>이 됩니다. 여기서 $r$은 rank를 의미합니다. 한 번 예시를 통해 확인해보겠습니다.   
\begin{aligned}    
A=   
\begin{bmatrix} 
1 & 0   \newline   
0 & 1  \newline   
0 & 0  \newline   
\end{bmatrix}   
\quad rank = 2, \; n = 2   
\end{aligned}   

이 경우, Null Space는 $n - r = 2 - 2 = 0$인 0차원이 됩니다. Null Space를 만드는 $\begin{bmatrix} 0 \newline 0 \end{bmatrix}$인 점 한개가 존재한다는 뜻입니다. 

여기서 매우 중요한 개념에 대해 알려드리겠습니다.   
<span style='color:blue'>**Null space와 Row space는 서로 수직한 space**</span>입니다.    

\begin{aligned}    
A\underline{x} =& \underline{0} \newline   
A\underline{x} =& \begin{bmatrix} a_1 \newline \vdots \newline a_n \newline \end{bmatrix}   
\cdot   
\begin{bmatrix} x_1 \newline \vdots \newline x_k \newline \end{bmatrix}   
=   
\begin{bmatrix} 0 \newline \vdots \newline 0 \newline \end{bmatrix}   
\end{aligned}    

즉, $A\underline{x}$는 A의 row vector인 $a_n$와 x vector간의 내적한 값이라고 생각할 수 있습니다. 그 내적값이 모두 0이 된다는 뜻입니다. 여기서 $a_1$인 row vector와 $a_2$인 row vector와의 내적값이 모두 0이 되니 $\underline{a_1}x_1 + \underline{a_2}x_2 = \underline{0}$이 되는것을 알 수 있습니다. 그럼 row space 전체와 $\underline{x}$는 항상 수직하다고 할 수 있습니다.   
따라서, $dim(R(A))+ dim(N(A)) = n$을 나타낼 수 있습니다.   
### Left Null Space   
$A\underline{x}$를 column space로 생각했던 것처럼, $\underline{x}^T A = \underline{0}^T$를 row space로 바라본 것 입니다.    
여기서 A가 m x n 이라면, $\underline{x}$는 m 차원에 놓인 vector일 것입니다. 따라서, Null space의 dimension은 $dim(N_L(A)) = m - r$이 됩니다. 이전처럼 내적으로 생각해보면 이번에는 모든 column vector와 수직하게 됩니다. 따라서, $dim(C(A)) + dim(N_L(A)) = m$이 됩니다.   
<img src="../../../assets/images/LinearAlgebra/2023-12-01-Rank and NullSpace/Null Space2.JPG" alt="Null Space2" style="zoom:80%;" />    
