---
title:  "촐레스키 분해(Cholesky Decomposition)"
categories: LinearAlgebra
tag: [theory, math]
tags: [Jekyll, MathJax]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: 촐레스키 분해
comments: true
date: 2024-1-12
toc_sticky: true
---

## 촐레스키 분해(Cholesky Decomposition)
Cholesky Decomposition은 Decomposition하기 위해서 **Positive Semi-Definite(p.s.d)**을 만족해야하지 Decomposition을 수행할 수 있습니다.   
**Positive Semi-Definite(p.s.d)**은 추후에 한개의 주제로 공부를 해보겠습니다. 이전 eigen value 포스팅에서 간단하게 다뤘었습니다. 다시 상기를 해보자면, <span style='color:blue'>**어떤 Symmetric Matrix $A$가 $\underline{z}^T A \underline{z} > 0$을 만족**</span>을 해야합니다. 선형 변환한 $A\underline{z}$와 자기 자신인 $\underline{z}$간의 내적값이 양수가 나와야한다는 뜻입니다. 그 뜻은 선형 변환한 $A\underline{z}$와 $\underline{z}$간의 각도가 90도를 넘어가지 않아야한다는 뜻이됩니다.    
   
그럼 이어서 Cholesky Decomposition으로 넘어와서 **p.s.d를 만족**한다는 뜻이니 Symmetric Matrix라는 말이 됩니다. 그럼 Symmetric Matrix인 3 x 3짜리 $A$의 예시를 통해 설명하겠습니다.   
\begin{aligned}    
A =& \begin{bmatrix} a_{11} & a_{21} & a_{31} \newline a_{21} & a_{22} & a_{32} \newline a_{31} & a_{32} & a_{33} \end{bmatrix} \newline   
=& L L^T \newline   
=& \begin{bmatrix} L_{11} & 0 & 0 \newline L_{21} & L_{22} & 0 \newline L_{31} & L_{32} & L_{33} \end{bmatrix} \begin{bmatrix} L_{11} & L_{21} & L_{31}  \newline 0 & L_{22} & L_{32} \newline 0 & 0 & L_{33} \end{bmatrix} \newline   
=& \begin{bmatrix} L_{11}^2 &  &  \newline L_{21}L_{11} & L_{21}^2 + L_{22}^2 &  \newline L_{31}L_{11} & L_{31}L_{21} + L_{32}L_{22} & L_{31}^2 + L_{32}^2 + L_{33}^2 \end{bmatrix}
\end{aligned}    

상기와 같이 $A=L L^T$인 Low Triangle Matrix와 그의 Tranpose로 분해할 수 있습니다. $L$이 Symmetric하니 $L L^T$ 또한 Symmetric 하게 됩니다. 
그럼 이제 각각의 $L_{ij}$인 element들을 구해보겠습니다.   
\begin{aligned}    
L_{11} =& \sqrt{a_{11}} \newline   
L_{21} =& a_{21} / L_{11} \newline   
L_{31} =& a_{31} / L_{11} \newline   
L_{22} =& \sqrt{a_{22} - L_{21}^2} \newline   
L_{32} =& (a_{32} - L_{31}L_{21}) / L_{22} \newline   
L_{33} =& \sqrt{a_{33} - L_{31}^2 - L_{32}^2} \newline   
\end{aligned}   

그럼 $L_{ii}$와 $L_{ij}$를 일반화하여 표기해보겠습니다.   
\begin{aligned}    
L_{ii} =& \sqrt{a_{ii} - \sum_{k=1}^{i-1} L_{ik}^2} \newline   
L_{ij} =& \frac{1}{L_{jj}} (a_{ij} - \sum_{k=1}^{j-1} L_{ik} L_{jk})
\end{aligned}   

### $LDL^T$ 분해
이전 LU Decomposition 포스팅에서 LDU Decomposition을 기억하실 겁니다. LU에서 U의 Diagonal Element값들을 1로 표현하기 위해서 한개의 Diagonal Matrix를 추가해 분해한 것이었습니다. $LL^T$ 또한 똑같이 Diagonal Matrix를 추가한 $LDL^T$꼴로 Decomposition을 할 수 있습니다. 하기에 행렬 $A$를 Cholesky Decomposition을 수행한 것에 대해 $LDL^T$ Decomposition을 나타내보겠습니다.   
\begin{aligned}    
A =& \begin{bmatrix} 1 & -1 & 2 \newline -1 & 5 & -4 \newline 2 & -4 & 6 \end{bmatrix} \newline   
=& L L^T \newline   
=& \begin{bmatrix} 1 & 0 & 0 \newline -1 & 2 & 0 \newline 2 & -1 & 1 \end{bmatrix} \begin{bmatrix} 1 & -1 & 2 \newline 0 & 2 & -1 \newline 0 & 0 & 1 \end{bmatrix} \newline   
=& L D L^T \newline   
=& \begin{bmatrix} 1 & 0 & 0 \newline -1 & 1 & 0 \newline 2 & -\frac{1}{2} & 1 \end{bmatrix} \begin{bmatrix} 1 & 0 & 0 \newline 0 & 4 & 0 \newline 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} 1 & -1 & 2 \newline 0 & 1 & -\frac{1}{2} \newline 0 & 0 & 1 \end{bmatrix}
\end{aligned}    

### 응용
어떤 $\underline{n}$인 noise vector가 존재한다고 해보겠습니다. 