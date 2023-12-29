---
title:  "고유값과 고유벡터(eigen value and eigen vector)2"
categories: LinearAlgebra
tag: [theory, math]
tags: [Jekyll, MathJax]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: 고유값 분해(eigen decomposition)
comments: true
date: 2023-12-28
toc_sticky: true
---

## 고유값과 고유벡터(eigen values and eigen vector)
먼저 eigen value와 eigen vector의 정의에 대해 다시 한번 더 짚어보겠습니다.   
> 선형대수학에서, 선형 변환의 고유벡터(eigen vector)는 그 선형 변환이 일어난 후에도 방향이 변하지 않는, 0이 아닌 벡터입니다. 고유 벡터의 길이가 변하는 배수를 선형 변환의 그 고유 벡터에 대응하는 고윳값(eigen value)이라고 합니다. 선형 변환은 대개 고유 벡터와 그 고윳값만으로 완전히 설명할 수 있습니다.
고유 벡터와 고유값의 개념은 여러 응용수학 분야에서 중요한 위치를 차지하며, 특히 선형대수학, 함수해석학, 그리고 여러 가지 비선형 분야에서도 자주 사용되기도 합니다.   

상기는 위키피디아에 나와있는 정의입니다.   

이전 포스팅에서는 고유값과 고유벡터의 정의와 계산법에 대해 알아보았습니다. 이번 포스팅에서는 <span style='color:red'>**고유값 분해**</span>에 대해 알아보겠습니다.

### 고유값 분해(eigen decomposition)
우선 예시를 들어 설명해보겠습니다. 2 x 2인 행렬 A가 존재하며, 행렬 A의 eigen value가 $\lambda_1, \lambda_2$이며, 무수한 eigen vector 중 basis(서로 independent)인 2개를 선택한 eigen vector가 $\underline{v_1}, \underline{v_2}$라고 해보겠습니다. 그럼 하기와 같이 표현할 수 있습니다.   
\begin{aligned}    
A\underline{v_1} = \lambda_1 \underline{v_1}, \quad A\underline{v_2} = \lambda_2 \underline{v_2} \newline    
\end{aligned}
이 표기를 한개의 식으로 바꿔서 표현해보겠습니다.   

\begin{aligned}    
A \begin{bmatrix} \underline{v_1} & \underline{v_2} \end{bmatrix} =& \begin{bmatrix} \lambda_1 \underline{v_1} & \lambda_2 \underline{v_2} \end{bmatrix} \newline   
A \begin{bmatrix} \underline{v_1} & \underline{v_2} \end{bmatrix} =& \begin{bmatrix} \underline{v_1} & \underline{v_2} \end{bmatrix} \begin{bmatrix} \lambda_1 & 0 \newline  0 & \lambda_2 \end{bmatrix} \newline   
AV =& V \Lambda
\end{aligned}   
상기와 같이 $\begin{bmatrix} \underline{v_1} & \underline{v_2} \end{bmatrix} = V$, $\begin{bmatrix} \lambda_1 & 0 \newline  0 & \lambda_2 \end{bmatrix} = \Lambda$로 표현을 할 수 있습니다. 그럼 여기시ㅓ $V^{-1}$을 취해주면 $A$를 구할 수 있을 것입니다.   

\begin{aligned}    
AV =& V \Lambda \newline   
A =& V \Lambda V^{-1}
\end{aligned}    
여기서 취해준 행위는 A를 eigen value와 eigen vector로 분해하여 표현을 한 것이 됩니다. 이 행위를 <span style='color:red'>**Decomposition**</span>이라고 합니다.    
eigen decomposition에서 알 수 있는 재밌는 사실이 하나 더 있습니다. 이번에는 반대로 $\Lambda$를 구하기 위해 모양을 바꿔보겠습니다.   

\begin{aligned}    
AV =& V \Lambda \newline   
V^{-1} AV =& \Lambda
\end{aligned}    
상기와 같이 $\Lambda = V^{-1} A V$로 표현할 수 있습니다. 여기서 $\Lambda$는 **대각 성분에만 값을 가지는 Diagnoal Matrix**입니다. 이 모양을 만들수 있는 경우를 <span style='color:blue'>**A는 Diagnolizable한 matrix이다**</span> 라고 합니다. 여기서 알 수 있는 매우 중요한 점이 있습니다. <span style='color:red'>**A가 n x n 이며, Diagnolizable하다면, independent한 eigen vector는 n개가 존재**</span>합니다.   
### 고유값 분해(eigen decomposition)의 성질
\begin{aligned}    
A =& V \Lambda V^{-1}
\end{aligned}   
하기에는 eigen decomposition을 할 수 있으면 왜 좋은가에 대해 알아보겠습니다.   

① $A^{k} = V \Lambda V^{-1} V \Lambda V^{-1} V \Lambda V^{-1} \ldots = V \Lambda^{k} V^{-1}$  
&rarr; $\Lambda$는 대각 성분만 가지고 있기 때문에 대각 성분들에 k제곱을 취해주면 됩니다.       
② $A^{-1} = (V \Lambda V^{-1})^{-1} = V \Lambda^{-1} V^{-1}$   
&rarr; Diagnoal Matrix의 inverse는 대각 성분들에 대해 역수를 취해주면 됩니다.($\lambda_1^{-1} = \frac{1}{\lambda_1}$)   
③ $det(A) = det(V \Lambda V^{-1}) = det(V) det(\Lambda) det(V^{-1}) = det(V) det(\Lambda) \frac{1}{det(V)} = det(\Lambda)$   
&rarr; Diagonal Matrix의 determinant는 determinant 내용 포스팅에서 배웠듯이 대각 성분들의 곱이 됩니다.($\lambda_1 \lambda_2 \ldots = \Pi_{i=1}^{n} \lambda_i$)   
④ $tr(A) = tr(V \Lambda V^{-1}) = tr(\Lambda V^{-1} V) = tr(\Lambda)$   
&rarr; tracer의 **Cyclic Property**에 의해 상기와 같이 표현됩니다. trace는 대각 성분의 합이 됩니다.($\lambda_1 + \lambda_2 + \ldots = \sum_{i=1}^{n} \lambda_i$)    
⑤ **rank-deficient는 $det(A)=0$**이 됩니다.   
&rarr; $det(A)=0$이라는 뜻은, determinant는 모든 $\lambda$의 곱이라고 했습니다. 그럼 <span style='color:blue'>**0인 egien value가 적어도 하나가 존재한다**</span>는 뜻이 됩니다.   

이번에는 eigen decomposition의 성질에 대해 알아보겠습니다.   

① $A^T$의 eigen value와 $A$의 eigen value는 동일합니다.    
&rarr; $det(A) = det(A^T)$이 됩니다. 따라서, $det(A - \lambda I) = det((A - \lambda I)^T) = det(A^T - \lambda I)$가 되기 때문에 eigen value는 동일합니다.   
② A가 orthogonal matrix라면, $\lambda_i = \pm 1$     
&rarr; orthognoal matrix를 $Q$라고 표기하겠습니다.    

\begin{aligned}    
Q\underline{v} =& \lambda \underline{v} \newline   
(Q\underline{v})^T Q \underline{v} =& \underline{v^T} Q^T Q \underline{v} \newline   
=& \underline{v^T} I \underline{v} =||v||_2^2 = \lambda^2 ||v||_2^2   
\end{aligned}   

orthogonal matrix의 성질에 상기와 같이 전개됩니다. 따라서 $\lambda^2 = 1$이되니 $\lambda = \pm 1$이 됩니다.
③ A가 Positive Semi-definite(p.s.d)이면, $\lambda_i \ge 0$   
&rarr; positive semi-definition(p.s.d)에 대해 간략하게 설명하겠습니다.   
어떤 symmetric matrix 가 $\underline{z^T} A \underline{z} \ge 0 = ||\underline{z}||_2^2 \lambda \ge 0$을 만족합니다.($A\underline{z} = \lambda \underline{z}$)   
$\underline{z^T} A \underline{z}$이 뜻은 이전까지 배웠던 내용으로 해석해보겠습니다. $\underline{z}$를 A 행렬을 통과시킨 선형변환을 하게됩니다. 그럼 $A\underline{z}$가 나옵니다. 이 선형변환을 한 $A\underline{z}$와 원본 자신인 $\underline{z}$와 내적을 했다는 뜻이 됩니다. 근데 $\underline{z^T} A \underline{z} \ge 0$이어야 하니, $A\underline{z}$ vector는 $\underline{z}$와 직교하는 평면 뒤쪽으로는 존재 하지않는 다는 뜻이 됩니다. 즉, 내적 했을 때 90도를 넘어가지 않는다는 뜻이됩니다. 하기에 그림에서와 같이 $A\underline{z}$ vector는 파란색 영역에 존재하게 된다는 뜻입니다.   
<img src="../../../assets/images/LinearAlgebra/2023-12-28-eigen vector and eigen value2/eigen decomposition 1.jpg" alt="eigen decomposition 1" style="zoom:80%;" />    
④ <span style='color:red'>**Diagonalizable한 matrix의 non-zero eigen value의 수는 rank와 동일**</span>합니다.    
&rarr; Diagonalizable하다고 했으니, $A = V \Lambda V^{-1}$로 표현할 수 있습니다. 그럼 $rank(A) = rank(V \Lambda V^{-1})$이 되며, rank는 **작은 rank를 따라갑니다.** $V$ 와 $V^{-1}$는 eigen vector을 모아 놓은 것이니 서로 independent합니다. 따라서 **Full-rank**가 됩니다. 그럼 $rank(V \Lambda V^{-1}) = rank(\Lambda)$를 따르게 됩니다. 따라서, eigen value의 수와 rank가 동일해집니다.    

### Symmetric Matrix
가장 중요한 성질에 대해 다뤄보겠습니다.   
<span style='color:red'>**Symmetric Matrix는 Diagnolizable**</span>합니다.($A^T = A$)   
\begin{aligned}    
A = V \Lambda V^{-1} \newline
\end{aligned}   
Symmetric Matrix라면 상기와 같이 나타낼 수 있으며, 이전에 설명했던 성질들을 이용할 수 있습니다. 즉, A의 rank만 알면, non-zero eigen value의 수를 알 수 있습니다.   

\begin{aligned}    
A =& V \Lambda V^{-1} \newline   
A = A^T =& V^{-T} \Lambda^T V^T = V^{-T} \Lambda V^T \newline   
V =& V^{-T} \newline   
V^{-1} =& V^T
\end{aligned}   
A가 Symmetric Matrix라면 상기와 같이 나타낼 수 있습니다.   

\begin{aligned}    
V =& V^{-T} \newline   
V^{-1} =& V^T
\end{aligned}   
그럼 상기와 같다는 것을 알아낼 수 있습니다. 이 뜻은, $V$가 **Orthogonal Matrix**라는 의미가 됩니다. Orthogonal Matrix의 성질인 $Q^TQ = I$에서 $Q^T = Q^{-1}$인 것과 같게 됩니다.   
<span style='color:red'>**Symmetric Matrix는 Diagnolizable하면 $A=Q \Lambda Q^T$**</span>가 됩니다. 하기에 이 정의를 전개해보겠습니다.   

\begin{aligned}    
A =& Q \Lambda Q^T(\underline{q_1} \bot \underline{q_2} \bot \underline{q_3}) \newline   
=& \begin{bmatrix} \underline{q_1} & \underline{q_2} & \underline{q_3} \end{bmatrix} \begin{bmatrix} \lambda_1 &  &  \newline & \lambda_2 & \newline & & \lambda_3 \end{bmatrix} \begin{bmatrix} \underline{q_1^T} \newline \underline{q_2^T} \newline \underline{q_3^T} \end{bmatrix} \newline   
=& \begin{bmatrix} \lambda_1 \underline{q_1} & \lambda_2 \underline{q_2} & \lambda_3 \underline{q_3} \end{bmatrix} \begin{bmatrix} \underline{q_1^T} \newline \underline{q_2^T} \newline \underline{q_3^T} \end{bmatrix} \newline   
=& \lambda_1 \underline{q_1} \underline{q_1^T} + \lambda_2 \underline{q_2} \underline{q_2^T} + \lambda_3 \underline{q_3} \underline{q_3^T} \newline   
\end{aligned}   
A를 rank-1 matrix들로 분해를 할 수 있다는 뜻입니다.    
<img src="../../../assets/images/LinearAlgebra/2023-12-28-eigen vector and eigen value2/eigen decomposition 2.jpg" alt="eigen decomposition 2" style="zoom:80%;" />    
상기의 이미지와 같이 $\underline{q_1} \underline{q_1^T}$를 $\lambda_1$만큼 확대시키고, $\underline{q_2} \underline{q_2^T}$를 $\lambda_2$만큼 확대시키고, $\underline{q_3} \underline{q_3^T}$를 $\lambda_3$만큼 확대시켜서 합친 것이 A가 된다는 뜻입니다. 여기서 하나 알 수 있는 점이 **$\lambda$가 크다는 것은 A를 decomposition했을 때, 잘 나타내는 행렬**이라는 뜻이 됩니다. 즉, 데이터 압축시 A인 원본을 제일 잘 나타낼 수 있는 것들만 남기고 나머지는 날리게되면 데이터 압축이 됩니다.  

### $A\underline{x}$
$A=A^T$이면서 A는 3 x 3이고 $\underline{q_1} \bot \underline{q_2} \bot \underline{q_3}$, $A = \lambda_1 \underline{q_1} \underline{q_1^T} + \lambda_2 \underline{q_2} \underline{q_2^T} + \lambda_3 \underline{q_3} \underline{q_3^T}$로 나타낼 수 있습니다.   
이때 $\underline{x}$는 eigen vector가 아닐 시 $A\underline{x}$는 하기와 같이 나타내집니다.   
\begin{aligned}   
A = \lambda_1 \underline{q_1} \underline{q_1^T} \underline{x} + \lambda_2 \underline{q_2} \underline{q_2^T} \underline{x} + \lambda_3 \underline{q_3} \underline{q_3^T} \underline{x} \newline
\end{aligned}   

여기서 $\underline{q_1^T} \underline{x}$은 $\underline{q_1}$과 $\underline{x}$의 내적이 됩니다. 그럼 앞에 있는 $\underline{q_1} \underline{q_1^T}$ 또한 어디서 많이 봤던 수식인 것 같습니다. 예전 포스팅에서 최소자승법을 배울 때, $P_A$인 Projection Matrix를 기억하실 수 있을 겁니다. $P_A = A(A^T A)^{-1} A^T$였습니다. 그럼 q에 대한 것은 $P_q = \underline{q}(\underline{q^T} \underline{q})^{-1} underline{q^T}$가 됩니다. 근데 여기서 $\underline{q}$는 orthonormal한 vector들의 모음이니 $\underline{q^T}\underline{q} = 1$이 됩니다. 따라서 $P_q = \underline{q} \underline{q^T}$가 되며 상기의 수식와 같아집니다. 그럼 $\underline{q} \underline{q^T} \underline{x}$는 <span style='color:blue'>**q vecotr를 span한 q column space에 x를 정사영 시킨 vector**</span>가 됩니다.    
그럼 여기서 $\underline{x}$를 각각 $\underline{q_1}, \underline{q_2}, \underline{q_3}$의 column space에 내적을 내렸다고 볼 수 있습니다. 즉, 하기와 같이 나타냈다고 생각할 수 있습니다.    
<img src="../../../assets/images/LinearAlgebra/2023-12-28-eigen vector and eigen value2/eigen decomposition 3.jpg" alt="eigen decomposition 3" style="zoom:80%;" />    
상기와 같이 $\underline{x}$를 각 $\underline{q_1}, \underline{q_2}, \underline{q_3}$에 정사영을 내리고 내적의 크기만큼 곱해준 vector들로 분해할 수 있습니다. 여기서 만약 $\lambda$들이 모두 1이라면, 그대로 $A\underline{x}=\underline{x}$를 구할 수 있습니다. 그 이유는 $A=Q \Lambda Q^T$인데, $\Lambda$의 value가 모두 1이라는 뜻이 됩니다. 그럼 $\Lambda = I$가 되어 $A = Q \Lambda Q^t = Q I Q^T = Q Q^T= I$를 통해 $A\underline{x} = I \underline{x} = \underline{x}$를 얻을 수 있습니다.    
즉, $\lambda$가 모두 1이 아니라면, 각각 $\underline{q_1} \underline{q_1^T} \underline{x}, \underline{q_2} \underline{q_2^T} \underline{x}, \underline{q_3} \underline{q_3^T} \underline{x}$들에 $\lambda_1, \lambda_2, \lambda_3$만큼 뻥튀기를 해주어서 재조합을 했다고할 수 있습니다.   
정리해보겠습니다.   
$\underline{x}$를 그냥 $q_1, q_2, q_3$에 각각 내리고 더해주면 그대로 $\underline{x}$가 나오지만, $q_1, q_2, q_3$에 내린 후 $\lambda_1, \lambda_2, \lambda_3$만큼 크기를 바꿔주어 재조합한 것이 $A\underline{x}$이 됩니다. 더 쉽게 말하자면, <span style='color:red'>**$\underline{x}$를 선형변환하는데, Symmetric한 Matrix가 가지고 있는 orthognoal한 eigen vector들로 $\underline{x}$를 내린 후, 각각의 eigen value만큼 조절하여 재조합한 행위**</span>라고 해석할 수 있습니다.