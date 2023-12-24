---
title:  "고유값과 고유벡터(eigen value and eigen vector) 1"
categories: LinearAlgebra
tag: [theory, math]
tags: [Jekyll, MathJax]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: 고유값과 고유 벡터의 정의와 계산법
comments: true
date: 2023-12-23
toc_sticky: true
---

## 고유값과 고유벡터(eigen values and eigen vector)
이번 포스팅에서는 선형대수학에서 아주 중요하며 많이 사용되는 eigen value와 eigen vector에 대해 알아보겠습니다.   
우선 eigen value와 eigen vector의 정의에 대해 알아보겠습니다.   
> 선형대수학에서, 선형 변환의 고유벡터(eigen vector)는 그 선형 변환이 일어난 후에도 방향이 변하지 않는, 0이 아닌 벡터입니다. 고유 벡터의 길이가 변하는 배수를 선형 변환의 그 고유 벡터에 대응하는 고윳값(eigen value)이라고 합니다. 선형 변환은 대개 고유 벡터와 그 고윳값만으로 완전히 설명할 수 있습니다.
고유 벡터와 고유값의 개념은 여러 응용수학 분야에서 중요한 위치를 차지하며, 특히 선형대수학, 함수해석학, 그리고 여러 가지 비선형 분야에서도 자주 사용되기도 합니다.   

상기는 위키피디아에 나와있는 정의입니다.   
### 정의 
좀 더 쉽게 설명을 해보겠습니다.   
\begin{aligned}    
A\underline{v} = \lambda \underline{v}
\end{aligned}    
상기의 식을 만족하는 $\lambda$와 $\underline{v}$가 eigen value와 eigen vector가 됩니다. 즉, $\underline{v}$가 방향은 유지되며 크기만 바뀌는 vector가 eigen vector이고 그 크기를 나타내는 값이 eigen value가 됩니다. 여기서 중요한 점은 행렬 $A$는 반드시 square matrix이어야 합니다.   
$A$가 square matrix가 아닌 m x n matrix라고 가정해보겠습니다. 그럼 $\underline{v}$는 n x 1이 되어야 행렬 곱을 수행할 수 있습니다. 그럼 결과는 m x 1이 나오게 됩니다. 근데 $\lambda$는 scalar를 뜻하기 때문에 결과 vector의 사이즈에 영향을 주지 않습니다. 따라서 $m=n$이 성립하게 됩니다. 따라서, $A$는 반드시 square matrix이어야 합니다.   

한번 행렬을 곧 함수로 생각해보겠습니다. $A\underline{v}$를 하기와 같이 생각해보겠습니다.   
<img src="../../../assets/images/LinearAlgebra/2023-12-23-eigen vector and eigen value1/eigen vector 1.jpg" alt="eigen vector 1" style="zoom:80%;" />    
$\underline{v}$를 행렬 $A$인 함수를 통과시켜 $A\underline{v}$로 된 것이라는 관점으로 보겠습니다. 그럼 이 $\underline{v}$는 행렬 $A$를 통과하는 행위를 **선형 변환**이라고 할 수 있습니다. 여기서 선형은 크게 **Scaling과 additivity**를 만족해야 합니다. 하기에 예시를 통해 추가적으로 설명해보겠습니다.   
$A = \begin{bmatrix} 2  & -1 \newline -1 2 \end{bmatrix}$에 각 다른 vector들을 통과시켜보겠습니다.   
<img src="../../../assets/images/LinearAlgebra/2023-12-23-eigen vector and eigen value1/eigen vector 2.jpg" alt="eigen vector 2" style="zoom:80%;" />    
초록 vector의 경우 $\begin{bmatrix} 0 \newline 1 \end{bmatrix}$이 행렬 $A$를 통과하니 $\begin{bmatrix} -1 \newline 2 \end{bmatrix}$로 **선형 변환**됬다고 할 수 있습니다.    
여기서 중요한 부분이 있는데, 자홍색 vector입니다. 선형 변환 전 $\begin{bmatrix} 1 \newline -1 \end{bmatrix}$인 vector인데 행렬 $A$를 통한 선형 변환 후 $\begin{bmatrix} 3 \newline -3 \end{bmatrix}$이 되었습니다. 즉, 방향은 같지만 크기가 3배가 된 것을 확인할 수 있습니다. 이 자홍색 vector가 우리가 이번 포스팅에서 설명할 eigen vector가 됩니다. 여기서 크기가 3배가 커졌으니 3이 eigen value가 됩니다.   

이 전 포스팅에서는 $A\underline{v}$를 column space 측면에서 바라봤습니다. 이번에는 $A\underline{v}$를 선형 변환 측면에서 바라보겠습니다. 그럼 여기서 행렬이 invertible한지 안한지를 선형 변환 관점에서 해석해보면 <span style='color:blue'>invertible 하다는 뜻은 즉, 행렬 $A$에 대해 다른 입력이 들어왔을 때, 다른 출력이 나온다는 말입니다. 즉, 다른 입력들에 대해 같은 출력이 존재하면 안됩니다.</span> 예를 들어 추가적인 이해를 도와보도록 하겠습니다. $\underline{v_1}, \underline{v_2}$을 $A$를 통한 선형 변환을 시켜주면 $A\underline{v_1}, A\underline{v_2}$가 됩니다. invertible하다는 뜻은  $A\underline{v_1}$을 통해 $\underline{v_1}$을 찾을 수 있다는 뜻입니다. 하지만 만약 $A\underline{v_1}=A\underline{v_2}$라면,  $A\underline{v_1}$을 통해  $\underline{v_1}$으로 찾을 수가 없습니다.  $\underline{v_1}$인지  $\underline{v_2}$인지를 알 수 없기 때문입니다. 
### 고유값과 고유벡터 계산
이제 eigen value와 eigen vector를 구해보겠습니다.   
\begin{aligned}    
A\underline{v} =& \lambda \underline{v} \newline   
A\underline{v} - \lambda \underline{v} = (A - \lambda I) \underline{v} =& \underline{0} \newline   
\end{aligned}    
상기의 식에서 $(A - \lambda I)$나 $\underline{v}$ 중에 하나가 $\underline{0}$라면 식이 성립하게 됩니다. 하지만, eigen vector인 $\underline{v} = \underline{0}$이라면 **trivial solution**이 되기 때문에 취급을 하지 않습니다. 따라서, $(A - \lambda I) = \underline{0}$을 구해주면 됩니다. 여기서 <span style='color:blue'>$(A - \lambda I)$이 역행렬을 가진다면, $(A - \lambda I)^{-1} (A - \lambda I) \underline{v} = (A - \lambda I)^{-1} \underline{0}$으로 $\underline{v} = \underline{0}$을 가질 수 밖에 없게 됩니다. 따라서 역행렬을 가지지않아야 하는 조건인 $det(A - \lambda I) = 0$을 만족해야합니다. </span> 또한, **크래머 정리**에 의해 $det(A - \lambda I) = 0$이어야 하기도 합니다. 그럼 determinant를 통해 eigen value인 $\lambda$를 구할 수 있습니다. 그 후, 구한 $\lambda$를 $N(A - \lambda I)$에 대입해 Null space에 존재하는 vector들을 찾아주면 eigen vector도 구할 수 있습니다. 따라서 **eigen vector는 Null space에 존재하기 때문에 무한하거나 존재하지 않을 수** 있습니다. 하기에 정리해보겠습니다.   
① $det(A - \lambda I) = 0$인 $\lambda$(eigen value) 구해줍니다.   
② 구한 $\lambda$를 통해 $N(A - \lambda I)$인 Null space에 존재하는 vector들 찾아주면 해당 vector들이 eigen vector들이 됩니다.    
여기서 우리는 보통 eigen vector를 $N(A - \lambda I)$의 Null space에 존재하는 vector들 중 **basis를 eigen vector**로 나타냅니다.
### 예시 문제
- $A=\begin{bmatrix} 2 & -1 \newline -1 & 2 \end{bmatrix}$의 eigen value와 eigen vector를 구해보겠습니다.    
$A - \lambda I =\begin{bmatrix} 2 - \lambda & -1 \newline -1 & 2 - \lambda \end{bmatrix}$입니다.   
$det(A - \lambda I) = (2 - \lambda )(2 - \lambda) - 1 = 0$이니 $\lambda = 1 \; or \; 3$   
$A - 1 I=\begin{bmatrix} 1 & -1 \newline -1 & 1 \end{bmatrix}$이니 이 Null space의 basis을 구해보겠습니다.    
$\begin{bmatrix} 1 & -1 \newline -1 & 1 \end{bmatrix}\underline{v} = \underline{0}$에서 $\underline{v} = \begin{bmatrix} 1 \newline 1 \end{bmatrix}$이 됩니다.    
$A - 3 I=\begin{bmatrix} -1 & -1 \newline -1 & -1 \end{bmatrix}$이니 이 Null space의 basis을 구해보겠습니다.   
$\begin{bmatrix} -1 & -1 \newline -1 & -1 \end{bmatrix}\underline{v} = \underline{0}$에서 $\underline{v} = \begin{bmatrix} 1 \newline -1 \end{bmatrix}$이 됩니다.    
- $A=\begin{bmatrix} 2 & 0 \newline 0 & 2 \end{bmatrix}$의 eigen value와 eigen vector를 구해보겠습니다.    
$A - \lambda I =\begin{bmatrix} 2 - \lambda & 0 \newline 0 & 2 - \lambda \end{bmatrix}$입니다.   
$det(A - \lambda I) = (2 - \lambda )(2 - \lambda) = 0$이니 $\lambda = 2$인 중근을 가집니다.   
$A - 2 I=\begin{bmatrix} 0 & 0 \newline 0 &0 \end{bmatrix}$이니 이 Null space의 basis을 구해보겠습니다.    
$\begin{bmatrix} 0 & 0 \newline 0 & 0 \end{bmatrix}\underline{v} = \underline{0}$이니 2차원 전체를 가지게 됩니다. basis는 $\begin{bmatrix} 1 \newline 0 \end{bmatrix} \begin{bmatrix} 0 \newline 1 \end{bmatrix}$으로 잡아보겠습니다.     
- $A=\begin{bmatrix} 1 & 1 \newline 0 & 1 \end{bmatrix}$의 eigen value와 eigen vector를 구해보겠습니다.    
$A - \lambda I =\begin{bmatrix} 1 - \lambda & 1 \newline 0 & 1 - \lambda \end{bmatrix}$입니다.   
$det(A - \lambda I) = (1 - \lambda )(1 - \lambda) = 0$이니 $\lambda = 1$인 중근을 가집니다.   
$A - 2 I=\begin{bmatrix} 0 & 1 \newline 0 &0 \end{bmatrix}$이니 이 Null space의 basis을 구해보겠습니다.    
$\begin{bmatrix} 0 & 1 \newline 0 & 0 \end{bmatrix}\underline{v} = \underline{0}$이니 basis는 $\begin{bmatrix} 1 \newline 0 \end{bmatrix}$으로 잡아보겠습니다.   
- $A=\begin{bmatrix} 0 & -2 \newline 2 & 0 \end{bmatrix}$의 eigen value와 eigen vector를 구해보겠습니다.    
$A - \lambda I =\begin{bmatrix} - \lambda & -2 \newline 2 & - \lambda \end{bmatrix}$입니다.   
$det(A - \lambda I) = \lambda^2 + 4 = 0$이니 $\lambda$는 허수를 가집니다.   
이 경우에는 실수 eigen value는 없다고 말하며, No eigen vector가 없다고 합니다. 즉, 방향이 유지되는 vector가 없다고 말할 수 있습니다. 이런 행렬을 회전 행렬이라고도 말합니다.   

이런 예시들을 통해 알 수 있는 점은 **Symetric Matrix가 아닌 경우에는 eigen vector는 rank들과 관련이 없다**는 점입니다. 