---
title:  "그람-슈미트 직교화(Gram-Schmidt Orthogonalization)"
categories: LinearAlgebra
tag: [theory, math]
tags: [Jekyll, MathJax]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: 그람-슈미트 직교화 정의
comments: true
date: 2024-1-11
toc_sticky: true
---

## Gram-Schmidt Orthogonalization
그람-슈미트의 정의는 매우 간단합니다. 쉽게 말하면 임의의 vector 집합으로부터 Orthognoal set을 구하는 것입니다. Orthogonal set를 구하는 과정은 가지고 있던 vector 집합에서 한 vector를 다른 vector로 Projection을 시킨 것을 이용해 Orthogonal set를 구할 수 있습니다.   

그림
더 좋은 이해를 위해 예시를 들어 설명해보겠습니다.   
만약, 서로 linear independent한 vector들 $\underline{a_1}, \underline{a_2}, \underline{a_3}, \underline{a_4}$ 4개가 있다고 가정해보겠습니다. 이 4개의 vector는 linearly independent하니 4개의 vector를 가지고 span을 하면, 4차원 공간이 span될 것 입니다. 그럼 이 **span한 공간이랑도 Orthogonal한 공간이 있을텐데, 이 공간의 basis를 찾는 것**이 목적입니다. 즉, 상기의 그림처럼 $\underline{q_1} \bot \underline{q_2} \bot \underline{q_3} \bot \underline{a_4}$와 같은 **Orthogonal basis**을 얻고자 하는 방법입니다.   

### 과정
$\underline{a_1}, \underline{a_2}, \underline{a_3}, \underline{a_4}$이 4개의 vector를 이용하여 구하는 방법에 대한 예시를 보여보겠습니다.   
그림   
$\underline{a_1}$을 축으로 $\underline{a_1}$과 직교하는 vector들로 바꿔보겠습니다. 즉, $\underline{a_1}=\underline{v_1}$이고 $\underline{q_1}$는 unit vector이니 $\underline{q_1}=\frac{\underline{v_1}}{|| \underline{v_1} ||}$이 되는 것입니다. 그럼 상기의 이미지와 같이 $\underline{a_2}$를 $\underline{v_1}$에 Projection을 시킨 후 Projection시킨 vector와 $\underline{a_2}$를 빼주면 $\underline{v_1}$과 직교하는 vector를 찾을 수 있습니다. 그 vector를 상기의 이미지처럼 $\underline{v_2}$라고 하겠습니다. 그럼 하기와 같이 정리할 수 있습니다.   
\begin{aligned}    
\underline{v_1} =& \underline{a_1} \newline    
\underline{v_2} =& \underline{a_2} - (\underline{a_2}^T \frac{\underline{v_1}}{|| \underline{v_1} ||}) \cdot \frac{\underline{v_1}}{|| \underline{v_1} ||} \newline   
=& \underline{a_2} - \frac{\underline{a_2}^T \underline{v_1}}{|| \underline{v_1} ||^2} \cdot \underline{v_1} \newline
\end{aligned}    

지금까지 $\underline{v_1} \bot \underline{v_2}$까지 구하는 방법에 대해 알아보았습니다. 그럼 이제 $\underline{q_3}$를 구하는 방법에 대해 알아보겠습니다.
그림   
상기의 이미지와 같이 4개의 vector로 4차원을 나타낸다고 했으니, $\underline{a_3}$는 $\underline{a_1}, \underline{a_2}$가 span하는 공간과 다른 차원에 존재하는 vector일 것입니다. 그럼 $\underline{a_1}, \underline{a_2}$가 span하는 공간과 직교하는 vector를 찾으면 $\underline{a_1}, \underline{a_2}$인 2개의 vector와도 직교를 하게 될 것입니다. 그 vector를 $\underline{v_3}$라고 정의하겠습니다. 그럼 $\underline{v_3}$를 구하기 위해서는 $\underline{a_3}$를 $\underline{a_1}, \underline{a_2}$가 span하는 공간에 Projection을 시킨 vector와 빼주면 $\underline{v_3}$를 찾을 수 있습니다. 즉, 이 방법은 예전에 포스팅 했던 Least Squares를 구할 때 사용했던 방법과 동일하게 생각하시면 될 것 입니다. $A \underline{x} = b$를 구하는 방법에 대해 알아보았는데, 이 때 $A$의 Column space인 $C(A)$ $\underline{x}$를 정사영을 시켰었습니다. 이와 동일하게 생각하면 이전에 구했던 $\[\underline{v_1}, \underline{v_2}\] = V=A$라고 생각을 하시면 이해하는데 더 큰 도움이 되실 겁니다. 즉, $\underline{v_1}, \underline{v_2}$가 span하는 공간인 $C(V)$에 $\underline{a_3}$를 Projection시키면 된다는 뜻입니다. 그럼 하기에서 구하는 수식에 대해 알아보겠습니다.   
\begin{aligned}    
V \underline{\hat{x}} =& \underline{a_3} \newline   
\underline{\hat{x}} =& (V^TV)^{-1} V^T \underline{a_3} \newline   
V \underline{\hat{x}} =& V(V^TV)^{-1} V^T \underline{a_3} \newline   
\underline{v_3} =& \underline{a_3} - V(V^TV)^{-1} V^T \underline{a_3} \newline   
=& \underline{a_3} - \begin{bmatrix} \underline{v_1} & \underline{v_2} \end{bmatrix}(\begin{bmatrix} \underline{v_1}^T \newline \underline{v_2}^T \end{bmatrix} \begin{bmatrix} \underline{v_1} & \underline{v_2} \end{bmatrix})^{-1} \begin{bmatrix} \underline{v_1}^T \newline \underline{v_2}^T \end{bmatrix} \underline{a_3} \newline   
(\begin{bmatrix} \underline{v_1}^T \newline \underline{v_2}^T \end{bmatrix} \begin{bmatrix} \underline{v_1} & \underline{v_2} \end{bmatrix})^{-1} =& \begin{bmatrix} \underline{v_1}^T \underline{v_1} & \underline{v_1}^T \underline{v_2} \newline \underline{v_2}^T \underline{v_1} & \underline{v_2}^T \underline{v_2} \end{bmatrix} \newline   
=& \begin{bmatrix} \frac{1}{|| \underline{v_1} ||^2} & 0 \newline 0 & \frac{1}{|| \underline{v_2} ||^2} \end{bmatrix} \newline   
\underline{v_3} =& \underline{a_3} - (\frac{\underline{v_1} \underline{v_1}^T \underline{a_3}}{ || \underline{v_1} ||^2 } + \frac{\underline{v_2} \underline{v_2}^T \underline{a_3}}{ || \underline{v_2} ||^2 }) \newline   
=& \underline{a_3} - \frac{\underline{a_3}^T \underline{v_1}}{ || \underline{v_1} ||^2 } \underline{v_1} - \frac{ \underline{a_3}^T \underline{v_2}}{ || \underline{v_2} ||^2 }\underline{v_2} \newline   
\end{aligned}    

현재까지 $\underline{v_1} \bot \underline{v_2} \bot \underline{v_3}$까지 구하는 방법에 대해 알아보았습니다. 그럼 이제 마지막으로 $\underline{q_4}$를 구하는 방법에 대해 알아보겠습니다. 각각 $\underline{v_2}, \underline{v_3}$를 구하는 식을 보면 뭔가 패턴이 보입니다. $\underline{v_2}$를 구하기 위해서는 $\underline{a_2}$를 $\underline{v_1}$에 Projection 시킨 vector와 빼서 구했습니다. 똑같이 $\underline{v_3}$를 구하기 위해서 $\underline{a_3}$를 $\underline{v_1}$에 Projection 시킨 vector와 $\underline{v_2}$에 Projection 시킨 vector 2개를 모두 빼줬습니다. 그럼 $\underline{v_4}$를 구하기 위해서는 똑같이 $\underline{v_1} , \underline{v_2}, \underline{v_3}$에 Projection 시킨 vector들을 각각 $\underline{a_4}$에 빼주면 된다는 것을 알 수 있을 것 입니다.   
\begin{aligned}    
\underline{v_4} =& \underline{a_4} - \frac{\underline{a_4}^T \underline{v_1}}{|| \underline{v_1} ||^2} \underline{v_1} -\frac{\underline{a_4}^T \underline{v_2}}{|| \underline{v_2} ||^2} \underline{v_2} -\frac{\underline{a_4}^T \underline{v_3}}{|| \underline{v_3} ||^2} \underline{v_3} 
\end{aligned}    

그럼 이제 구한 $\underline{v}$들을 하기에 정리해보겠습니다.   
\begin{aligned}    
\underline{v_1} =& \underline{a_1} \newline   
\underline{v_2} =& \underline{a_2} - \frac{\underline{a_2}^T \underline{v_1}}{|| \underline{v_1} ||^2} \cdot \underline{v_1} \newline   
\underline{v_3} =& \underline{a_3} - \frac{\underline{a_3}^T \underline{v_1}}{ || \underline{v_1} ||^2 } \underline{v_1} - \frac{ \underline{a_3}^T \underline{v_2}}{ || \underline{v_2} ||^2 }\underline{v_2} \newline   
\underline{v_4} =& \underline{a_4} - \frac{\underline{a_4}^T \underline{v_1}}{|| \underline{v_1} ||^2} \underline{v_1} -\frac{\underline{a_4}^T \underline{v_2}}{|| \underline{v_2} ||^2} \underline{v_2} -\frac{\underline{a_4}^T \underline{v_3}}{|| \underline{v_3} ||^2} \underline{v_3} 
\end{aligned}   

그럼 Orthonormal vector인 $\underline{q_1} \bot \underline{q_2} \bot \underline{q_3} \bot \underline{q_4}$들을 구해보겠습니다. 이전 정의 처럼 **Normalize**만 취해주면 됩니다.   
\begin{aligned}    
\underline{q_1} = \frac{\underline{v_1}}{|| \underline{v_1} ||} \newline   
\underline{q_2} = \frac{\underline{v_2}}{|| \underline{v_2} ||} \newline   
\underline{q_3} = \frac{\underline{v_3}}{|| \underline{v_3} ||} \newline   
\underline{q_4} = \frac{\underline{v_4}}{|| \underline{v_4} ||}
\end{aligned}   

그럼 $\underline{v}$들은 서로 orthognoal한 vector들이고, $\underline{q}$들은 서로 orthonormal한 vector들이 됩니다. 