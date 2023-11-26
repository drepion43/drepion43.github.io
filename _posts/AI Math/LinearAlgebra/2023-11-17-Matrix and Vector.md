---
title:  "행렬과 벡터의 성질 1"
categories: LinearAlgebra
tag: [theory, math]
tags: [Jekyll, MathJax]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: 행렬과 벡터란
comments: true
date: 2023-11-17
toc_sticky: true
---

## 선형대수학이란?
선형대수학에서 "대"란 "代(대신할 대)"를 뜻합니다. 즉, 숫자를 대신해 문자를 이용하여 방정식을 푸는 수학이라는 뜻입니다. 위키백과에 표현되있는 선형대수학의 정의는 하기와 같습니다.   
> 선형대수학(線型代數學, 영어: linear algebra)은 벡터 공간, 벡터, 선형 변환, 행렬, 연립 선형 방정식 등을 연구하는 대수학의 한 분야이다.   

우리가 흔히 알고 있는 $ax^2 + bx + c = 0$과 방정식을 선형(1차) 방정식으로 표현하여 해결하는 분야입니다. $ax_1 + bx_2 + c = 0$으로 표현하면 선형(1차) 방정식으로 생각하여 해결할 수 있습니다.   
선형대수학에서는 단지 **행렬**과 **벡터**로 표현하여 방정식을 해결합니다.    

## 연립 일차 방정식
우리가 흔히 알고 있는 연립 일차 방정식을 표현해보겠습니다.   
$\begin{Bmatrix}
x + 2y = 4 \newline
2x + 5y = 9
\end{Bmatrix}$   
상기의 연립 일차 방정식을 **행렬**과 **벡터**로 표현해보겠습니다.   
$\begin{bmatrix}
1 & 2 \newline
2 & 5
\end{bmatrix} 
\begin{bmatrix}
x \newline
y
\end{bmatrix}
=
\begin{bmatrix}
4 \newline
9
\end{bmatrix}$
로 나타낼 수 있습니다. 즉, $Ax = y$의 꼴이 됩니다.   

## 벡터
벡터를 직교좌표계에 표현하면 하기와 같이 나타낼 수 있습니다.   
그림   
그럼 이제 벡터의 연산을 확인해보겠습니다.   
벡터끼리의 연산은 하기와 같이 됩니다.   
그림   
벡터의 스칼라배는 하기와 같이 됩니다.   
그림   
이번에는 2차원 벡터를 통해 2차원 좌표 평면 전체를 표현해겠습니다. 하기와 같이 unit vector에 스칼라배를 해주면 2차원 평면을 다 나타낼 수 있습니다.   
그림   


## 전치(transpose)
전치란 행렬의 diagnoal인 원소들만 제외하고 대칭적으로 자리를 바꾸면 됩니다.(off-diagonal elements : diagnoal elements가 아닌 elements) 즉, $A$의 원소인 $a_{ij}$의 위치에 있는 값을 $a_{ji}$의 위치의 값으로 바꿔주면 됩니다.      
$ A =
\begin{bmatrix}
1 & 2 & 3 \newline
4 & 5 & 6 \newline
7 & 8 & 9
\end{bmatrix} $
$ A^T =
\begin{bmatrix}
1 & 4 & 7 \newline
2 & 5 & 8 \newline
3 & 6 & 9
\end{bmatrix}$   
과 같이 나타낼 수 있습니다.  
참고로 $ A =
\begin{bmatrix}
1 & 2 & 3 \newline
2 & 5 & 4 \newline
3 & 4 & 9
\end{bmatrix}$와 같이 $A^T = A$인 matrix는 **Symmetric Matrix**라고 부릅니다.   
또한, 다음과 같이 conjugate를 갖고 있는 matrix인 $ A = \begin{bmatrix} 1 & 1 + i \newline 1-i & 2 \end{bmatrix}$의 경우 $(A^{\*})^T = A^H = A$를 만족하는 matrix를 **Hermitian Matrix**라고 부릅니다. **Hermitian Matrix**에 대해 하기에 추가적으로 더 설명하겠습니다.   
conjugate란 $a + bi$가 conjugate가 되면 $a - bi$로 바뀌게 됩니다. 즉, $A^{\*}$는 **Conjugate Matrix**가 됩니다. 예시를 들어 보겠습니다. $ A = \begin{bmatrix} 1 & -2 - i \newline 1+i & i \end{bmatrix}$인 matrix에 conjugate를 취하게 되면, $ A^* = \begin{bmatrix} 1 & -2 + i \newline 1 - i & -i \end{bmatrix}$가 됩니다. 여기서 전치(transpose)를 취해주면, $ (A^{\*})^T = A^H = \begin{bmatrix} 1 & 1 - i \newline -2 + i & -i \end{bmatrix}$가 됩니다. 즉, **Hermitian Matrix**는 $A = A^H$를 만족하는 matrix가 된다는 뜻입니다. 

이번에 전치의 성질에 대해 알아보겠습니다.   
① $(A^T)^T = A$   
② $(A + B)^T = A^T + B^T$   
③ $(AB)^T = B^T A^T$   
&rarr; $(A^T A)^T = A^T A $와 $(A A^T)^T = A A^T$로 **Symmetric Matrix**입니다.   
④ $(cA)^T = c A^T$  
⑤ $det(A^T) = det(A)$  
⑥ $(A^T)^{-1} = (A^{-1})^T$  

## 내적과 정사영
**Dot Product**는 두 벡터간의 닮은 정도를 의미합니다. **Dot Product**의 계산은 각각의 성분을 곱하고 더하면 됩니다.         
Dot Product는 Sclar Product와 같은 말이며, inner product와는 다릅니다. $inner \; product \supset (dot \; product = scalar \; product)$와 같은 관계를 가집니다.   
$\begin{bmatrix} 1 \newline 3 \end{bmatrix} \cdot \begin{bmatrix} 5 \newline 1 \end{bmatrix} = 8 = a^T b = b^T a$   
8이라는 스칼라 값을 가지기기 때문에 $a^T b$에 $(a^T b)^T$를 취해줘도 같은 값을 가집니다.   

이제 Dot Product의 수식을 하기와 같습니다.   
\begin{aligned} 
a^T b = \| \| a \| \| \; \| \| b \| \| cos \theta
\end{aligned}

Dot Product를 다양하게 해석할 수도 있지만, 저는 두 벡터중 한개의 벡터를 다른 벡터로 정사영(projection)을 시킨 후의 크기와 다른 벡터의 크기와의 곱으로 정의하고 있습니다.   
이렇게 정의를 할 시, $a^T b = \| \|a \| \| cose \theta \|\|b\|\|$로 생각할 수 있습니다. 하기의 이미지를 통해 추가적으로 설명하겠습니다.    
<img src="../../../assets/images/LinearAlgebra/2023-11-17-Matrix and Vector/dot product1.jpg" alt="Dot product 1" style="zoom:80%;" />    
이미지와 같이 정사영을 시킨 크기인 $\| \|a \| \| cose$나 $\| \|b \| \| cose$와 나머지 베ㄱ터의 크기를 곱해준 결과가 Dot Product의 결과가 됩니다.   
그럼 한개 벡터의 크기를 구할려면 어떻게 하면 될까요?   
$a^T a = \| \|a \| \| \; | \|a \| \| cos 0 = \| \|a \| \| \; \| \|a \| \| = \| \|a \| \|^2$이 됩니다. 2-norm의 제곱이 되니 크기는 2-norm 구해주면 됩니다. 따라서, $\sqrt{a^T a}$가 벡터의 크기가 됩니다.   

그럼 이제 **단위 벡터(Unit Vector)**에 대해 알아보겠습니다.   
단위 벡터는 크기가 1인 벡터를 의미합니다. 즉, 크기가 1이 아닌 벡터에 대해 **방향은 보존**하며 **크기만 1인 벡터**로 만들어주면 됩니다. 즉, 방향성만 가지고 있는 벡터를 의미합니다.   
\begin{aligned} 
\frac{\vec{a}}{\sqrt{a^T a}}
\end{aligned}   

그럼 이 단위 벡터와 내적을 했을 때, 어떤 방향의 단위 벡터일 때 해당 벡터의 성분을 가장 잘 나타낼 수 있는지 알아보겠습니다.   
<img src="../../../assets/images/LinearAlgebra/2023-11-17-Matrix and Vector/dot product2.jpg" alt="Unit Vector 1" style="zoom:80%;" />    
처음 주황색 방향의 단위 벡터와 내적을 했을 때, 4의 값이 도출됩니다. 즉, 단위 벡터는 x축 방향으로 4의 성분만 가지고 있다는 뜻이 됩니다. 그럼 어느 방향일 때 최대가 되는지 알아보겠습니다. 보라색 방향으로 나타냈을 때, 내적하는 벡터와 같은 방향을 나타내므로 내적의 값이 가장 커집니다. 하지만, 음의 방향인 남색 방향인 경우에는 음수를 가지므로 가장 최솟값을 가지게됩니다. 또한, 추후에 배울 수직인 Orthogonal인 경우에는 내적값이 0이 됩니다. 이는 내적의 정의에서 $cos 90 = 0$의 값을 가지기 때문에 0을 가집니다. 연두색 방향이 해당 Orthogonal인 벡터가 됩니다.   

이제 내적을 각 성분의 곱과 덧셈을 통해서 구하는 것이 아닌 일반화를 시켜보겠습니다.   
<img src="../../../assets/images/LinearAlgebra/2023-11-17-Matrix and Vector/dot product3.jpg" alt="Dot Product 2" style="zoom:80%;" />    
$\vec{a}$를 $\vec{b}$로 정사영(projection)을 시키겠습니다. 그럼 수식에서 $a^T b = \| \|a \| \| cos \theta \|\| b \|\|$에서 $\| \|a \| \| cos \theta$가 정사영(projection)을 내린 크기가됩니다. 그럼 식을 넘겨서 정사영 내린 크기를 구해보겠습니다.   
$\| \|a \| \| cos \theta = \frac{a^T b}{\|\| b \|\|} = \frac{a^T b}{\sqrt{b^T b}}$가 됩니다. 정사영 내린 크기는 다음과 같이 되지만, 여기에는 방향이 존재하지 않는 **스칼라(Scalar)**값이 됩니다. 그럼 방향을 추가해줘야 주황색 방향의 벡터가 완성됩니다. 방향은 $\vec{b}$로 정사영을 시켰으니 $\vec{b}$의 방향을 가집니다. 그럼 여기서 방향만을 나타내는 단위 벡터를 곱해주면 됩니다. 따라서, 주황색의 벡터는 하기와 같이 나타내집니다.   
\begin{aligned} 
\frac{a^T b}{\sqrt{b^T b}} \frac{b}{\sqrt{b^T b}} = \frac{a^T b}{b^T b} b = \frac{8}{26} \cdot \begin{bmatrix} 5 \newline 1 \end{bmatrix}
\end{aligned}   

그럼 이번에는 보라색 벡터인 $\vec{b}$를 $\vec{a}$에 정사영 시킨 벡터를 구해보겠습니다.   
\begin{aligned} 
\frac{a^T b}{\sqrt{a^T a}} \frac{a}{\sqrt{a^T a}} = \frac{a^T b}{a^T a} a = \frac{8}{10} \cdot \begin{bmatrix} 1 \newline 3 \end{bmatrix}
\end{aligned}   


이번에는 추후에 **least squarse 구할 때 쓰이는 중요한 방법**으로 해결해보겠습니다.   
<img src="../../../assets/images/LinearAlgebra/2023-11-17-Matrix and Vector/dot product4.jpg" alt="Dot Product 3" style="zoom:80%;" />    
$\vec{a}$를 $\vec{b}$로 정사영(projection)을 시킨 후 $\vec{b}$의 방향인 주황색 벡터를 $b \cdot \hat{x}$로 설정해보겠습니다. 그럼 $\vec{b}$와 수직인(orthogonal)인 벡터는 $\vec{a} - \vec{b} \cdot \hat{x}$가 됩니다.   
이전에 설명했 듯이, 수직인 벡터와의 내적 값은 0이라고 했습니다. 이 말은 $b \cdot \hat{x} \bot \vec{a} - \vec{b} \cdot \hat{x}$이며 $(\vec{a} - \vec{b} \cdot \hat{x})^T \cdot b \cdot \hat{x} = 0$을 의미합니다. 여기서 $\hat{x}$는 스칼라값이며 절대 0이 될 수 없다는 것을 알 수 있습니다. 그럼 $(\vec{a} - \vec{b} \cdot \hat{x})^T \cdot b$ 부분이 0이 되야 하는 것을 알 수 있습니다.   
\begin{aligned} 
(\vec{a} - \vec{b} \hat{x})^T \cdot b =& 0 \newline
a^Tb - b^T b \hat{x} =& 0 \newline
\hat{x} =& \frac{a^T b}{b^T b}
\end{aligned}   

따라서, 주황색 벡터는 $\frac{a^T b}{b^T b} \vec{b}$인 것을 구할 수 있습니다.   

참고로, 주황색 벡터를 구하기 위해 $a^T (\frac{b}{\sqrt{b^T b}})$는 $\vec{a}$와 b 방향의 닮은 정도를 구한다는 뜻이 됩니다. 여기에 b의 방향인 $\frac{b}{\sqrt{b^T b}}$ 곱해줘도 $a^T b$를 구할 수 있습니다.   
