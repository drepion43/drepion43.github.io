---
title:  "역행렬(Inverse Matrix)"
categories: LinearAlgebra
tag: [theory, math]
tags: [Jekyll, MathJax]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: 역행렬의 의미와 특징 및 행렬식이란
comments: true
date: 2023-12-10
toc_sticky: true
---

## 역행렬(Inverse Matrix)
고등학교 시절에 2 x 2 행렬의 역행렬을 구하는 방법은 공식을 외워서 해결한 경우가 많을 거라고 생각합니다. 왜냐하면 역행렬을 구하는 방법도 다양하지만 직접 구하려면 시간이 좀 소요되기 때문입니다. 따라서, 자주 사용되는 2 x 2의 경우에는 외워서 사용했습니다. 만약 3 x 3의 역행렬을 구하려면 2 x 2 구하는 시간보다 배로는 걸릴 수 있습니다. 그럼 2 x 2의 역행렬 구하는 공식은 하기와 같습니다.   
$A = \begin{bmatrix} a & b \newline c & d  \end{bmatrix}$ &rarr; $A^{-1} = \frac{1}{ad - bc} \begin{bmatrix} d & -b \newline -c & a  \end{bmatrix}$   

### 가우스 조던

그럼 다른 방법인 이전 포스팅에서 배웠던 가우스 조던 소거법(Gauss Jordan Elimination)을 이용한 역행렬을 구해보겠습니다.   
$A \times B = I$를 기억하시나요? 항등행렬을 만들려면 B가 $B = A^{-1}$을 만족하면 됩니다. B는 하기와 같이 구하면 됩니다.   
$\begin{bmatrix} 1 & 2 & 3 \newline 4 & 5 & 6 \newline 7 & 8 & 9 \end{bmatrix} \begin{bmatrix} x_1 & x_2 & x_3 \newline y_1 & y_2 & y_3 \newline z_1 & z_2 & z_3 \end{bmatrix} = \begin{bmatrix} 1 & 0 & 0 \newline 0 & 1 & 0 \newline 0 & 0 & 1  \end{bmatrix}$   
상기는 가우스 조던을 이용할 수 있습니다. 가우스 조던을 이용하여 $\begin{bmatrix} x_1 & x_2 & x_3 \newline y_1 & y_2 & y_3 \newline z_1 & z_2 & z_3 \end{bmatrix}$을 구해주면 됩니다.    
이 방법은 가장 쉬우면서 시간이 많이 소요되는 방법입니다.   

그럼 가우스 조던을 이용하여 2 x 2 행렬의 역행렬을 직접 구해보겠습니다.   
$\begin{bmatrix} a & b \newline c & d  \end{bmatrix} A^{-1} =  \begin{bmatrix} 1 & 0 \newline 0 & 1  \end{bmatrix}$   
$A^{-1} = \begin{bmatrix} a & b & \| & 1 & 0 \newline c & d & \| & 0 & 1 \end{bmatrix}$ 
상기의 확장 행렬을 구해주면 됩니다. 

이전 포스팅과 같이 row 순서대로 $L_1, L_2$로 명시하겠습니다.   
$L_2 - \frac{c}{a} \times L_1 \rightarrow L_1$을 취해보겠습니다.    
$\begin{bmatrix} a & b & \| & 1 & 0 \newline 0 & \frac{ad - bc}{a} & \| & - \frac{c}{a} & 1 \end{bmatrix}$ 

$L_2$를 항등행렬 꼴로 바꿔주겠습니다. $\frac{a}{ad - bc} \times L_2 \rightarrow L_2$을 취해주겠습니다.   
$\begin{bmatrix} a & b & \| & 1 & 0 \newline 0 & 1 & \| & - \frac{c}{ad - bc} & \frac{a}{ad - bc} \end{bmatrix}$ 

$L_1 - b \times L_2 \rightarrow L_1$을 취해주겠습니다.   
$\begin{bmatrix} a & 0 & \| & \frac{ad}{ad - bc} & -\frac{ab}{ad - bc} \newline  0 &  1 & \| & - \frac{c}{ad - bc} & \frac{a}{ad - bc} \end{bmatrix}$ 

$L_1$도 항등행렬 꼴로 바꿔주겠습니다. $\frac{1}{a} \times L_1 \rightarrow L_1$을 취해주겠습니다.    
$\begin{bmatrix} 1 & 0 & \| & \frac{d}{ad - bc} & -\frac{b}{ad - bc} \newline  0 &  1 & \| & - \frac{c}{ad - bc} & \frac{a}{ad - bc} \end{bmatrix}$ 

$A^{-1}$을 구했습니다. 공통 분모로 묶어주면 $A^{-1} = \frac{1}{ad - bc} \begin{bmatrix} d & -b \newline -c & a  \end{bmatrix}$을 얻을 수 있습니다. 

여기서 중요한 점은 $\frac{1}{ad - bc}$에서 $ad - bc = 0$이라면 역행렬이 정의되지 않습니다. 즉, 역행렬이 존재하지 않다는 것을 뜻합니다. 이런 역행렬의 존재 여부를 판단할 수 있는 식을 **행렬식(determinant)**라고 합니다. 

### 역행렬 property

**square matrix A가 invertible하다**(Non Singular Matrix(invertible하지 않다면 Singular Matrix))와 같은 의미에 대해 알아보겠습니다.   
① $det(A) \neq 0$   
② A가 full rank입니다.(즉, $det(A) = 0 $이라면 rank-deficient)  
③ $N(A) = \underline{0}$  
보통 **rank와 determinant**를 보고 역행렬의 존재여부를 따질 수 있습니다.   

이제 역행렬(Inverse Matrix)의 Property에 대해 정리해보겠습니다.   
\- $(AB)^{-1} = B^{-1}A^{-1}$(교환 법칙이 성립 X, 순서를 잘 고려해야합니다)   
\- $(A^{-1})^{-1} = A$   
\- $(kA)^{-1} = \frac{1}{k}A$   
\- $(A^T)^{-1} = (A^{-1})^T$   
\- $det(A^{-1}) = \frac{1}{det(A)}$   

## 행렬식(determinant)
위키 백과에 determinant의 정의는 하기와 같이 명시되어있습니다.   
> 선형대수학에서 행렬식(行列式, 영어: determinant)은 정사각 행렬에 스칼라를 대응시키는 함수입니다.   
실수 정사각 행렬의 행렬식의 절댓값은 그 행렬이 나타내는 선형 변환이 초부피를 확대시키는 배수를 나타내며, 행렬식의 부호는 방향 보존 여부를 나타냅니다.   

### determinant 계산
우선 3 x 3 행렬에 대한 determinant 계산법에 대해 알아보겠습니다.   
$A = \begin{bmatrix} a & b & c \newline d & e & f \newline g & h & i  \end{bmatrix}$   
$det(A) = a(ei - fh) - b(di - fg) + c(dh - eg)$   

여기서 미리 알아야 할 점은 2 x 2 행렬의 determinant를 구하는 방법입니다.   
$A = \begin{bmatrix} a & b \newline c & d  \end{bmatrix}$의 determinant를 구하는 방법은 $det(A) = ad - bc$입니다. 즉, 양 대각선 element들에 대해 곱을 한 후 서로 빼주면 됩니다.   

그럼 3 x 3에서 $ei - fh$를 보면, a를 담고 있는 열과 행을 빼보겠습니다. 그럼 하기와 같이 됩니다.   
$\begin{bmatrix} e & f \newline h & i  \end{bmatrix}$   
이에 대해 2 x 2 determinant를 구하는 방법을 해주면 됩니다. 
이번에는 $di - fg$에 대해 알아보겠습니다. a와 동일하게 b가 담고 있는 열과 행을 빼보겠습니다. 그럼 하기와 같이 됩니다.   
$\begin{bmatrix} d & f \newline g & i  \end{bmatrix}$   
이에 대해 2 x 2 determinant를 구하면 $di - fg$가 됩니다. 

그럼 여기서 b 앞에 붙는 부호는 왜 "\-" 인지 알아보겠습니다.   
우선 A행렬에서 a 원소의 위치는 1행 1열 입니다. a의 위치들을 다 더해주면 2가 됩니다. 따라서 $(-1)^2$이니 "+"가 됩니다. 그럼 b 원소의 위치는 1행 2열입니다. 더하면 3이됩니다. 그럼 $(-1)^{3} = -1$인 "-"가 됩니다. 이런 방법을 **Cofactor expansion** 또는 **Laplace expansion**이라고 부릅니다.   

determinant는 **역행렬을 구했을 때, 역행렬의 각 공통 분모로 사용**됩니다. 

### determinant property
이제 determinant의 property들에 대해 알아보겠습니다.   
\- $det(A) = 0$이라면 A는 singular Matrix입니다. 즉, A는 invertible하지 않다는 뜻입니다.    

\- A가 rank-deficient라면, $det(A) =0 $입니다.   

\- **Diagnoal Matrix에 대해서는 $det(A)=a_{11}a_{22}a_{33}...a_{nn}$**입니다. 즉, 대각 성분들의 곱입니다. (Diagnoal Matrix는 대각에 대해서만 성분이 존재합니다.)   

\- **Triangular Matrix에 대해서는 $det(A)=a_{11}a_{22}a_{33}...a_{nn}$**입니다.   
&nbsp;&nbsp; &rarr; Traingular Matrix는 $A = \begin{bmatrix} 1 & 2 \newline 0 & 6  \end{bmatrix}$, $B = \begin{bmatrix} 1 & 2 & 4 \newline 0 & 6 & 0 \newline 0 & 0 & 3 \end{bmatrix}$, $C = \begin{bmatrix} 2 & 0 & 0 \newline -10 & 2 & 0 \newline 0 & 3 & 4  \end{bmatrix}$와 같이 대각을 축으로 삼각형을 그리는 부분에만 원소가 존재하는 Matrix입니다. 여기서 A,B는 위쪽 모양의 삼각형을 나타내므로 upper triangular matrix라고 하며 C는 lower triangular matrix라고 부릅니다. $det(A) = 6, det(B) = 18, det(C) = 16$입니다.   

\- 항등행렬에 대한 $det(A) = 1$입니다.   

\- $det(CA) = C^n det(A)$입니다. 여기서 A는 n x n Matrix인 경우입니다.   

\- $det(A^T) = det(A)$입니다.   

\- **$det(AB) = det(A)det(B)$**입니다.   

\- **$det(A^{-1})=\frac{1}{det(A)}$**입니다. $det(A A^{-1}) = det(A)det(A^{-1}) = det(I) = 1$을 통해 증명할 수 있습니다.   

\- **$det(A) = \lambda_1 \lambda_2 \lambda_3 ... \lambda_n$**입니다.
