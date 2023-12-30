---
title:  "PCA(Principal Component Analysis)"
categories: LinearAlgebra
tag: [theory, math]
tags: [Jekyll, MathJax]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: PCA의 정의와 어떤 방향인지
comments: true
date: 2023-12-30
toc_sticky: true
---

## PCA(Principal Component Analysis)
PCA는 한글로는 주성분 분석이라고합니다. 주성분 분석은 이전에 배웠던 Eigen value와 Eigen vector 등을 활용하는 방법 중에 하나입니다. 보통 통계 데이터분석(주성분 찾기), 데이터 압축(차원감소), 노이즈 제거 등 다양한 곳에 활용됩니다.   
먼저 PCA의 활용 예시에 대해 알아보겠습니다.   
### PCA란?
<img src="../../../assets/images/LinearAlgebra/2023-12-30-PrincipalComponentAnalysis/PCA 1.jpg" alt="PCA 1" style="zoom:80%;" />    
상기의 이미지에서 좌표에들에 보이는 점이 데이터들입니다. 이 데이터는 $(x,y)$로 이루어져 있으며, 이 데이터들이 어떤 특정한 분포를 따르고 있다고 하겠습니다. 그럼 이 분포의 평균으로부터 이 **데이터를 가장 잘 나타내는 방향을 주성분 방향**이라고 합니다. 그리고 이 방향을 가지는 vector를 찾는 것을 PCA라고 합니다. 그럼 데이터를 가장 잘 나타내는 방향을 가지는 vector로 데이터들을 Projection을 시키게 되면 2차원 차원에서 1개의 차원으로 축소를 시킬 수 있습니다.   
그럼 여기서 가장 중요한 <span styl='color:red'>**이 분포를 가장 잘 나타내는 vector는 어떤 vector일까**</span>입니다. 이 답은<span styl='color:red'>**이 분포의 분산이 가장 큰 방향이 이 분포를 가장 잘나타내는 방향**</span>이 됩니다. 그 다음으로 잘 나타내는 방향은 <span styl='color:red'>**분산이 가장 큰 방향과 직교하는 방향이 두번째 주성분**</span>이 됩니다. 그럼 왜 분산이 가장 큰 방향이 이 분포를 잘 나타내는가에 대해 알아보겠습니다.   
### 분산이 가장 큰 방향
우선 데이터와 평균에 대해 정의하겠습니다. 데이터는 $\underline{\tilde{d_i}} = \begin{bmatrix} x_i \newline y_i \end{bmatrix}$, 평균은 $\underline{\bar{d}} = \begin{bmatrix} \bar{x} \newline \bar{y} \end{bmatrix}$로 정의하겠습니다. 우선 평균이 0인 지점에서 구하는 것이 편하니 이 분포의 데이터들을 평균이 0인 지점으로 이동시키겠습니다.    
<img src="../../../assets/images/LinearAlgebra/2023-12-30-PrincipalComponentAnalysis/PCA 2.jpg" alt="PCA 2" style="zoom:80%;" />    
상기의 이미지는 분포의 데이터들을 이동시킨 후의 데이터들 입니다. 그럼 이동시킨 데이터들은 $\underline{d_i} = \underline{\tilde{d_i}} - \underline{\bar{d}}$이 됩니다. 그리고 주성분 방향의 vector를 $\underline{u}$라고 하겠습니다. 그럼 초록색점의 데이터에서 이 분포를 잘나타내는 방향의 vector로 Projection을 시켜보겠습니다.    
<img src="../../../assets/images/LinearAlgebra/2023-12-30-PrincipalComponentAnalysis/PCA 3.jpg" alt="PCA 3" style="zoom:80%;" />    
초록색 데이터와 주성분 vector간의 Projection을 시켰을 때의 차이인 분홍색의 크기가 오차를 나타내게 됩니다. 그럼 이 분포에 존재하는 **모든 데이터들과 이 주성분 vector간의 오차가 최소**가 되는 vector가 곧 이 분포를 가장 잘 나타내는 vector라고 할 수 있습니다. 그럼 이제 이해한 내용을 바탕으로 수식을 통해 정의해보겠습니다.   
\begin{aligned}    
min_{\underline{u}} \; \frac{1}{N} \sum_{i=1}^{N} (\underline{d_i} - \underline{d_i^T} \underline{u} \underline{u})^T (\underline{d_i} - \underline{d_i^T} \underline{u} \underline{u}) \newline   
s.t \quad ||\underline{u}||_2^2 = \underline{u^T} \underline{u} = 1
\end{aligned}    

상기의 수식은 총 N개의 데이터들에 대해 Projection내린 오차의 크기가 가장 최소가 되는 vector를 찾는 식입니다. 그럼 분홍색의 vector를 나타내면 $\underline{d_i} - \underline{d_i^T} \underline{u} \underline{u}$과 같이 나타낼 수 있으며, 이의 크기인 2-norm을 구하기 위해서는 자기 자신과 내적을 해주면 됩니다. 근데 여기서 한가지 조건이 필요합니다. 주성분 방향을 찾고자 하니, 크기가 중요하지 않은 방향이 중요합니다. 그럼 크기에 영향을 받지 않는 unit vector를 이용하면 된다는 의미가 됩니다.   
따라서 $ \| \| \underline{2} \| \|_2^2 = 1$을 만족 시키는 $\underline{u}$를 찾으면 됩니다. 그럼 이제 상기의 수식을 한 번 전개하여 왜 분산이 가장 큰 방향인지를 알아보겠습니다.    

\begin{aligned}    
PCA =& \frac{1}{N} \sum_{i=1}^{N} (\underline{d_i} - \underline{d_i^T} \underline{u} \underline{u})^T (\underline{d_i} - \underline{d_i^T} \underline{u} \underline{u}) \newline   
=& \frac{1}{N} \sum_{i=1}^{N} (\underline{d_i^T} \underline{d_i} - (\underline{d_i^T} \underline{u}) \underline{u^T} \underline{d_i} - \underline{d_i^T}(\underline{d_i^T} \underline{u}) \underline{u} + (\underline{d_i^T} \underline{u}) \underline{u^T} (\underline{d_i^T} \underline{u}) \underline{u} ) \newline    
\end{aligned}    

상기의 수식에서 $(\underline{d_i^T} \underline{u})$는 크기를 나타내는 scalar가 되니 자리를 바꾸든 상관이 없어집니다. 이를 활용하여 소거될 것들은 소거하고, 그리고 저희가 알고 싶은 것은 $\underline{u}$이니 $\underline{d_i^T} \underline{d_i}$ 또한 고려하지 않고 전개를 해보겠습니다.   

\begin{aligned}    
PCA =& \frac{1}{N} \sum_{i=1}^{N} (\underline{d_i^T} \underline{d_i} - (\underline{d_i^T} \underline{u}) \underline{u^T} \underline{d_i} - \underline{d_i^T}(\underline{d_i^T} \underline{u}) \underline{u} + (\underline{d_i^T} \underline{u}) \underline{u^T} (\underline{d_i^T} \underline{u}) \underline{u} ) \newline    
=& - \frac{1}{N} \sum_{i=1}^{N} \underline{u^T} \underline{d_i} (\underline{d_i^T} \underline{u}) (\underline{d_i} = \underline{\tilde{d_i}} - \underline{\bar{d}}) \newline   
=& - \underline{u^T} \frac{1}{N} \sum_{i=1}^{N} (\underline{\tilde{d_i}} - \underline{\bar{d}})(\underline{\tilde{d_i}} - \underline{\bar{d}})^T \underline{u} \newline   
=& - \underline{u^T} R_d \underline{u}
\end{aligned}    

여기서  $\frac{1}{N} \sum_{i=1}^{N} (\underline{\tilde{d_i}} - \underline{\bar{d}})(\underline{\tilde{d_i}} - \underline{\bar{d}})^T$은 데이터 sample들로 만들어진 **Covariance Matrix**가 됩니다. 또한, **Covariance의 MLE(Maximum Likelihood Estimation) Solution**이 됩니다. Covariance Matrix를 $R_d$라고 정의해 놓겠습니다. 추가적으로 Covariance Matrix는 항상 **Symmetric**하다는 성질이 있습니다. 최종적으로 **$- \underline{u^T} R_d \underline{u}$를 Minimize하는 것이 목표인데 음수 부호를 빼고 Maximize**로 바꿀수도 있습니다. 그럼 최종적으로 수식을 정리해보겠습니다.   

\begin{aligned}    
max_{\underline{u}} \; \underline{u^T} R_d \underline{u} \newline   
s.t \quad ||\underline{u}||_2^2 = \underline{u^T} \underline{u} = 1
\end{aligned}    

상기의 수식인 최적화를 해결하면 최종적으로 구하고자 하는 주성분 분석의 방향을 구할 수 있게 됩니다. 그럼 하기에 최적화 문제를 해결해보겠습니다. **라그랑지안(Lagrangian) 함수**를 이용하여 해결해보겠습니다.   
\begin{aligned}    
L =& \underline{u^T} R_d \underline{u} + \lambda(1 - \underline{u^T} \underline{u}) \newline   
dL_{\underline{u}} =& d\underline{u^T} R_d \underline{u} + \underline{u^T} R_d d \underline{u} - \lambda d \underline{u^T} \underline{u} - \lambda \underline{u^T} d \underline{u} \newline   
=& 2 \times \underline{u^T} R_d d \underline{u} - 2 \lambda \underline{u^T} d \underline{u} \newline   
=& \underline{u} (2 \underline{u^T} R_d - 2 \lambda \underline{u^T}) \newline   
=& \underline{u} \frac{\partial L}{\partial \underline{u^T}}   
\end{aligned}    

$d\underline{u}$앞에 있는 $(2 \underline{u^T} R_d - 2 \lambda \underline{u^T})$이 곧 변화량인 $\frac{\partial L}{\partial \underline{u^T}}$가 됩니다. 따라서 이 변화량이 0으로 가는 $\underline{u}$를 찾아주면 상기의 최적화 문제를 해결할 수 있습니다.    
\begin{aligned}    
\frac{\partial L}{\partial \underline{u^T}} =& 0 \newline   
(2 \underline{u^T} R_d - 2 \lambda \underline{u^T}) =& 0 \newline   
\underline{u^T} R_d =& \lambda \underline{u^T} \newline   
(\underline{u^T} R_d)^T =& (\lambda \underline{u^T})^T \quad(R_d : Symmetric) \newline   
R_d \underline{u} =& \lambda \underline{u} \newline   
\end{aligned}    

상기와 같이 최종적으로 $R_d \underline{u} = \lambda \underline{u}$의 결과가 나옵니다. 어디서 많이 봤던 구조인 것 같습니다. 이전 포스팅에서 배웠던 eigen vector와 eigen value입니다. 하지만, **Convex Problem**이 아니기 때문에, 만족하는 값들 중 잘 나타내는 것을 선별을 해야합니다.   

그럼 이제 왜 분산이 가장 큰 방향인지에 대해 알아보겠습니다. $R_d$는 Symmetric Matrix라고 했습니다. 그럼 이전 포스팅에서 배웠던 것을 생각해보면, <span style='color:blue'>**Symmetric Matrix라면 Diagnolizable하고, Diagnolizable하다면 Orthognoal Matrix로 Decomposition이 가능**</span>하다고 배웠습니다. 따라서 상기와 같이 decomposition이 됩니다.   
\begin{aligned}    
\underline{u^T} R_d \underline{u} =& \underline{u^T} (\lambda_1 \underline{q_1} \underline{q_1^T} + \lambda_2 \underline{q_2} \underline{q_2^T} + \ldots) \underline{u} \newline   
\end{aligned}   

근데 이전 수식인 $R_d \underline{u} = \lambda \underline{u}$을 통해 $\underline{u}$는 $R_d$의 eigen vector라는 것을 알 수 있습니다. 그럼 <span style='color:red'>**$\underline{u}$가 $R_d$의 eigen vector들이니 $\underline{q_1}, \underline{q_2}, \ldots$들 중에 하나**</span>가 됩니다. 그리고 상기의 $(\lambda_1 \underline{q_1} \underline{q_1^T} + \lambda_2 \underline{q_2} \underline{q_2^T} + \ldots)$이 $\lambda$의 크기순으로 정렬을 하여 나타낸 거라고 해보겠습니다. 그럼 $\underline{u}$가 $\underline{q_1}$일 때, $\underline{u^T} R_d \underline{u}$가 가증 큰 값을 가지게 됩니다. 그 이유에 대해 알아보겠습니다. <span style='color:red'>**$\underline{u^T} \lambda_1 \underline{q_1} \underline{q_1^T} \underline{u} = \underline{q_1^T} \lambda_1 \underline{q_1} \underline{q_1^T} \underline{q_1} = \lambda_1$(R_d는 orthognoal matrix로 decomposition했으니 나머지는 0)**</span>이 나옵니다.($\underline{q_1^T} \underline{q_1} = 1$ : orthonormal vector) 크기 순으로 정렬했다고 했으니 $\lambda_1$이 가장 큰값이 됩니다. 따라서 $R_d$의 eigen vector들인 $\underline{q_1}, \underline{q_2}, \ldots$들 중 가장 큰 값을 나타낸 것이 $\underline{q_1}$이 됩니다. 그럼 두번째로는 $\underline{q_2}$가 될 것 입니다. 근데 여기서 <span style='color:blue'>**$\underline{q_2}$는 orthogonal하니, $\underline{q_1}$과 수직 방향**</span>이 됩니다.   

### 차원 축소   
<img src="../../../assets/images/LinearAlgebra/2023-12-30-PrincipalComponentAnalysis/PCA 4.jpg" alt="PCA 4" style="zoom:80%;" />    
그럼 이제 분포를 가장 잘나타내는 방향의 vector인 $\underline{u} = \underline{q_1}$이라는 것을 알게 되었습니다. 즉, 현재는 2차원을 1차원으로 축소를 시켰습니다. 하지만 만약, 100차원인 데이터를 2차원으로 축소를 시킨다고 해보겠습니다. 첫번째 주성분 방향은 $\underline{q_1}$이고 두번째 주성분 방향은 $\underline{q_2}$가 됩니다. 그럼 데이터를 $\underline{d_i^T} \underline{q_1} \underline{q_1} + \underline{d_i^T} \underline{q_2} \underline{q_2}$로 내리면 됩니다. 즉, **100차원의 데이터를 2차원인 $\underline{q_1}, \underline{q_2}$가 span하는 공간으로 Projection을 시킨다**는 뜻이 됩니다.   
<img src="../../../assets/images/LinearAlgebra/2023-12-30-PrincipalComponentAnalysis/PCA 5.jpg" alt="PCA 5" style="zoom:80%;" />    
상기의 이미지에서처럼 100차원 데이터 $\underline{d_i}$를 2차원인 $\underline{q_1}, \underline{q_2}$가 span하는 공간으로 Projection을 시킬 때, 좌표는 $(\underline{d_i^T} \underline{q_1}, \underline{d_i^T} \underline{q_2})$가 되며, 그 점을 $\underline{d_i^T} \underline{q_1} \underline{q_1} + \underline{d_i^T} \underline{q_2} \underline{q_2}$라고 표현합니다.    

### 응용(얼굴 인식)
