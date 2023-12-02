---
title:  "행렬 벡터 공간"
categories: LinearAlgebra
tag: [theory, math]
tags: [Jekyll, MathJax]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: column space와 span
comments: true
date: 2023-11-28
toc_sticky: true
---

**이제부터 벡터는 $\underline{a}$로 표현하도록 하겠습니다.**

## 행렬의 곱셈
$\begin{Bmatrix}
x_1 + 2y_2 = 4, \quad  x_2 + 2y_2 = 3 \newline
2x_1 + 5y_2 = 9, \quad 2x_2 + 5y_2 = 7
\end{Bmatrix}$   
상기의 연릭 일차 방정식들을 행렬로 표현해보겠습니다.   

$\begin{bmatrix}
1 & 2 \newline
2 & 5
\end{bmatrix} 
\begin{bmatrix}
x_1 & x_2 \newline
y_1 & y_2
\end{bmatrix}
=
\begin{bmatrix}
4 & 3 \newline
9 & 7
\end{bmatrix}$   

여기서 왼쪽 행렬인 $\begin{bmatrix} 1 & 2 \newline 2 & 5 \end{bmatrix}$의 **열(column)**의 수와 오른쪽 행렬인  $\begin{bmatrix} x_1 & x_2 \newline y_1 & y_2 \end{bmatrix}$의 **행(row)**의 수가 맞아야 합니다. 왼쪽 행렬은 현재 2x<span style='color:blue'>**2**</span> 행렬이며 오른쪽 행렬 또한 <span style='color:blue'>**2**</span>x2이므로 맞으면 곱셈이 가능합니다.   
그럼 이제 결과 행렬에 대해 말해보겠습니다. 일단 행렬의 곱셈을 일반화 해보겠습니다.   
\begin{aligned} 
m \; \times k \quad k \times n = m \times n
\end{aligned}   
좌측 행렬의 column과 우측 행렬의 row가 모두 k로 통일되면, **좌측 행렬의 row인 $m$과 우측 행렬의 column인 $n$을 row와 column으로 가지는 행렬**이 나타납니다.   
또한, 행렬의 곱에서는 **순서가 매우 중요**합니다. 순서가 바뀌면 값 또한 바뀌게 됩니다.   


### 관점 I (내적)
<img src="../../../assets/images/LinearAlgebra/2023-11-28-Matrix Calculation/matrix representation.jpg" alt="Matrix representation 1" style="zoom:80%;" />       
상기의 이미지처럼 행렬은 곧 벡터를 쌓아서 만든 것과 동일합니다. 즉, 상기의 이미지처럼 A 행렬과 B 행렬을 벡터들의 모음으로 표현할 수 있습니다. 여기서 보통 벡터는 column 벡터로 생각을 하기 때문에, A 행렬의 경우 Tranpose를 취한 벡터들로 표현했습니다.   
그럼 행렬의 곱인 $AB$를 표현해보겠습니다.    
\begin{aligned} 
AB = \begin{bmatrix} \underline{a_1^T} \newline \underline{a_2}^T \newline \underline{a_3}^T \newline \underline{a_4}^T \end{bmatrix} 
\begin{bmatrix} \underline{b_1} & \underline{b_2} & \underline{b_3} & \underline{b_4} \end{bmatrix}
\end{aligned}   

즉, 4 x 1 행렬과 1 x 4 행렬의 곱으로 표현할 수 있습니다. 그럼 결과는 4 x 4가 될 것입니다.   
\begin{aligned} 
AB = \begin{bmatrix} \underline{a_1}^T\underline{b_1} & \underline{a_1}^T\underline{b_2} & \underline{a_1}^T\underline{b_3} & \underline{a_1}^T\underline{b_4} \newline 
\underline{a_2}^T\underline{b_1} & \underline{a_2}^T\underline{b_2} & \underline{a_2}^T\underline{b_3} & \underline{a_2}^T\underline{b_4} \newline 
\underline{a_3}^T\underline{b_1} & \underline{a_3}^T\underline{b_2} & \underline{a_3}^T\underline{b_3} & \underline{a_3}^T\underline{b_4} \newline 
\underline{a_4}^T\underline{b_1} & \underline{a_4}^T\underline{b_2} & \underline{a_4}^T\underline{b_3} & \underline{a_4}^T\underline{b_4}
\end{bmatrix} 
\end{aligned}   

상기처럼 내적들로 표현하여 내적으로 생각해볼 수 있습니다.

### 관점 II (rank-1 matrix sum)
현재 rank에 대해 설명을 안했기 때문에 일단은 행렬과 행렬의 더하기로 나타낸다고 생각하면 됩니다. 추후에 rank에 대해 설명하며 부가적으로 설명하겠습니다.   
내적에서 a 벡터는 (row vector)행 벡터 b 벡터는 (column vector)열 벡터로 봤었는데 반대로 생각하면 됩니다.   
\begin{aligned} 
AB = \begin{bmatrix} \underline{a_1} \newline \underline{a_2} \newline \underline{a_3} \newline \underline{a_4} \end{bmatrix} 
\begin{bmatrix} \underline{b_1}^T & \underline{b_2}^T & \underline{b_3}^T & \underline{b_4}^T \end{bmatrix}
= \underline{a_1}\underline{b_1}^T  + \underline{a_2}\underline{b_2}^T + \underline{a_3}\underline{b_3}^T + \underline{a_4}\underline{b_4}^T
\end{aligned}   

여기서 $\underline{a_1}\underline{b_1}^T$는 column vector와 row vector를 곱하게 되니, 4 x 1 과 1 x 4를 곱한 결과인 4 x 4 행렬이 나옵니다. 따라서, $\underline{a_1}\underline{b_1}^T, \underline{a_2}\underline{b_2}^T, \underline{a_3}\underline{b_3}^T, \underline{a_4}\underline{b_4}^T$가 모두 4 x 4 행렬의 덥셈으로 표현됩니다. 이렇게 column vector x row vector는 **필연적으로 그 결과 행렬에 rank가 반드시 1**이 됩니다. 따라서, $\underline{a_1}\underline{b_1}^T, \underline{a_2}\underline{b_2}^T, \underline{a_3}\underline{b_3}^T, \underline{a_4}\underline{b_4}^T$를 모두 **rank-1 matrix**라고 부릅니다. 

### 관점 III (<span style='color:red'>**Column space**</span>)
이번에는 행렬과 행렬의 곱이 아닌 **행렬과 벡터의 곱**에 대한 관점입니다. 행렬 A와 벡터 x의 곱을 나타내보겠습니다.   
\begin{aligned} 
A\underline{x} = \begin{bmatrix} \underline{a_1} \newline \underline{a_2} \newline \underline{a_3} \newline \end{bmatrix} 
\begin{bmatrix} x_1 & x_2 & x_3 \end{bmatrix}
= \underline{a_1}x_1  + \underline{a_2}x_2 + \underline{a_3}x_3
\end{aligned}   

여기서 $\underline{a_1}x_1$를 벡터 $\underline{a_1}$를 x_1만큼 스칼라(Scalar)배를 해줬다고 생각할 수 있습니다. 여기서 Scalar배가 실수 전체에 포함됬다고 가정을 한다면, $\underline{a_1}, \underline{a_2}, \underline{a_3}$인 **column vector들로 만들 수 있는 부분 공간인 column space**라고 할 수 있습니다. 예를 들어 보겠습니다.   
\begin{aligned} 
A = \begin{bmatrix} 1 & 0  \newline 0 & 1 \end{bmatrix} \;
B = \begin{bmatrix} 1 & 0 & 0 \newline 0 & 1 & 0 \newline 0 & 0 & 1  \end{bmatrix}   \;
C = \begin{bmatrix} 1 & 1 & 0 \newline 0 & 1 & 1 \newline 0 & 0 & 0  \end{bmatrix}
\end{aligned}   
A의 경우는 2차원 평면을 모두 나타낼 수 있습니다. B의 경우에는 3차원 평면을 모두 나타낼 수 있습니다. C의 경우에는 Scalar배를 어떻게 해줘도 3차원 부분에는 모두 0으로 되어있으니 2차원까지 밖에 나타낼 수 없습니다. 

추가적으로 행렬과 행렬의 곱도 $AB = A \begin{bmatrix} \underline{b_1} & \underline{b_2}  & \underline{b_3} \end{bmatrix} = \begin{bmatrix} A\underline{b_1} & A\underline{b_2}  & A\underline{b_3} \end{bmatrix}$로 나타낼 수 있습니다.   

### 관점 IV (<span style='color:red'>**Row space**</span>)
Column space로 바라봤던 것과 비슷하지만, A를 row by row로 나타내면 됩니다.   
\begin{aligned} 
\underline{x}^T A=\begin{bmatrix} x_1 & x_2 & x_3 \end{bmatrix}
\begin{bmatrix} \underline{a_1}^T \newline \underline{a_2}^T \newline \underline{a_3}^T \newline \end{bmatrix} 
= x_1\underline{a_1}^T  + x_2\underline{a_2}^T + x_3\underline{a_3}^T
\end{aligned}   
상기와 같이 나태내면 이전에는 column space로 나타냈다면, row space로 표현했다고 할 수 있습니다. 이 부분은 **transformer self-attention**에서 이용되기도 합니다. 

## span
원래 span의 정의는 **vecotr들의 linear combination으로 나타낼 수 있는 모든 vector들을 모은 집합**입니다. 즉, span은 **vector들을 이용하여 모든 선형결합을 통해 만들어진 vector space**이라고 할 수 있습니다. 이전에 설명했던 column space는 **column space가 span하는 vector**들입니다.   
### Linear Combination
> 선형 결합은 일차결합과 같은 말입니다.   
벡터 공간 V에 속하는 벡터 $v_1, v_2, v_3, ... $와 어떤 스칼라 $a_1, a_2, a_3, ...$에 대한 선형 결합(linear combination)은 다음 꼴로 나타납니다.   
\begin{aligned} 
a_1 v_1 + a_2 v_2 + a_3 v_3 + ... + a_n v_n
\end{aligned}   

즉, vector space에 있는 vector들에 Scalar배를 하여 더한다는 뜻입니다. 여기서 곱하고 더하기만 하게되니 linear하다고 할 수 있으며, vector들을 가지고 조합해서 만드니 combination이라고 할 수 있습니다.   
여기서, $a_1, a_2, a_3$들은 새로운 vector를 만들 때, $v_1, v_2, v_3$들을 얼마나 사용하여 만들지를 결정하는 계수(coefficient)가 됩니다. 하기의 이미지와 같이 좌측의 경우, 모든 2차원을 표현할 수 있지만, 우측의 경우, 한 선에 대해서만 나타낼 수 있습니다.    
<img src="../../../assets/images/LinearAlgebra/2023-11-28-Matrix Calculation/column space 1.jpg" alt="Column space 1" style="zoom:80%;" />    
즉, vector들에 따라, 점만 나타낼 수도 있고, 선만 나타낼 수도 있으며, 2차원을 나타낼 수도 있습니다.    
      
그럼 이전에 설명했던 **column space는 행렬의 column들이 span하는 vector space**입니다. column space는 range라고도 하며, 기호로 $C(A)$ 또는 $range(A)$로 표기하기도 합니다.   

### Linear Independent
Span은<span style='color:red'> **어떤 vector들을 가지고 span**</span>하느냐에 따라 표현할 수 있는 영역이 달라집니다. 즉, vector들을 선택하는게 매우 중요하다고 할 수 있습니다. 그럼 좋은 vector들을 가지고 span을 하면 더 고차원을 표현할 수 있다는 뜻입니다. 그럼 이 좋은 vector들은 어떻게 선별을 하면될까요? **vector들이 서로 Linear Independent한 vector들을** 가지고 span하는게 가장 좋은 방법입니다.   

우선 Linear Independent의 정의에 대해 알아보겠습니다.   
<img src="../../../assets/images/LinearAlgebra/2023-11-28-Matrix Calculation/Linear Independent 1.png" alt="Linear Independent 1" style="zoom:100%;" />    
즉, $x_1 \underline{v_1} + x_2 \underline{v_2} + x_3 \underline{v_3} + ... = \underline{0}$을 만족하기 위해 **계수들인 $x_1,x_2,x_3,...$들이 모두 반드시 0이어야 한다**면 Linear Independent하다고 할 수 있습니다. 하기의 그림을 통해 확인해보겠습니다.   
<img src="../../../assets/images/LinearAlgebra/2023-11-28-Matrix Calculation/Linear Independent 2.jpg" alt="Linear Independent 2" style="zoom:80%;" />    
상기의 이미지처럼 주황색 두개의 vector를 1차원인 선만 표현을 할 수 있습니다. 즉, 주황색 두개의 vector는 linear independent가 아닙니다. 하지만, 보라색과 주황색 1개의 vector의 경우는 2차원 평면을 나타낼 수 있습니다. 그 이유는 두 개가 linear independent하기 때문입니다. 이 보라색 vector는 주황색 vector와 orthogonal합니다. 여기서 알 수 있는 것이, **orthogonal 한 vector들은 서로 Linear Independent하다**고 할 수 있습니다. 하지만, Linear Independent한 vector들이 모두 Orthogonal하지는 않습니다. 즉, $Independent \supset Orthogonal$을 만족한다고 할 수 있습니다.

그럼 반대로, Linear dependent는 $x_1 \underline{v_1} + x_2 \underline{v_2} + x_3 \underline{v_3} + ... = \underline{0}$에서 **계수들인 $x_1,x_2,x_3,...$들 중 하나라도 nonzero**라면 Linear dependent하다고 할 수 있습니다. 예를 들어 보겠습니다.   
\begin{aligned} 
-2\begin{bmatrix} 1 \newline 1 \end{bmatrix}
+
1\begin{bmatrix} 2 \newline 2 \end{bmatrix} 
= 0 
\end{aligned}   
상기의 경우 Linear Independent의 정의를 만족못하니 dependent합니다.    

만약에, $x_1 \underline{v_1} + x_2 \underline{v_2} = \underline{0}$가 linear independent하다면 2차원을 표현할 수 있고, $x_1 \underline{v_1} + x_2 \underline{v_2} + x_3 \underline{v_3} = \underline{0}$라면 3차원을 표현할 수 있습니다. 즉, **Linear Independent한 vector들의 수가 곧 표현할 수 있는 Dimension**이 됩니다.   

### Basis(기저)
우선 Basis의 원래 정의에 대해 설명하겠습니다.   
> 벡터 공간 V에 대하여 S : \{ $v_1, v_2, v_3, ...$ \}은 V의 부분 집합이라고 하겠습니다.   
집합 S가 다음 두 조건을 만족할 때 S는 V의 Basis라고 합니다.   
① \{ $v_1, v_2, v_3, ...$ \}이 V를 생성합니다.   
② \{ $v_1, v_2, v_3, ...$ \}이 모두 선형 독립(linearly independent)입니다.    

Basis는 쉽게 말해 어떤 공간을 이루는 필수적인 구성 요소라고 할 수 있습니다. 즉, **space를 나타내는 linearly independent한 vector들**이라고 할 수 있습니다.   
한 번 2차원 basis들의 예시르 들어 보겠습니다.   
\begin{aligned} 
A = \begin{bmatrix} 1 \newline 0 \end{bmatrix}
\begin{bmatrix} 0 \newline 1 \end{bmatrix} \quad 
B = \begin{bmatrix} 1 \newline 0 \end{bmatrix}
\begin{bmatrix} 1 \newline 1 \end{bmatrix} \quad 
C = \begin{bmatrix} 1 \newline 0 \end{bmatrix}
\begin{bmatrix} 2 \newline 0 \end{bmatrix}  
\end{aligned}   

A의 경우에는 두 vector들이 linearly independent하며 orthogonal까지 하기 때문에, **orthogonal basis**라고 합니다. B의 경우에는 두 vector들이 linearly independent하기 때문에 2차원의 basis입니다. C의 경우에는 1차원은 나타내지만, 2차원은 나타내지 못합니다. 따라서 2차원 basis라고 할 수 없습니다.   
