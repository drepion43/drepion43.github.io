---
title:  "QR 분해(QR Decomposition)"
categories: LinearAlgebra
tag: [theory, math]
tags: [Jekyll, MathJax]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: QR 분해
comments: true
date: 2024-1-12
toc_sticky: true
---

## QR Decomposition
이전 포스팅에서 그람-슈미트 직교화에 대해 다뤘습니다. 그람-슈미트는 간단하게 말해 임의 independent한 vector $\begin{bmatrix} \underline{a_1} & \underline{a_2} & \underline{a_3} \end{bmatrix}$를 가지고, Orthonormal set인 $\begin{bmatrix} \underline{q_1} & \underline{q_2} & \underline{q_3} \end{bmatrix}$를 구하는 직교화 방법이었습니다. 하기에 간략하게 설명하는 이미지를 참고하시면 조금 더 도움이 될 것 입니다.   
<img src="../../../assets/images/LinearAlgebra/2024-1-12-QRDecomposition/QR Decomposition 1.jpg" alt="QR Decomposition 1" style="zoom:80%;" />    
그럼 역으로 $\underline{q}$를 이용하여 $\underline{a}$들을 정의해보겠습니다.   
\begin{aligned}    
\underline{a_1} =& (\underline{a_1}^T \underline{q_1}) \underline{q_1} \newline   
\underline{a_2} =& (\underline{a_2}^T \underline{q_1}) \underline{q_1} + (\underline{a_2}^T \underline{q_2}) \underline{q_2} \newline   
\underline{a_3} =& (\underline{a_3}^T \underline{q_1}) \underline{q_1} + (\underline{a_3}^T \underline{q_2}) \underline{q_2} + (\underline{a_3}^T \underline{q_3}) \underline{q_3} \newline   
\end{aligned}    

$\underline{a_3}$에 대해서만 간략하게 추가 설명을 하겠습니다. $\underline{a_3}$의 경우는 $(\underline{a_3}^T \underline{q_1})$을 통해 $\underline{q_1}$에 내린 $\underline{a_3}$의 크기를 얻고 그 후 방향을 나타내는 unit vector $\underline{q_1}$을 곱해주면 $\underline{a_3}$가 $\underline{q_1}$에 가지고 있는 성분을 나타낼 수 있습니다. 이렇게 $\underline{q_2}, \underline{q_3}$에서도 $\underline{a_3}$의 성분을 끄집어내 더해주면 $\underline{a_3}$를 나타낼 수 있습니다.    

### 과정
그럼 이제 QR Decomposition에 대해 알아보겠습니다. QR Decomposition을 하기 위해서는 우선 Gram-Schmidt Orthognolization을 통해 $\underline{q}$들인 **Orthognoal Matrix**를 얻은 후 원본 행렬인 $A$를 $\underline{q}$들을 통해 Decomposition을 하는 것을 뜻합니다. 여기서 $R$은 Upper Triangle Matrix형태로 나옵니다. 그럼 수식을 통해 살펴보겠습니다. 수식의 정의는 매우 간단하게 하기와 같습니다.   
\begin{aligned}    
A =& QR \newline   
\begin{bmatrix} \underline{a_1} & \underline{a_2} & \underline{a_3} \end{bmatrix} =& \begin{bmatrix} \underline{q_1} & \underline{q_2} & \underline{q_3} \end{bmatrix} \begin{bmatrix} \underline{a_1}^T \underline{q_1} & \underline{a_2}^T \underline{q_1} & \underline{a_3}^T \underline{q_1} \newline 0 & \underline{a_2}^T \underline{q_2} & \underline{a_3}^T \underline{q_2} \newline 0 & 0 & \underline{a_3}^T \underline{q_3} \end{bmatrix}
\end{aligned}    

현재까지는 Square Matrix이며 Full-Rank에 대해 QR Decomposition을 수행했습니다. 그럼 이번엔 Full-Column Rank인 행렬 5 x 3짜리 행렬 $A$에 대해 QR Decomposition을 간단하게 해보겠습니다. 그럼 $rank(A)=3$인 행렬 $A=\begin{bmatrix} \underline{a_1} & \underline{a_2} & \underline{a_3} \end{bmatrix}$에 대해 Decomposition을 해보겠습니다.   
\begin{aligned}    
\begin{bmatrix} \underline{a_1} & \underline{a_2} & \underline{a_3} \end{bmatrix} =& \begin{bmatrix} \underline{q_1} & \underline{q_2} & \underline{q_3} & \underline{q_4} & \underline{q_5} \end{bmatrix} \; R \newline   
\end{aligned}    

여기서 $Q$가 Square Matrix이니 Square Matrix가 되기 위해서는 5 x 5 가 되어야 합니다. 그럼 이전까지 배웠던 그람-슈미트를 통해 $\begin{bmatrix} \underline{q_1} & \underline{q_2} & \underline{q_3} \end{bmatrix}$을 얻을 수 있습니다. 그럼 2개 차원이 비는데, 2개 차원은 $A$는 3개 차원을 span하는데, 나머지 2개 차원이 비게됩니다. 이 <span style='color:blue'>**빈 2개차원을 span하면서 $\underline{q_1}, \underline{q_2}, \underline{q_3}$과도 수직한 직교하는 Basis vector인 $\underline{q_4}, \underline{q_5}$를 찾아서 채워 넣으면 됩니다.**</span> 그럼 이어서 행렬 $R$은 자연스럽게 5 x 3이 되게 되는데 $R$은 어떤 것을 채워놓는지 확인해보겠습니다.   
\begin{aligned}    
R =& \begin{bmatrix} \underline{a_1}^T \underline{q_1} & \underline{a_2}^T \underline{q_1} & \underline{a_3}^T \underline{q_1} \newline 0 & \underline{a_2}^T \underline{q_2} & \underline{a_3}^T \underline{q_2} \newline 0 & 0 & \underline{a_3}^T \underline{q_3} \newline 0 & 0 & 0 \newline 0 & 0 & 0 \end{bmatrix} \newline   
\end{aligned}   

상기와 같이 $R$의 나머지 부분은 모두 0 element를 가진채로 5 x 3의 꼴로 나타내집니다. 그럼 하기에 최종적으로 QR의 꼴을 나타내보겠습니다.   
\begin{aligned}    
A =& QR \newline   
=& \begin{bmatrix} \underline{q_1} & \underline{q_2} & \underline{q_3} & \underline{q_4} & \underline{q_5} \end{bmatrix} \begin{bmatrix} \underline{a_1}^T \underline{q_1} & \underline{a_2}^T \underline{q_1} & \underline{a_3}^T \underline{q_1} \newline 0 & \underline{a_2}^T \underline{q_2} & \underline{a_3}^T \underline{q_2} \newline 0 & 0 & \underline{a_3}^T \underline{q_3} \newline 0 & 0 & 0 \newline 0 & 0 & 0 \end{bmatrix}
\end{aligned}   

이제 Square Matrix가 아닌 형태에서도 QR Decomposition을 수행할 수 있습니다.
### 응용
그럼 이전 설명과 동일한 5 x 3 짜리 행렬 $A$를 QR Decomposition을 해보겠습니다. 여기서 채워놓은 $\underline{q_4}, \underline{q_5}$는 $Q_2$라고 표기하고 그람-슈미트를 통해 얻은 것은 $Q_1$이라고 정의하겠습니다.   
\begin{aligned}    
A =& QR \newline   
=& \begin{bmatrix} Q_1 & Q_2 \end{bmatrix} \begin{bmatrix} R \newline 0 \end{bmatrix} = Q_1R 
\end{aligned}    

이전 포스팅인 $A\underline{x} \approx b$에서 $\underline{x}$를 찾는 Least Squares를 기억하실 겁니다. 하기의 이미지에서 $\underline{b} - A \underline{\hat{x}}$의 크기를 최소화를 해주는 $\underline{\hat{x}}$를 찾는 것이 Least Sqaures 문제였습니다.    
<img src="../../../assets/images/LinearAlgebra/2024-1-12-QRDecomposition/QR Decomposition 2.jpg" alt="QR Decomposition 2" style="zoom:80%;" />    
그럼 이 오차의 크기는 2-norm으로 표현하면 $|| \underline{b} - A \underline{\hat{x}} ||_2^2$이 됩니다. 그럼 $A=QR$이니 대입을 통해 수식을 전개해보겠습니다.   
\begin{aligned}    
|| \underline{b} - A \underline{\hat{x}} ||_2^2 =& || \underline{b} - QR \underline{\hat{x}} ||_2^2
\end{aligned}    

$Q$는 Orthogonal Matrix이니 Orthogonal Matrix의 성질인 $Q Q^T =I$의 성질을 이용하면 하기와 같이 나타낼 수 있습니다.   
\begin{aligned}    
|| \underline{b} - QR \underline{\hat{x}} ||_2^2 =& || Q (Q^T \underline{b} - R \underline{\hat{x}}) ||_2^2 \newline   
\& || Q^T \underline{b} - R \underline{\hat{x}} ||_2^2
\end{aligned}   

상기의 수식에서 $Q$를 밖으로 빼줬는데, 이 $Q$가 사라졌습니다. 2-norm의 정의는 $|| A ||_2^2 = A^T A$와 같습니다. 정의를 이용하여 수식을 통해 그 이유를 살펴보겠습니다.   
\begin{aligned}    
(Q (Q^T \underline{b} - R \underline{\hat{x}}) )^T (Q (Q^T \underline{b} - R \underline{\hat{x}})) =& (Q^T \underline{b} - R \underline{\hat{x}})^T Q^T Q (Q^T \underline{b} - R \underline{\hat{x}}) \newline   
=& (Q^T \underline{b} - R \underline{\hat{x}})^T (Q^T \underline{b} - R \underline{\hat{x}}) \newline   
=& || Q^T \underline{b} - R \underline{\hat{x}} ||_2^2
\end{aligned}   

Rectangular Matrix에서의 $Q$는 $Q_1, Q_2$와 $R$은 $R, \underline{0}$으로 쪼개진다고 했었습니다. 그럼 하기와 같이 표현될 수 있습니다.   
\begin{aligned}    
|| Q^T \underline{b} - R \underline{\hat{x}} ||_2^2 =& || \begin{bmatrix} Q_1^T \newline Q_2^T \end{bmatrix} \underline{b} - \begin{bmatrix} R & 0 \end{bmatrix} \underline{\hat{x}} ||_2^2 \newline   
=& || \begin{bmatrix} Q_1^T \underline{b} - R \underline{\hat{x}} \newline  Q_2^T \underline{b} \end{bmatrix} ||_2^2 \newline   
=& || Q_1^T \underline{b} - R \underline{\hat{x}} ||_2^2 + || Q_2^T \underline{b} ||_2^2
\end{aligned}   

여기서 $R$은 그람-슈미트를 통해 구한 Upper Triangle Matrix입니다. 그럼 <span style='color:blue'>**Upper Triangle Matrix의 Property 중 Determinant는 Diagnoal element의 곱들이 됩니다. 근데 그람-슈미트를 통해 구한 $R$은 Diagnoal element들 중에 0이 존재하지 않게 됩니다. 그럼 Invertible하다는 뜻**</span>이 되므로 $R^{-1}$을 취해줄 수 있습니다.    
Least Squares는 최종적으로 구한 $|| Q_1^T \underline{b} - R \underline{\hat{x}} ||_2^2 + || Q_2^T \underline{b} ||_2^2$에서 $\underline{\hat{x}}$을 통해 전체 값을 최소화 해주는 것이 목적입니다. 그럼 $Q_1^T \underline{b} - R \underline{\hat{x}}$을 넘겨주어 $\underline{\hat{x}}$를 구해주면 $\underline{\hat{x}} = R^{-1 }Q_1^T \underline{b}$을 얻을 수 있습니다. 그럼 구한 $\underline{\hat{x}}$을 대입해주면 $Q_1^T \underline{b} - R \underline{\hat{x}} = 0$을 얻을 수 있습니다.   
그럼 최종적으로 Least Squares의 식은 $|| Q_2^T \underline{b} ||_2^2$을 얻을 수 있습니다. 그럼 여기서 $|| Q_2^T \underline{b} ||_2^2$의 정의는 $\underline{\hat{x}}$을 아무리 움직여도 $\underline{b}$와 $C(A)$간의 오차가 존재하는데, 그 크기를 뜻하게 됩니다.   
<img src="../../../assets/images/LinearAlgebra/2024-1-12-QRDecomposition/QR Decomposition 3.jpg" alt="QR Decomposition 3" style="zoom:80%;" />    

그럼 이제 QR Decomposition을 통해 구한 $\underline{\hat{x}}$와 Least Sqaures 포스팅에서 구한 $\underline{\hat{x}}$과 동일한지 확인해보겠습니다.   
\begin{aligned}    
\underline{\hat{x}} =& R^{-1} Q_1^T \underline{b} \quad (QR \quad Decomposition) \newline   
\underline{\hat{x}} =& (A^T A)^{-1} A^T \underline{b} \quad (Least \quad Squares) \newline   
=& (R_1^T Q_1^T Q_1 R_1)^{-1} R_1^T Q_1^T \underline{b} \newline   
=& R_1^{-1} R_1^{-T} R_1^T Q_1^T \underline{b} \newline   
=& R_1^{-1} Q_1^T \underline{b}
\end{aligned}   



