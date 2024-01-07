---
title:  "LU Decomposition"
categories: LinearAlgebra
tag: [theory, math]
tags: [Jekyll, MathJax]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: LU, PLU, LDU Decomposition에 대한 설명
comments: true
date: 2024-1-6
toc_sticky: true
---

## LU(Lower Triangle Upper Triangle) Decomposition
이전 포스팅에서 Eigen Decompositon, SVD(Singular Value Decomposition)을 배웠습니다. 이런 Decomposition말고도 더 다양한 Decomposition이 존재하는데, 이번 포스팅에서는 LU Decomposition에 대해 알아보도록 하겠습니다. 우선 LU Decomposition는 행렬 $A$를 하기와 같이 Decomposition하는 것을 뜻합니다.   
\begin{aligned}    
A =& LU
\end{aligned}   

그림   
상기의 이미지에서 볼 수 있듯이, L은 Lower Triangle Matrix꼴이고, U는 Upper Triangle Matrix꼴로 $A$를 분해할 수 있습니다.   
Lower Triangle은 $\begin{bmatrix} 1 & 0 \newline 3 & 2 \end{bmatrix}$와 같이 아래 삼각형에만 값을 가지고 있는 꼴입니다. 또한, 대각 element에 $\begin{bmatrix} 1 & 0 \newline 3 & 0 \end{bmatrix}$과 같이 0을 가지고 있어도 Lower Triangle Matrix입니다.   
Upper Triangle은 $\begin{bmatrix} 1 & 4 \newline 0 & 2 \end{bmatrix}$와 같이 위쪽 역삼각형에만 값을 가지고 있는 꼴입니다. Lower와 동일하게 $\begin{bmatrix} 1 & 4 \newline 0 & 0 \end{bmatrix}$ 대각 element에 0의 값을 가지고 있어도 Upper Triangle Matrix입니다.   

그럼 이제 $A \underline{x} = \underline{b}$와 같은 연립 선형 방정식의 문제를 LU Decomposition을 통해 해결해보겠습니다.   
\begin{aligned}    
A \underline{x} =& \underline{b} \newline   
L U \underline{x} =& \underline{b}
\end{aligned}   

상기의 $A = LU$를 예시를 들어 설명해보겠습니다. 하기와 같은 연립일차 방정식을 풀어보겠습니다.   
\begin{aligned}    
\begin{Bmatrix}
2x + y - z = 8 \newline   
-3x -y +2z = -11 \newline   
-2x + y +2z = -3
\end{Bmatrix}   
\end{aligned}    

상기의 연립일차방정식은 이전에 **가우스 조던 소거법**을 이용하여 풀었습니다. LU분해는 행렬 $A$에 대해 분해하는 것이니 우선 상기의 연립일차방정식을 행렬로 표현해보겠습니다.   
\begin{aligned}    
A \underline{x} =& \underline{b} \newline   
\begin{bmatrix} 2 & 1 & -1  \newline -3 & -1 & 2 \newline -2 & 1 & 2 \end{bmatrix} \begin{bmatrix} x  \newline -y \newline z \end{bmatrix} =& \begin{bmatrix} 8  \newline -11 \newline -3 \end{bmatrix} \newline   
A =& \begin{bmatrix} 2 & 1 & -1  \newline -3 & -1 & 2 \newline -2 & 1 & 2 \end{bmatrix}
\end{aligned}    

상기와 같이 연립일차방정식을 행렬로 표현할 수 있습니다. 그럼 이전에 가우스 조던 소거법에서는 행렬 $A$에 어떤 값들을 계속 곱해줘서 **Upper Triangle Matrix의 결과**를 얻을 수 있었습니다. 즉, $KA = U$의 꼴이 나타났습니다. 그럼 이 Upper Triangle Matrix로 만들어 주기 위해 $K$가 어떤 것들인지 살펴보겠습니다.   
\begin{aligned}    
A =& \begin{bmatrix} \underline{a_1^T} \newline \underline{a_2^T} \newline \underline{a_3^T} \end{bmatrix} =\begin{bmatrix} 2 & 1 & -1  \newline -3 & -1 & 2 \newline -2 & 1 & 2 \end{bmatrix}  \newline   
L_1 A =& \begin{bmatrix} 1 & 0 & 0  \newline \frac{3}{2} & 1 & 0 \newline 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} \underline{a_1^T} \newline \underline{a_2^T} \newline \underline{a_3^T} \end{bmatrix} = \begin{bmatrix} \underline{a_1^T} \newline \frac{3}{2}\underline{a_1^T} + \underline{a_2^T} \newline \underline{a_3^T} \end{bmatrix} \newline   
=&\begin{bmatrix} 1 & 0 & 0  \newline \frac{3}{2} & 1 & 0 \newline 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} 2 & 1 & -1  \newline -3 & -1 & 2 \newline -2 & 1 & 2 \end{bmatrix} = \begin{bmatrix} 2 & 1 & -1  \newline 0 & \frac{1}{2} & \frac{1}{2} \newline -2 & 1 & 2 \end{bmatrix} \newline   
L_1 A =& \begin{bmatrix} \underline{a_1^T} \newline \underline{a_2^T} \newline \underline{a_3^T} \end{bmatrix} \newline   
L_2 L_1 A =& \begin{bmatrix} 1 & 0 & 0  \newline 0 & 1 & 0 \newline 1 & 0 & 1 \end{bmatrix} \begin{bmatrix} \underline{a_1^T} \newline \underline{a_2^T} \newline \underline{a_3^T} \end{bmatrix} = \begin{bmatrix} \underline{a_1^T} \newline \underline{a_2^T} \newline \underline{a_1^T} + \underline{a_3^T} \end{bmatrix} \newline   
=& \begin{bmatrix} 2 & 1 & -1  \newline 0 & \frac{1}{2} & \frac{1}{2} \newline 0 & 2 & 1 \end{bmatrix} \newline   
L_2 L_1 A =& \begin{bmatrix} \underline{a_1^T} \newline \underline{a_2^T} \newline \underline{a_3^T} \end{bmatrix} \newline   
L_3 L_2 L_1 A =& \begin{bmatrix} 1 & 0 & 0  \newline 0 & 1 & 0 \newline 0 & -4 & 1 \end{bmatrix} \begin{bmatrix} \underline{a_1^T} \newline \underline{a_2^T} \newline \underline{a_3^T} \end{bmatrix} \newline   
=& \begin{bmatrix} 2 & 1 & -1  \newline 0 & \frac{1}{2} & \frac{1}{2} \newline 0 & 0 & -1 \end{bmatrix}
\end{aligned}    

상기와 같이 최종적으로 $L_3 L_2 L_1 A = \begin{bmatrix} 2 & 1 & -1  \newline 0 & \frac{1}{2} & \frac{1}{2} \newline 0 & 0 & -1 \end{bmatrix}$인 Upper Triangle Matrix 형태로 표현되었습니다. 그럼 $A=LU$꼴로 나타내기주 위해선 역함수를 곱해주면 됩니다. 그럼 우선 $L_3, L_2, L_1$이 **Invertible**한지부터 확인해야합니다. 그럼 이전 Determinant 포스팅에서 Lower이나 Upper Triangle Matrix의 determinant는 **Diagonal Element들의 곱**이라고 했었던 것을 기억하실겁니다. 따라서 $det(L_3) = 1, det(L_2) = 1, det(L_1) = 1$로 모두 **Invertible**합니다.   
\begin{aligned}    
L_3 L_2 L_1 A =& LA = U \newline   
A =& L_1^{-1} L_2^{-1} L_3^{-1} U \newline   
=& L U \newline   
=& \begin{bmatrix} 1 & 0 & 0  \newline - \frac{3}{2} & 1 & 0 \newline -1 & 4 & 1 \end{bmatrix} \begin{bmatrix} 2 & 1 & -1  \newline 0 & \frac{1}{2} & \frac{1}{2} \newline 0 & 0 & -1 \end{bmatrix}
\end{aligned}    

최종적으로 $A = LU$로 Decomposition을 완료했습니다.    
그럼 이제 이 LU Decomposition이 어디에 쓰이는지 알아보겠습니다.   
\begin{aligned}    
A \underline{x} =& \underline{b} \newline   
LU \underline{x} =& \underline{b} \newline   
① \; L \underline{y} =& \underline{b} \newline   
② \; U\underline{x} =& \underline{y}
\end{aligned}    

A를 Lower과 Upper로 분해를 한 후 ①인 $L \underline{y} = \underline{b}$는 쉽게 구할 수 있습니다. $L$과 $\underline{b}$를 알고있기 때문입니다. 그 후 ①에서 구한 $\underline{y}$를 이용하여 ②인 $U\underline{x} = \underline{y}$을 구해주면 우리가 원하는 $\underline{x}$를 쉽게 구할 수 있습니다. 한번 상기에서 구한 예시로 간단하게 ①, ②을 구해보겠습니다.   
\begin{aligned}    
L \underline{y} =& \underline{b} \newline   
\begin{bmatrix} 1 & 0 & 0  \newline - \frac{3}{2} & 1 & 0 \newline -1 & 4 & 1 \end{bmatrix} \begin{bmatrix} y_1 \newline y_2 \newline y_3 \end{bmatrix} =& \begin{bmatrix} 8  \newline -11 \newline -3 \end{bmatrix} \newline   
\end{aligned}    

상기를 통해 $\underline{y}$를 매우 쉽게 구할 수 있습니다. 그리고 구한 $\underline{y}$를 이용하여 상기와 비슷하게 $U\underline{x} = \underline{y}$을 이용하여 $\underline{x}$를 구하면 됩니다.    
## PLU Decomposition
항상 LU Decomposition이 되는 것은 아닙니다. 만약 하기와 같이 생긴 행렬 $A$의 경우에는 LU Decomposition이 불가능합니다.    
\begin{aligned}    
A =& \begin{bmatrix} 0 & 3 & 2  \newline 1 & 1 & 4 \newline 2 & 2 & 5 \end{bmatrix}
\end{aligned}    

상기의 수식은 LU Decomposition은 안되지만, 가우스 조던 소거법으로는 해결을 할 수 있습니다. 가우스 조던을 이용할 때, 행의 자리를 바꾸어 해결하면 됬었습니다. 이 자리 바꿈을 해주는 것이 PLU Decomposition입니다. PLU에서 P는 Permutation의 약어로 행렬 내부의 행의 자리를 바꿔주는 역할을 해줍니다. 하기에서 확인해보겠습니다.   
\begin{aligned}    
A =& \begin{bmatrix} \underline{a_1^T} \newline \underline{a_2^T} \newline \underline{a_3^T} \end{bmatrix} = \begin{bmatrix} 0 & 3 & 2  \newline 1 & 1 & 4 \newline 2 & 2 & 5 \end{bmatrix} \newline   
PA =& \begin{bmatrix} 0 & 1 & 0  \newline 1 & 0 & 0 \newline 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} \underline{a_1^T} \newline \underline{a_2^T} \newline \underline{a_3^T} \end{bmatrix} \newline   
=& \begin{bmatrix} \underline{a_2^T} \newline \underline{a_1^T} \newline \underline{a_3^T} \end{bmatrix}   
=& \begin{bmatrix} 1 & 1 & 4  \newline 0 & 3 & 2 \newline 2 & 2 & 5 \end{bmatrix} \newline   
PA =& LU
\end{aligned}    

상기와 같이 $P$를 곱해줌으로써 LU Decomposition을 가능하게 만들어줬습니다. 여기서 $P$인 Permutation Matrix는 자세히보면 **Orthogonal Matrix**입니다. 그럼 Orthognoal Matrix의 성질인 $Q^{-1} = Q^T$입니다. 따라서 $A = P^{-1}LU = P^T LU$를 통해 구할 수 있습니다. 

## LDU Decomposition
LDU Decomposition은 LU Decomposition의 결과에서 U의 Diagnoal Element값들이 모두 1이 아닌 상태일 때, **U의 Diagnaol Element들을 모두 1로 만들어주기 위해 Diagonal Matrix를 추가하여 U Matrix를 바꿔준 것**입니다. 상기 LU Decomposition에서 구했던 Lower Matrix와 Upper Matrix를 가지고 LDU를 만들어보겠습니다.   
\begin{aligned}    
A =& LU \newline   
=& \begin{bmatrix} 1 & 0 & 0  \newline - \frac{3}{2} & 1 & 0 \newline -1 & 4 & 1 \end{bmatrix} \begin{bmatrix} 2 & 1 & -1  \newline 0 & \frac{1}{2} & \frac{1}{2} \newline 0 & 0 & -1 \end{bmatrix} \newline   
=& L D U \newline   
=& \begin{bmatrix} 1 & 0 & 0  \newline - \frac{3}{2} & 1 & 0 \newline -1 & 4 & 1 \end{bmatrix} \begin{bmatrix} 2 & 0 & 0  \newline 0 & \frac{1}{2} & 0 \newline 0 & 0 & -1 \end{bmatrix} \begin{bmatrix} 1 & \frac{1}{2} & -\frac{1}{2}  \newline 0 & 1 & 1 \newline 0 & 0 & 1 \end{bmatrix}
\end{aligned}    

상기와 같이 Upper Triangle Matrix의 Diagonal Element들이 1로 모두 바뀌었습니다. 