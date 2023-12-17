---
title:  "최소 자승법(least squares)"
categories: LinearAlgebra
tag: [theory, math]
tags: [Jekyll, MathJax]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: 최소 자승법 계산
comments: true
date: 2023-12-12
toc_sticky: true
---

## 최소 자승법(Least Square Method)
최소 자승법의 정의에 대해 알아보겠습니다.   
> 최소제곱법, 또는 최소자승법, 최소제곱근사법, 최소자승근사법(method of least squares, least squares approximation)은 어떤 계의 해방정식을 근사적으로 구하는 방법으로, 근사적으로 구하려는 해와 실제 해의 오차의 제곱의 합(SS)이 최소가 되는 해를 구하는 방법입니다.   

최소 자승법은 LSM이라고 부르며, 흔히 자주 들었던 최소 제곱법, Ordinary Leas Squares(OLS)와 모두 같은 말입니다.    

## 선형대수학적 이해
<img src="../../../assets/images/LinearAlgebra/2023-12-12-least squares/least squares 1.jpg" alt="least squares 1" style="zoom:80%;" />    
상기의 그림과 같이 Full column rank인 A matrix가 존재한다고 하겠습니다. A의 rank는 3이며, column space가 3이라면 뜻입니다. 즉, A의 column space는 10차원 안에서 3차원으로 span하게 된다는 말입니다. 그럼 우측의 10차원에서 3차원으로 span한 A의 column space가 있다고 하겠습니다. 또한 $\underline{b}$는 A가 span하는 공간에 놓여있지 않은 10차원 vector라고 하겠습니다. 그럼 $C(A)$안에 $\underline{b}$는 존재하지 않게되며 A의 column space에서 $\underline{b}$를 나타낼 수 없습니다. 즉, A의 column space에 있는 vector인 $A\underline{x}$로 $\underline{b}$를 나타낼 수는 없게 됩니다. 그럼 여기서 **어떤 $\underline{x}$일 때, $\underline{b}$와 가장 근사한 값이 될 수 있을지를 구하는 방법이 최소 자승법**이 됩니다.   

<img src="../../../assets/images/LinearAlgebra/2023-12-12-least squares/least squares 2.jpg" alt="least squares 2" style="zoom:80%;" />    
상기의 이미지에서 ① vector와 ② vector 중 어떤 vector가 $\underline{b}$와 더 유사하다고 생각하시나요? 제 생각에는 ① vector가 더 유사하다고 생각합니다. 그럼 여기서 ①와 $\underline{b}$간의 유사한 정도를 알기 위해서는 역으로 두 벡터간의 차이(error)을 알면 해결할 수 있을 거라고 생각합니다. 두 vector간의 error는 두 vector간의 거리라고 생각을 해보겠습니다. 그럼 두 vector간의 거리를 구하려면 두 vector간의 차이를 나타내는 vector를 구한 후 그 vector의 크기가 곧 거리이며 error가 됩니다. 그럼 error vector를 구해보겠습니다. ① vector는 A의 column space안에 존재하는 vector이니 $A\underline{x}$라고 표기하겠습니다. 그럼 $A\underline{x} - \underline{b}$가 error가 되며 이 vector를 $\underline{e}$라고 부르겠습니다. 즉, $A\underline{x} - \underline{b} = \underline{e}$가 됩니다. 그럼 현재 ①, ② 두 개의 vector만을 표현했는데, A의 column space에는 무수히 많은 vector들이 존재합니다. 그럼 $\underline{b}$와 가장 유사한 vector를 찾기 위해서는 방금 구한 $\underline{e}$의 크기를 최소로 해주는 vector $\underline{x}$를 찾아내면 됩니다. 일단 **Least Sqaures에서는 크기를 2-norm으로 구하는 방법론**입니다. 따라서, $\|\|e\|\|_{2}^{2}$을 최소로 하는 $\underline{x}$를 찾아주면 됩니다. 근데 Least Sqaures가 아닌 가장 유사한 vector를 구하고 싶다면 이와 비슷한 개념을 가지고 유사한 vector를 찾아내면 됩니다. 

### 내적으로 해결
<img src="../../../assets/images/LinearAlgebra/2023-12-12-least squares/least squares 3.jpg" alt="least squares 3" style="zoom:80%;" />    
$\underline{e}$가 최소가 되려면, **$\underline{b}$와 $A\underline{x}$가 서로 수직**할 때 최소가 됩니다. 즉, **$\underline{b}$와 $A\underline{x}$의 내적값이 0이 되는 $\underline{x}$**을 찾으면 됩니다. 상기의 이미지에서 $\underline{b}$와 수직하도록 해주는 A의 column space의 vector를 $A\hat{\underline{x}}$라고 하겠습니다. 하기에 내적을 통해서 구해보겠습니다.    
\begin{aligned}   
(\underline{b} - A\hat{\underline{x}})^T A\hat{\underline{x}} =& \underline{0} \newline   
(\underline{b}^T A - \hat{\underline{x}}^T A^T A)\hat{\underline{x}} =& \underline{0} \newline   
\end{aligned}   
여기서 $\hat{\underline{x}} = \underline{0}$이라면 등식이 성립합니다. 수직해서 내적값이 0이된다가 아니라 $\underline{0}$와 내적하여 0을 얻게된다는 뜻이기 때문에, $\hat{\underline{x}} = \underline{0}$인 경우에 대해서는 고려하지 않아야합니다. 그럼 $\underline{b}^T A - \hat{\underline{x}}^T A^T A = \underline{0}$을 구해주면 됩니다.   

\begin{aligned}   
(\underline{b}^T A - \hat{\underline{x}}^T A^T A) =& \underline{0} \newline   
\underline{b}^T A =& \hat{\underline{x}}^T A^T A \newline   
A^T \underline{b} =& A^T A \hat{\underline{x}} \newline   
\end{aligned}    
$A^T \underline{b} = A^T A \hat{\underline{x}}$을 **정규 방정식(Normal Equation)**이라고 부릅니다. 또한, $A^T A$는 3 x 10과 10 x 3과의 곱으로 3 x 3이 됩니다. 즉, $rank(A^TA) = rank(A)$인 것을 알 수 있습니다. $A^T A$는 3 x 3으로 square matrix이며, rank는 3인 Full rank가 됩니다. 여기서 square matrix이며, <span style='color:red'>**Full rank라는 말은 곧 invertible**</span>하다 는 것을 알 수 있습니다. **invertible하니 앞에 inverse Matrix를 곱해주면 Identity Matrix**가 나옵니다.

\begin{aligned}   
A^T \underline{b} =& A^T A \hat{\underline{x}} \newline   
(A^T A)^{-1} A^T \underline{b} =& (A^T A)^{-1} A^T A \hat{\underline{x}} \newline   
\hat{\underline{x}} =& (A^T A)^{-1} A^T \underline{b}
\end{aligned}    

그럼 $\underline{e}$를 가장 최소로 해주는 $A\hat{\underline{x}}$는 하기와 같이 됩니다.   

\begin{aligned}   
A\hat{\underline{x}} =& A (A^T A)^{-1} A^T \underline{b} \newline
\end{aligned} 

여기서 알 수 있는 점이 **가장 최소가 되는 $A\hat{\underline{x}}$는 $\underline{b}$를 A의 column space로 정사영(Projection)을 시킨 vector**가 됩니다. 그럼 $\underline{b}$를 정사영을 시켜준다는 뜻이 되니, $\underline{b}$ 앞에 있는 $A (A^T A)^{-1} A^T = P_A$은 Projection Matrix가 됩니다.

## 데이터기반으로 해석
최소자승법은 모델의 파라미터를 구하기 위한 방법 중 하나입니다. 즉, 모델과 데이터간의 잔차(residual)의 제곱 합을 최소로하는 파라미터를 구하는 방법입니다. 가장 간단한 일차 방정식 예제를 통해 설명해보겠습니다.   
$f(x) = \beta x + \alpha, \quad y_i = \alpha + \beta x_i + u_i$의 관계식을 통해 설명하겠습니다.   
<img src="../../../assets/images/LinearAlgebra/2023-12-12-least squares/least squares 4.jpg" alt="least squares 4" style="zoom:80%;" />    
상기의 이미지는 현재 존재하는 데이터들에 대해 회귀식($y=x, y= x+2, y=2x+1$)을 나타낸 것입니다. 여기서 어떤 회귀식을 선택하냐에 따라 현재 데이터들의 분포를 가장 잘 나타낼 수 있는지를 평가할 수 있습니다. 상기의 관계식인 $y_i$는 현재 분포하고 있는 데이터를 나타내며, 여기서 $u_i$는 회귀식과의 오차를 나타내며 잔차(residual)가 됩니다. 그럼 잔차(residual)은 하기와 같이 됩니다.   
\begin{aligned}   
e_i = \| y_i - f(x_i) \| \newline
\end{aligned}   

그럼 여기서 이 잔차의 제곱 합을 최소로하는 회귀식을 구하면 됩니다. 회귀식의 파라미터는 **기울기와 절편인 $\alpha, \beta$**가 됩니다. 하기에 수식을 통해 정리해보겠습니다.   
\begin{aligned}   
\sum_i^{n} e_i =& \sum_i^{n} ( y_i - f(x_i) )^2 \newline   
=& \sum_i^{n} (y_i - \beta x_i - \alpha)^2 \newline   
OLS \; or \; LSM =& min(\sum_i^{n} (y_i - \beta x_i - \alpha)^2)
\end{aligned}  

OLS는 잔차(residual)을 최소로 해주는 파라미터를 찾을 수 있습니다.   
