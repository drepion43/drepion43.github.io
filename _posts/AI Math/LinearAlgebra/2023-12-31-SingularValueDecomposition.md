---
title:  "SVD(Singular Value Decomposition)"
categories: LinearAlgebra
tag: [theory, math]
tags: [Jekyll, MathJax]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: SVD의 정의와 계산방법
comments: true
date: 2023-12-31
toc_sticky: true
---

## 특이값 분해(SVD)란?
위키 백과에서의 특이값 분해의 정의는 하기와 같습니다.   
> 특잇값 분해(Singular Value Decomposition, SVD)는 행렬을 특정한 구조로 분해하는 방식으로, 신호 처리와 통계학 등의 분야에서 자주 사용됩니다.   
실수나 복소수로 이루어진 체 K의 원소로 구성되는 m × n 행렬 M에 대해, M은 다음과 같은 세 행렬의 곱으로 분해할 수 있습니다.   
$M = U \Sigma V^{\*}$   
이 행렬의 특징은 다음과 같습니다.   
\- $U$는 m x m 크기를 가지는 유니터리 행렬입니다.   
\- $\Sigma$는 m x n 크기를 가지며, 대각선상에 있는 원소의 값은 음수가 아니며 나머지 원소의 값이 모두 0인 대각행렬입니다.   
\- $V^{\*}$는 V의 켤레전치 행렬로, n x n 유니터리 행렬입니다.   
행렬 M을 이와 같은 세 행렬의 곱으로 나타내는 것을 M의 특잇값 분해라고 합니다.   
일반적으로 $\Sigma$행렬은 더 큰 값이 먼저 나오도록, 즉, $(\Sigma)_{i,i}$보다 $(i+1, i+1)$이 되도록 구하며, 이렇게 할 경우 $\Sigma$는 M에 따라 유일하게 결정됩니다.   

### 특이값 분해(SVD)
우선 특이값 분해(SVD)의 필요성에 대해 말해보겠습니다. 이전 포스팅에서 저희는 Egien Decomposition을 배웠습니다. 하지만 이 Eigen Decomposition은 제약조건이 존재했던 것을 기억할 것입니다. 그 제약 조건은 하기와 같습니다.   
① n x n인 Square Matrix에서만 가능합니다.   
② Symmetric Matrix여야 합니다.  
하지만, SVD의 경우에는 **m x n에서도 Decomposition이 가능하며, Square이 아닌 Rectangular에서도 가능**합니다. SVD의 Decomposition은 하기와 같은 꼴로 나타낼 수 있습니다.   
\begin{aligned}    
A = U \Sigma V^T
\end{aligned}   

여기서 4가지 행렬인 $(A, U, \Sigma, V)$의 크기와 모양이 어떤꼴인지에 대해 알아보겠습니다.   
\begin{aligned}    
A : \; m \; \times \; n \quad Rectangular \; Matrix \newline   
U : \; m \; \times \; m \quad Orthognoal \; Matrix \newline   
\Sigma : \; m \; \times \; n \quad Similar \; Diagnoal \; Matrix \newline   
V : \; n \; \times \; n \quad Orthognoal \; Matrix \newline   
\end{aligned} 

그럼 이제 $A$를 $(U, \Sigma, V)$로 Decomposition하는 방법에 대해 알아보겠습니다. 그 전에 중요하게 알아둬야할 사실이 있습니다.   
$A$는 Rectangular Matrix이어도 항상 성립하는 성질이 있습니다. <span style='color:red'>**$A A^T$와 $A^T A$는 무조건 Symmetric**</span>하다는 성질입니다. Symmetric의 성질에 대해 다시 한번 더 알려드리겠습니다. 어떤 $B$ 행렬이 Symmetric 하다면, $B^T = B$를 만족합니다. 그럼 $(A A^T)^T = A A^T$도 성립함을 알 수 있습니다. 똑같이 $(A^T A)^T = A^T A$도 성립하는 것을 알 수 있습니다. 따라서, $A A^T$와 $A^T A$는 Symmetric이라는 것을 알 수 있습니다. 그럼 이어서 Symmetric하다면, 이전 eigen decomposition에서 배웠던 <span style='color:red'>**Symmetric Matrix &rarr; Diagnolizable &rarr; Orthognoal Matrix인 $Q \Lambda Q^T$로 Decomposition이 가능**</span>하다는 것을 기억하실 겁니다.    

그럼 상기의 내용을 정리해보겠습니다. <span style='color:red'>**어떤 행렬(Rectangular Matrix) $A$에 대해 $A^T A, AA^T$는 항상 Square Matrix가 되며, Symmetric 하며, Diagnolizable도 가능하며, $Q \Lambda Q^T$로 Decomposition이 가능**</span>합니다.   

이 말은 곧 이전에 배웠던 Eigen Decomposition을 할 수 있다는 말과 동일해집니다. 이제 하기에 Decomposition을 해보겠습니다.   
\begin{aligned}    
A =& U \Sigma V^T \newline   
A A^T =& U \Sigma V^T (U \Sigma V^T)^T \newline   
=& U \Sigma V^T V \Sigma^T U^T \quad(V \; : \; Orthognoal \; Matrix) \newline   
=& U \Sigma \Sigma^T U^T \newline   
=& Q \Lambda Q^T \;(Q =U , \; \Sigma \Sigma^T = \Lambda) \newline   
A^T A =& V \Sigma^T U^T U \Sigma V^T =  V \Sigma^T \Sigma V^T() \newline   
=& Q \Lambda Q^T \;(Q =V , \; \Sigma^T \Sigma = \Lambda) \newline   
\end{aligned}    

상기는 $A A^T$의 Eigen Decomposition을 통해 $U$를 구했으며, $A^T A$의 Decomposition을 통해 $V$를 구할 수 있습니다. 그럼 $\Sigma$는 어떻게 되는지 알아보겠습니다.($\Sigma$ : m x n)   
<img src="../../../assets/images/LinearAlgebra/2023-12-31-SingularValueDecomposition/Singular Value 1.jpg" alt="Singular Value 1" style="zoom:80%;" />    
상기의 이미지에서 알 수 있듯이, $\Sigma$는 eigen value들과 그 크기가 초과하는 경우에는 0의 value를 갖고 있는 Matrix꼴로 나타나집니다. 그럼 $\Sigma \Sigma^T, \Sigma^T \Sigma$인 둘 중에 하나를 구한 후 제곱근을 구해주면 됩니다.(보통 양의 제곱근으로 구합니다.)   
추가적으로 $U = \begin{bmatrix} \underline{u_1} & \underline{u_2} & \ldots \end{bmatrix}$을 담고 있는데 이 $U$를 **Left Singular Vector**라고 합니다. 그럼 $V=\begin{bmatrix} \underline{v_1} & \underline{v_2} & \ldots \end{bmatrix}$을 담고 있는데 이 $V$를 **Right Singular Vector**라고 합니다. 그리고 $\Sigma$가 담고 있는 $\sigma$들을 **Signular Value**라고 부릅니다.    

SVD의 한 가지 중요한 성질에 대해 말씀드리겠습니다. Eigne value 포스팅 부분에서 \"Diagonalizable한 matrix의 non-zero eigen value의 수는 rank와 동일합니다.\"를 기억하실 겁니다. 그럼 $AA^T$도 Diagnolizable하기 때문에, <span style='color:blue'>**$rank(AA^T)$는 non-zero singular value의 수**</span>인 것을 알 수 있습니다. 근데 rank는 작은 것을 따라간다고 했었습니다. 따라서 $rank(AA^T) = rank(A)$이고 $rank(A^T A)=rank(A)$인 것을 알 수 있습니다. 따라서 <span style='color:blue'>**$rank(A)$는 non-zero singular value의 수**</span>것을 알 수 있습니다.   

### SVD 기하학적 해석
이번에는 SVD를 기하학적으로 생각해보겠습니다. 참고로 $(U, V)$는 Orthognoal Matrix이니 $QQ^T=I$이며, $Q^{-1} = Q^T, \quad Q^{-T} = Q$인 것을 인지하고 있어야합니다.   
\begin{aligned}    
A =& U \Sigma V^T \newline   
AV =& U \Sigma \newline   
A \begin{bmatrix} \underline{v_1} & \underline{v_2} & \ldots \end{bmatrix} =& \begin{bmatrix} \underline{u_1} & \underline{u_2} & \ldots \end{bmatrix} \begin{bmatrix} \sigma_1 &  & \newline & \sigma_2 & \newline & & \ldots \end{bmatrix} \newline   
=& \begin{bmatrix} \sigma_1 \underline{u_1} & \sigma_2 \underline{u_2} & \ldots \end{bmatrix}
\end{aligned}   

그럼 상기의 수식에서 $A\underline{v_1} = \sigma_1 \underline{u_1}, A\underline{v_2} = \sigma_2 \underline{u_2}, \ldots$인 것을 알 수 있습니다. 근데 $U,V$는 Orthognoal Matrix라고 했으니, $\underline{u_1} \bot \underline{u_2}, \; \underline{v_1} \bot \underline{v_2}$의 성질을 가지고 있습니다. 즉, $V$ Matrix는, A를 통과시켰을 때도, 여전히 **$A\underline{v_1} \bot A\underline{v_2} \bot \ldots$인 수직한 성질은 보유**한 채 크기만 변했다는 말이 됩니다.    


이번에는 $A\underline{x}$에 대해 살펴보겠습니다.   
\begin{aligned}    
A =& U \Sigma V^T \newline   
A \underline{x} =& U \Sigma V^T \underline{x}
\end{aligned}   

$\underline{x}$를 A를 통해 선형변환을 하면 상기와 같이 됩니다. 여기서 두가지를 알 수 있습니다.   
① $U, V$는 모두 Orthognoal Matrix이기 때문에 <span style='color:blue'>**determinant인 $det(U)=1, det(V)=1$**</span>을 갖습니다. 그 이유는 $det(U U^T) = det(I) = 1$인 것을 알 수 있습니다. 그럼 $det(U)$를 구해보면, $det(U U^T) = det(U) det(U^T) = det^2(U) = \pm 1$을 알 수 있습니다. 그럼 <span style='color:red'>**Orthognoal Matrix의 determinant는 1 or -1 인 것**</span>을 알 수 있습니다.   
② <span style='color:red'>**Orthognoal Matrix의 egien value들은 $\pm 1$**</span>인 것을 알 수 있습니다.    
$U\underline{x} = \lambda \underline{x}$인 eigen vector와 eigen value를 갖는다고 해보겠습니다. 그럼 스스로에게 내적을 진행해보면 하기와 같이 됩니다.   
$\underline{x^T} U^T U \underline{x} = \lambda^2 \underline{x^T} \underline{x}$   
그럼 $U^T U = I$이니, 최종적으로 $\underline{x^T} \underline{x} = \lambda^2 \underline{x^T} \underline{x}$이 되니, $\lambda = \pm 1$인 것을 알 수 있습니다.    

상기의 두가지를 통해 알 수 있는 것은, <span style='color:red'>**우선 방금전에 알아낸 $U, V$의 eigen value인 $\lambda = \pm 1$이다는 뜻은 $U,V$를 통해서는 $\underline{x}$의 크기를 바꿀 수는 없다**</span>는 뜻이 됩니다. 즉, 선형변환측면에서 $(U, \Sigma, V^T)$를 하나씩 통과시키는 것을 생각해보면, $V$를 통해 우선 방향만 바뀐 후, $\Sigma$를 통해 크기를 변경, 다시 $U$를 통해 방향을 바꿔 최종적으로 A를통한 선형변환이 일어난다는 뜻입니다. 

### 응용

#### 데이터 압축
\begin{aligned}    
A =& U \Sigma V^T \newline   
A =& \begin{bmatrix} \underline{u_1} & \underline{u_2} & \ldots \end{bmatrix} \begin{bmatrix} \sigma_1 &  & \newline & \sigma_2 & \newline & & \ldots \end{bmatrix} \begin{bmatrix} \underline{v_1^T} \newline \underline{v_2^T} \newline \ldots \end{bmatrix} \newline   
=& \begin{bmatrix} \sigma_1 \underline{u_1} & \sigma_2 \underline{u_2} & \ldots \end{bmatrix} \begin{bmatrix} \underline{v_1^T} \newline \underline{v_2^T} \newline \ldots \end{bmatrix} \newline   
=&  \sigma_1 \underline{u_1} \underline{v_1^T} + \sigma_2 \underline{u_2} \underline{v_2^T} + \ldots \newline
\end{aligned}    

<img src="../../../assets/images/LinearAlgebra/2023-12-31-SingularValueDecomposition/Data Compression1.jpg" alt="Data Compression 1" style="zoom:80%;" />    
상기와 같이 최종적으로 $\sigma_1 \underline{u_1} \underline{v_1^T} + \sigma_2 \underline{u_2} \underline{v_2^T} + \ldots$으로 나타내집니다. 즉, **rank-1 matrix**로 표현이 됩니다. 이전 Eigen Decomposition의 포스팅에서와 동일하게 행렬 $A$를 rank-1 Matrix로 쪼개놓은 것들이 됩니다. 또, $\sigma$들의 값의 크기순으로 내림차순으로 정렬을 했다고 했을 때, $\sigma$값이 큰 Matrix만 남겨놓으면 데이터 압축이 됩니다. Eigen Value를 통한 압축과 다른 점은 Eigen의 경우에는 **Symmetric Matrix에 대한 Decomposition**이지만, SVD의 데이터 압축은 상관이 없어집니다. 

#### PCA(차원 축소)
PCA도 이전 포스팅에서 다뤘습니다. 간단하게 말씀을 다시 드리면, PCA는 **분산이 가장큰 vector로 Projection을 시켰을 때, 에러가 가장 작아진다**입니다. 하기에 PCA의 정의를 수식으로 다시 설명하겠습니다.   
\begin{aligned}    
max_{\underline{u}} \underline{u^T} R_d \underline{u} \newline   
R_d =& \frac{1}{N} \sum \underline{d_i} \underline{d_i^T} \newline   
=& \frac{1}{N} \begin{bmatrix} \underline{d_1} &  \underline{d_2}  & \underline{d_3} & \ldots \end{bmatrix} \begin{bmatrix} \underline{d_1^T} \newline  \underline{d_2^T}  \newline \underline{d_3^T} \newline \vdots \end{bmatrix} \newline   
=& \frac{1}{N} D D^T 
\end{aligned}   

상기의 수식에서 PCA의 $R_d=\frac{1}{N} D D^T$로 표현할 수 있습니다. 여기서 **$D D^T$는 무조건 Symmetric Matrix**라고 위에서 설명했습니다. SVD에서는 $D$가 Symmetric이 아니더라도 $D D^T$는 Symmetric이기 때문에 하기와 같이 나타낼 수 있던 것을 기억하실 겁니다.   
\begin{aligned}    
A A^T =& U \Sigma V^T V \Sigma^T U^T \newline   
=& U \Sigma \Sigma^T U^T
\end{aligned}   

상기의 수식을 보면, 이전 PCA는 $R_d$의 Eigen vector를 찾는다고 했습니다. 그것이 이번 SVD로 확인해보면 **A의 $U$인 Left Singular Vector**가 되는 것을 확인할 수 있습니다. 즉, **$D$를 SVD해서 $U$인 Left Singular Vector**를 구하면 그것이 $R_d$의 Egien vector라는 말이 됩니다. 