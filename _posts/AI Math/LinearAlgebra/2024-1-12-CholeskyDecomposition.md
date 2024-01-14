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
\begin{aligned}    
\underline{n} \; \sim& \; N\(\underline{0}, I\) \newline   
\begin{bmatrix} n_1 \newline n_2 \end{bmatrix} \; \sim& \; N\(\begin{bmatrix} E\[n_1\]=0 \newline E\[n_2\]=0 \end{bmatrix}, E\[\begin{bmatrix} x_1 - \mu_1 \newline x_2 - \mu_2 \end{bmatrix} \begin{bmatrix} x_1 - \mu_1 & x_2 - \mu_2 \end{bmatrix}\]\) \newline   
Cov(x_1, x_2)=& \begin{bmatrix} E\[(x_1 - \mu_1)^2\] & E\[(x_1 - \mu_1)(x_2 - \mu_2)\] \newline E\[(x_2 - \mu_2)(x_1 - \mu_1)\] & E\[(x_2 - \mu_2)^2\] \end{bmatrix}
\end{aligned}   

$I$는 noise vector들의 Covariance Matrix가 되며, 이 **Covariance Matrix는 Symmetric Matrix이며, p.s.d를 만족**하게 됩니다. p.s.d를 왜 만족하는지 확인을 해보겠습니다.   
\begin{aligned}    
p.s.d =& \underline{z}^T E\[(\underline{X} - \underline{\mu})(\underline{X} - \underline{\mu})^T\] \underline{z} \newline   
=& E\[(\underline{z}^T \underline{X} - \underline{\mu})(\underline{X} - \underline{\mu})^T \underline{z} \] \newline   
=& E\[(\underline{z}^T (\underline{X} - \underline{\mu}))^2 \] \ge 0 
\end{aligned}   

상기와 같이 Scalar연산으로 바뀌고 제곱이 되니 Scalar의 Expectation은 Scalar가 그대로 나오니 다음과 같이 항상 양수가 되니 p.s.d를 만족합니다. 따라서 p.s.d가 성립하니 **Covariance Matrix는 Cholesky Decomposition이 가능**하게 됩니다. 

이제 어떤 경우에 활용적으로 쓰이는지 알아보겠습니다. 만약, 어떤 noise vector $\underline{n}$이 $N \; \sim \; (\underline{0}, I)$인 표준 정규 분포를 따른다고 해보겠습니다. 그럼 $E\[\underline{n}\]=\underline{0}, \; E\[\underline{n}\underline{n}^T\] = I$인 Expectation과 Covariance Matrix를 나타낼 수 있습니다. 이 때 새로운 noise vector인 $\underline{n_{new}}$는 $\underline{n_{new}} \; \sim \; N(\underline{0}, R)$을 따른다고 해보겠습니다. 그럼 Covariance Matrix인 $R$은 Cholesky Decomposition이 가능하니 $R=LL^T$이 됩니다.   
이 때 <span style='color:blue'>**$\underline{n_{new}} = L\underline{n}$**</span>이 됩니다. 그럼 $E\[\underline{n_{new}}\]=E\[L\underline{n}\] = LE\[\underline{n}\] = \underline{0}$이 됩니다. 그럼 Covariance Matrix는 $E\[L\underline{n} \underline{n}^T L^T\] = L E\[\underline{n} \underline{n}^T\] L^T = L L^T$을 구할 수 있습니다.   

최종 정리를 해보겠습니다. <span style='color:red'>**따르고 싶게끔하는 Covariance Matrix를 Cholesky Decomposition을 하고 그거를 따르게 하고 싶은 새로운 데이터에 곱해주면, 그 데이터가 내가 따르게 하고 싶은 Covariance Matrix를 따르는 데이터를 만들어줄 수 있습니다.**</span>