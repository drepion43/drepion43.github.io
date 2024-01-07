---
title:  "Pseudo Inverse"
categories: LinearAlgebra
tag: [theory, math]
tags: [Jekyll, MathJax]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: SVD를 응용한 Pseudo Inverse 정의
comments: true
date: 2024-1-5
toc_sticky: true
---

## Pseduo-Inverse
이전 Inverse Matrix 포스팅을 기억하실 겁니다. 보통 Inverse는 **Square Matrix**에 대해서만 정의가 된다고 설명했었습니다. 하지만, **Pseudo-Inverse는 Square가 아니어도, 즉 m x n Matrix에 대해서도 가능**합니다. 요약하자면, <span style='color:red'>**SVD는 어떤 행렬이든간에 Decomposition과 Inverse**</span>를 구할 수 있습니다. 수식으로 정의하면 하기와 같습니다.   
\begin{aligned}    
A =& U \Sigma V^T \newline   
A^{\dagger} \triangleq & V \Sigma^{\dagger} U^T \newline 
\end{aligned}  

Pseduo-Inverse는 상기의 수식과 같이 Dagger라고 부르는 $A^{\dagger}$로 표기합니다.   
여기서 $\Sigma^{\dagger} = \begin{bmatrix} \frac{1}{\sigma_1} &  & \newline & \frac{1}{\sigma_2} & \newline & & \ldots \end{bmatrix}$와 같이 기존 $\Sigma$의 원소들에 Inverse를 취해준 것처럼 **역수값을 가지는 형태이면서 m x n이라면 n x m으로 Size에 대해서 Tranpose**도 됩니다. 그럼 이제 Pseudo-Inverse와 원본과의 곱이 Indentity가 나오는지 확인해보겠습니다.  

### Full-Column Rank and Full-Row Rank
우선 행렬 $A$가 하기의 그림과 같은 형태인 Full-Column Rank인 Matrix라고 해보겠습니다.   
<img src="../../../assets/images/LinearAlgebra/2024-1-5-PseudoInverse/full column.jpg" alt="full column rank" style="zoom:80%;" />    
\begin{aligned}    
A^{\dagger} A =& V \Sigma^{\dagger} U^T U \Sigma V^T \newline   
=& V \Sigma^{\dagger} \Sigma V^T = V I V^T \newline   
=& I_n(n \times n)
\end{aligned}    

A가 Full-Column Rank인 m x n이라고 한다면, $\Sigma^{\dagger}$는 n x m이고 $\Sigma$는 m x n이어서 $\Sigma^{\dagger} \Sigma$는 n x n이 됩니다. <span style='color:blue'>**A의 rank는 non-zero singular value의 개수**</span>라고 했었습니다. 따라서 $\Sigma^{\dagger} \Sigma = \begin{bmatrix} 1 &  & \newline & 1 & \newline & & \ldots \end{bmatrix} = I$인 n x n인 Indentity Matrix가 됩니다. 여기서 **Full-Column Rank일 때, $A^{\dagger} A$는 Identity가 되지만, $A A^{\dagger}$는 Identity가 되지 않습니다.** 그럼 이번에는 $A A^{\dagger} = I$에 대해 알아보겠습니다.   

행렬 $A$가 Full-Row Rank이라고 해보겠습니다. $A$는 하기와 같은 형태입니다.   
<img src="../../../assets/images/LinearAlgebra/2024-1-5-PseudoInverse/full row.jpg" alt="full row rank" style="zoom:80%;" />    
\begin{aligned}    
A A^{\dagger} =& U \Sigma V^T V \Sigma^{\dagger} U^T \newline   
=& U \Sigma \Sigma^{\dagger} U^T = U I U^T \newline   
=& I_m(m \times m)
\end{aligned}  

A가 Full-Row Rank인 m x n이라고 한다면, $\Sigma^{\dagger}$는 n x m이고 $\Sigma$는 m x n이어서 $\Sigma^{\dagger} \Sigma$는 m x m이 됩니다. <span style='color:blue'>**A의 rank는 non-zero singular value의 개수**</span>라고 했었습니다. 따라서 $\Sigma^{\dagger} \Sigma = \begin{bmatrix} 1 &  & \newline & 1 & \newline & & \ldots \end{bmatrix} = I$인 m x m인 Indentity Matrix가 됩니다.   

이번에는 Pseduo-Inverse를 통해 쉽게 구할 수 있는 문제에 대해 알아보겠습니다. 이전 Least Squares에 대해 다뤘었는데, Least Squares를 Pseudo-Inverse를 통해 구해보겠습니다. 하기는 Least Sqaures에 대한 이미지입니다.  
<img src="../../../assets/images/LinearAlgebra/2024-1-5-PseudoInverse/leas squares.jpg" alt="leas squares" style="zoom:80%;" />    
\begin{aligned}    
\underline{b} \bot A \underline{\hat{x}} \newline   
(\underline{b} - A \underline{\hat{x}}) (A \underline{\hat{x}})^T =& 0 \newline   
\underline{\hat{x^T}} A^T (\underline{b} - A \underline{\hat{x}}) =& 0 \newline   
\underline{\hat{x^T}} =& (A^T A)^{-1} A^T \underline{b}
\end{aligned}  

여기서 우리가 구하고자 하는 것은 $\underline{b} \approx A\underline{\hat{x}}$입니다. 즉, $\underline{b}$와 가장 유사한 $\underline{\hat{x}}$를 찾는 것입니다. 즉, Pseudo-Inverse를 이용하면 하기와 같이 표현할 수 있습니다.   
\begin{aligned}    
\underline{b} \approx A \underline{\hat{x}} \newline   
A^{\dagger} \underline{b} = A^{\dagger} A \underline{\hat{x}} = \underline{\hat{x}}
\end{aligned}   

상기의 수식을 전개해보겠습니다.   
\begin{aligned}    
A^{\dagger} A \underline{\hat{x}} =& I \underline{\hat{x}} =  \underline{\hat{x}} \newline   
A^{\dagger} \underline{b} =& \underline{\hat{x}} = (A^T A)^{-1} A^T \underline{b} \newline   
A^{\dagger} =& V \Sigma^{\dagger} U^T = (A^T A)^{-1} A^T \newline   
(A^T A)^{-1} A^T =& (V \Sigma^T \Sigma V^T)^{-1} V \Sigma^T U^T \newline   
=& (V \begin{bmatrix} \sigma_1^2 &  & \newline & \sigma_2^2 & \newline & & \ddots \end{bmatrix} V^T )^{-1}  V \Sigma^T U^T \newline   
=& V \begin{bmatrix} \frac{1}{\sigma_1^2} &  & \newline & \frac{1}{\sigma_2^2} & \newline & & \ddots \end{bmatrix}(n \times n) \quad V^T V \Sigma^T(n \times m) U^T \newline   
=& V \begin{bmatrix} \frac{1}{\sigma_1^2} &  & & 0 \newline & \frac{1}{\sigma_2^2} & & 0 \newline & & \ddots & 0 \end{bmatrix}(n \times m) \quad U^T \newline   
A^{\dagger} =& V \Sigma^{\dagger} U^T = (A^T A)^{-1} A^T = V \begin{bmatrix} \frac{1}{\sigma_1^2} &  & & 0 \newline & \frac{1}{\sigma_2^2} & & 0 \newline & & \ddots & 0 \end{bmatrix}(n \times m) \quad U^T
\end{aligned}   

상기의 수식을 통해 $A^{\dagger} = (A^T A)^{-1} A^T$인 것을 증명했습니다. 그럼 $A^{\dagger} A = (A^T A)^{-1} A^T (A) = I$인 것을 알 수 있습니다. $A$의 왼쪽에 곱해서 Identity Matrix를 만든 것이어서 **Left Pseduo-Inverse**라고 부릅니다. 그럼 반대로 $A A^{\dagger} = (A) (A^T A)^{-1} A^T = I$도 있으며 이를 **Right Pesudo-Inverse**라고 부릅니다.   
여기서 중요한 점은 <span style='color:blue'>**행렬 $A$가 Full-Column Rank일 때는, Left Pseudo-Inverse를 만족하지만, Right는 만족하지 않습니다. 반대로 행렬 $A$가 Full-Row Rank일 때는, Right Pseudo-Inverse를 만족하지만, Left는 만족하지 않습니다.**</span>   

### Rank-Deficient
이번에는 Rank-Deficient인 경우에 대해 알아보겠습니다. 하기에 예시를 통해 설명해보겠습니다.   
$A$가 3 x 4이며 rank가 2인 행렬이라고 하겠습니다. 그럼 $A$의 SVD는 하기와 같이 됩니다.   
\begin{aligned}    
A =& U \begin{bmatrix} \sigma_1 &  & & 0\newline & \sigma_2 & & 0 \newline & & 0 & 0 \end{bmatrix} V^T \quad(rank=2) \newline   
A^{\dagger} =& V \begin{bmatrix} \frac{1}{\sigma_1} &  & \newline & \frac{1}{\sigma_2} &  \newline & & 0 \newline 0 & 0 & 0 \end{bmatrix} U^T \newline
\end{aligned}   

그럼 이번에는 $A A^{\dagger}$와 $A^{\dagger} A$를 확인해보겠습니다.   
\begin{aligned}    
A A^{\dagger} = U \Sigma \Sigma^{\dagger} U^T \quad (3 \times 3)\newline   
=& U \begin{bmatrix} 1 &  & \newline & 1 &  \newline & & 0 \end{bmatrix} U^T \newline   
=& \underline{u_1} \underline{u_1^T} + \underline{u_2} \underline{u_2^T}
\end{aligned}   

상기의 수식에서 확인할 수 있듯이 Rank-Deficient인 $A$에 대해서 $A A^{\dagger} = \underline{u_1} \underline{u_1^T} + \underline{u_2} \underline{u_2^T}$이 나옵니다. 만약, $A A^{\dagger} = \underline{u_1} \underline{u_1^T} + \underline{u_2} \underline{u_2^T} + \underline{u_3} \underline{u_3^T} = I$ 였다면, Identity Matrix가 나오는데, $\underline{u_3} \underline{u_3^T}$가 빠져있는 상태여서 Identity Matrix는 아니게 됩니다. 그 이유는 **Rank**때문이며, 하지만 Identity는 아니지만 유사는 하게 됩니다. 즉, 제일 기여가 작은 $\underline{u_3} \underline{u_3^T}$이 사라져서 Identity와 유사하게 됩니다. 따라서, **Pseudo-Inverse는 Rank-Deficient일 때, 최대한 Identity Matrix와 유사**하게 만들려고 해준다는 것을 알 수 있습니다.   
그럼 이번에는 $A^{\dagger} A$을 확인해보겠습니다.   

\begin{aligned}    
A^{\dagger} A = V \Sigma^{\dagger} \Sigma V^T \quad (4 \times 4)  \newline   
=& V \begin{bmatrix} 1 &  & & \newline & 1 &  & \newline & & 0  & \newline & & & 0 \end{bmatrix} V^T \newline   
=& \underline{v_1} \underline{v_1^T} + \underline{v_2} \underline{v_2^T}
\end{aligned}   

$A A^{\dagger}$와 똑같이 Identity Matrix와 유사하게 만들어줍니다. 하지만, $A A^{\dagger}$의 경우에는 3 x 3을 2개로 복원한거지만, $A^{\dagger} A $는 4 x 4을 2개로 복원한 것입니다. 따라서 $A^{\dagger} A $가 $A A^{\dagger}$의 Identity Matrix와 유사한 정도의 차이가 더 크게 나타납니다. 만약 4 x 3인 행렬 $A$에 대해서 수행했다면, 상기의 결과와 반대로 나타났을 것입니다.   

따라서 <span style='color:red'>**$A$가 m x n인 행렬일 때, $m < n$이라면 $A A^{\dagger}$을 이용하여 구하는게 Identity Matrix와 가장 유사하게 나타낼 수 있으며, 반대로 $m > n$이라면 $A^{\dagger} A $을 이용하여 구하는게 Identity Matrix와 더 유사**</span>하게 나타낼 수 있습니다.   

마지막으로 행렬 $A$가 m x n인 경우에 대해 정리를 해보겠습니다.    
<span style='color:red'>**Full-Column Rank와 Rank-Deficient일 때 $m > n$일 때는, Pseudo-Inverse를 $A^{\dagger} A$을 사용하여 구하는게, 더 잘 나타낼 수 있습니다.**</span>   
<span style='color:red'>**Full-Row Rank와 Rank-Deficient일 때 $m < n$일 때는, Pseudo-Inverse를 $A A^{\dagger}$을 사용하여 구하는게, 더 잘 나타낼 수 있습니다.**</span>