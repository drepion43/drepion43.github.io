---
title:  "행렬 종류"
categories: LinearAlgebra
tag: [theory, math]
tags: [Jekyll, MathJax]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: 여러가지의 행렬들에 대한 설명
comments: true
date: 2023-11-29
toc_sticky: true
---

## 항등 행렬(Identity Matrix)
항등 행렬을 알아보기 전에 **항등원**에 대해 먼저 알아보겠습니다. 항등원의 정의는 하기와 같습니다.   
> 항등원(identity element)이란 임의의 수 $a$에 대하여 어떤 수를 연산했을 때 처음의 수 $a$가 되도록 만들어 주는 수 입니다.

즉, 어떤 연산을 취해줬을 때, 그대로 자신이 튀어나오게 해주는 수입니다. 예를 들어, 곱셈의 항등원인 $1$이 됩니다. $3 \times x = 3$일 때, 항등원은 1이 됩니다.   
     
정방 행렬(Square Matrix)은 크기가 n x n 인 정사각형 모양을 띄는 행렬들을 일컫습니다. 여기서 대각 원소라는 말이 나오는데 이 말은 대각 성분에 존재하는 원소들을 말합니다. 하기의 행렬을 보고 추가적으로 설명하겠습니다.   
\begin{aligned} 
\begin{bmatrix} a_{11} & a_{12} & a_{13} & \ldots & a_{1j}  \newline  
a_{21} & a_{22} & a_{23} & \ldots & a_{2j}  \newline  
a_{31} & a_{32} & a_{33} & \ldots & a_{3j}  \newline  
\vdots & \vdots & \vdots & \vdots & \vdots  \newline  
a_{i1} & a_{i2} & a_{i3} & \ldots & a_{ij}  \newline  
\end{bmatrix} 
\end{aligned}  

여기서 $i=j$라면 정방 행렬이 됩니다. 또한, 각 원소들 $a$에서 $i=j$인 원소들이 곧 대각 원소가 됩니다.   

그럼 이제 항등 행렬에 대해 알아보겠습니다. 항등원의 개념을 행렬로 그대로 가져와보면, $A \times X = A$에서 $X$는 $I$가 됩니다.   
즉, 항등 행렬(Identity Matrix)는 모든 대각 원소들이 1이며 나머지 원소들이 0인 정방 행렬을 말합니다.항등 행렬은 $I_n$으로 표기합니다.   
\begin{aligned} 
I_2 = \begin{bmatrix} 1 & 0 \newline  
0 & 1 \newline  
\end{bmatrix} 
\quad
I_3 = 
\begin{bmatrix} 1 & 0 & 0 \newline  
0 & 1 & 0 \newline  
0 & 0 & 1 \newline  
\end{bmatrix} 
\end{aligned}    

항등 행렬의 성질인 자기 자신이 나오는 것을 행렬의 곱셈 예시를 통해 확인해보겠습니다. 여기서 행렬의 곱셈은 이전에 Column space로 볼 수 있다고 했습니다.   
\begin{aligned} 
A =& 
\begin{bmatrix} 1 & 2 & 3 \newline  
4 & 5 & 6 \newline  
\end{bmatrix} 
\newline
IA =& 
\begin{bmatrix} 1 & 0 \newline  
0 & 1 \newline  
\end{bmatrix} 
\begin{bmatrix} 1 & 2 & 3 \newline  
4 & 5 & 6 \newline  
\end{bmatrix} 
= 
\begin{bmatrix} 1 & 2 & 3 \newline  
4 & 5 & 6 \newline  
\end{bmatrix} 
\newline
AI =& 
\begin{bmatrix} 1 & 2 & 3 \newline  
4 & 5 & 6 \newline  
\end{bmatrix} 
\begin{bmatrix} 1 & 0 & 0 \newline  
0 & 1 & 0 \newline  
0 & 0 & 1 \newline  
\end{bmatrix} 
=
\begin{bmatrix} 1 & 2 & 3 \newline  
4 & 5 & 6 \newline  
\end{bmatrix} 
\end{aligned}


## 역행렬(Inverse Matrix)   
역행렬을 알기전에 이전과 동일하게 **역원**에 대해 우선 알아보겠습니다. 역원의 정의는 하기와 같습니다.   
> 역원(逆元, 영어: inverse element)이란 두 원소를 연산한 결과가 항등원일 때, 한 편에 대하여 다른 편을 이르는 말입니다.   
덧셈에서의 반수와 곱셈에서의 역수를 일반화한 개념입니다.

즉, 항등원이 나오게 해주는 수가 역원입니다. 예를 들어, 곱셈의 역원을 구해보겠습니다. $3 \times X = 1$과 같이 결과가 항등운인 1이 나옵니다. 이 때의 $X$는 $\frac{1}{3}$이 됩니다. 이 수가 바로 역원입니다.   

그럼 이제 역행렬에 대해 알아보겠습니다. 역원의 개념을 행렬로 그대로 가져와보면, $A \times X = I$에서 $X$는 $A^{-1}$가 됩니다.   
역행렬은 **정방 행렬(Square Matrix)**에 대해서만 존재합니다. 또한, 역행렬은 존재한다면 **하나의 행렬에 대해서 유일하게 한개의 역행렬**만 존재합니다. 하지만 반드시 **행렬에 대해 역행렬이 존재하지는 않습니다**. 만약 역행렬이 존재할 수 있는 행렬을 보통 **Invertible 하다**라고 말합니다. 역행렬은 행렬의 곱셈인 $Ax = b$에서 $x$를 구하는데 용이하게 쓰입니다. 이 부분은 추후에 다시 알아보도록 하겠습니다.   

## 대각 행렬(Diagonal Matrix)
대각 행렬은(Diagonal Matrix)은 대각 성분 이외의 나머지 성분들이 모두 0으로 값을 가지는 행렬을 말합니다. 정방 행렬이 아니어도 됩니다. 이전에 배웠던 항등 행렬(Identity Matrix)도 대각 행렬입니다. 대각 성분(diagonal)의 원소들은 0포함 아무런 원소들을 가져도 가능합니다. 하지만, off-diagnoal(대각 성분 이외의 것들)은 모두 0으로 가져야합니다. 하기에 대각 행렬의 예시를 몇개 보여주겠습니다.    
\begin{aligned} 
\begin{bmatrix} 3 & 0 & 0 \newline  
0 & 5 & 0 \newline  
\end{bmatrix} 
\quad
\begin{bmatrix} 3 & 0 & 0 \newline  
0 & 0 & 0 \newline  
0 & 0 & 0 \newline  
\end{bmatrix} 
\quad
\begin{bmatrix} 
1 & 0  \newline  
0 & 5  \newline  
\end{bmatrix} 
\end{aligned}   

일반적으로 대각 행렬(Diagonal Matrix)은 정방 행렬에 대한 행렬을 말하며, 정방 행렬이 아닌 경우에는 rectangular diagonal matrix라고 부르기도 합니다.    
당연히 알 수 있듯이 대각 행렬은 **symmetric matrix**입니다.   

가끔 Diagonal 표기로 $D = diag(\underline{a}), \quad \begin{bmatrix} a_1 \newline a_2 \newline a_3 \end{bmatrix}$라고 표기하기도 하는데, 해당 뜻은, $D = \begin{bmatrix} a_1 & 0 & 0 \newline 0 & a_2 & 0 \newline 0 & 0 & a_3 \end{bmatrix}$을 뜻합니다.   
또한, 반대로 $diag(D)$라고도 표기하기도 합니다. 이 뜻은, 반대로 대각 성분들만 갖고오면 됩니다. $diag(\begin{bmatrix} 1 & 2 & 3 \newline 4 & 5 & 6 \newline 7 & 8 & 9 \end{bmatrix}) = \begin{bmatrix} 1 \newline 5 \newline 9 \end{bmatrix}$가 됩니다.  

## 직교 행렬(Orthogonal Matrix)
직교 행렬(Orthogonal Matrix)은 Orthonormal vector들로 구성된 Matrix입니다. 그럼 우선 Orthonormal이 어떤건지부터 알아보겠습니다.   
### Unit Vector
단위 벡터(Unit Vector)을 기억하시나요? 보통 벡터는 **크기와 방향 성분**을 가집니다. 벡터 $\underline{v} = \begin{bmatrix} 3 & 4 \end{bmatrix}$에서 $\underline{v}$는 크기가 5이며 1사분면 대각선의 방향을 가지는 벡터입니다. 만약 벡터의 크기의 성분을 제외한 방향 성분에 대해서만 필요로 한다면 어떻게 해야할까요? 이 때 사용 되는 것인 **단위 벡터(Unit Vector)**입니다.   
Unit Vector는 **크기가 1인 벡터**를 의미합니다. 즉, $\underline{v}$의 Unit Vector는 $\frac{1}{5}\begin{bmatrix} 3 & 4 \end{bmatrix}$이 됩니다. 즉, <span style='color:red'>**Unit Vector는 크기 성분에는 영향을 끼치지 않습니다.**</span>   
이런 Unit Vector는 정규화 벡터(normalized vector)라고도 부릅니다. 즉, <span style='color:red'>**서로 다른 크기를 가지고 있는 벡터들을 동일한 크기에서 바라보기 위해 정규화를 시킨 벡터**</span>를 뜻합니다. 

### Orthonormal Vector
직교 벡터(Orthogonal Vector)는 두 벡터의 사이의 각도가 90도인 직각을 이루는 벡터를 의미합니다. 즉 $\underline{a} \bot \underline{b}$인 두 벡터를 의미하며, 내적값은 0이 됩니다.   
그럼 Orthonormal Vector는 **Orthogonal Vector**이면서 **Unit Vector**인 벡터를 의미합니다. 즉, <span style='color:red'>**두 벡터가 이루는 각이 90도(내적은 0)이며, 벡터의 크기는 1인 방향 성분만을 가지는 벡터**</span>가 Orthonormal Vector입니다. 수식 정의는 하기와 같습니다.   
<img src="../../../assets/images/LinearAlgebra/2023-11-29-Matrix types/orthonormal vector definition.jpg" alt="orthonormal vector defintion" style="zoom:80%;" />    
orthonormal vector를 각각 $q_i, q_j$라고 한다면, $q_i$는 자신과 직교하지 않습니다. 따라서, Unit Vector이므로 크기가 1이기 때문에, 내적값은 1이 됩니다. 하기의 orthonormal vector의 예시를 보여드리겠습니다.   
<img src="../../../assets/images/LinearAlgebra/2023-11-29-Matrix types/orthonormal vector1.jpg" alt="orthonormal vector1" style="zoom:80%;" />    
$\underline{a}, \underline{b}$는 모두 크기가 1인 unit vector입니다. 또한, 내적값이 0이며 직교하는 벡터이므로 orthonormal vector입니다.   

이러한 **orthonormal vector들을 행렬의 column vector로 가지는 행렬**이 직교 행렬(Orthogonal Matrix)가 됩니다. 이러한 **orthonormal vector들이 Orthogonal Matrix의 Basis**가 됩니다. Orthogonal Matrix는 orthonormal vector들로 이루어져 있기 때문에 크기에 영향을 미치지 않습니다. 따라서, 추후에 수치 연산을 하는 과정에서 값이 무한정 커지거나 작아지는 현상인 **overflow나 underflow**에 대해 강건할 수 있습니다.   



### Orthogonal Matrix
방금전에 설명했다 시피, Orthogonal Matrix는 **orthonormal vector들을 column vector로 가지는 Matrix**입니다. 따라서, Orthogonal Matrix는 **row vector와 column vecotr들이 자기 자신을 제외한 나머지 모든 row와 column vector과 직교(내적=0)이며 Unit Vector인 Matrix**를 의미합니다. 참고로 Orthogonal Matrix는 **정방 행렬(Square Matrix)에 대해서만 정의**됩니다.   
orthonormal vector들을 column vector로 삽입한 행렬이 Orthogonal Matrix가 되니, Orthogonal Matrix인 $Q$에 대해서는 하기를 만족하게 됩니다.   

\begin{aligned} 
Q^T Q =& I \newline   
Q^T Q =& \quad   
\begin{bmatrix} 
\ldots & q_{1}^{T} & \ldots  \newline  
\vdots & \vdots & \vdots  \newline  
\ldots & q_{n}^{T} & \ldots \newline
\end{bmatrix}   
\quad
\begin{bmatrix} 
\vdots & \dots & \vdots  \newline  
q_1 & \ldots & q_n  \newline  
\vdots & \vdots & \vdots  
\end{bmatrix}   
=   
\begin{bmatrix} 
1 & \ldots & 0  \newline  
\vdots & \vdots & \vdots  \newline  
0 & \ldots & 1  
\end{bmatrix}
\end{aligned}   

즉, 내적을 해서 자기 자신의 성분을 제외한 나머지 성분들은 모두 0의 값을 가지게 됩니다. 따라서, 자기 자신과 자기 자신을 제외한 나머지 모든 vector들과 직교합니다.    
역행렬은 항등원을 결과로 만들어주는 행렬이라고 했습니다. $Q^{-1}Q = I$를 만족하는 $Q^{-1}$가 역행렬인데, Orthogonal Matrix에서는 $Q^T$가 해당 식을 만족하게 됩니다. 따라서, 역행렬이 곧 전치 행렬과 동일한 **$Q^{-1} = Q^T$**가 됩니다.