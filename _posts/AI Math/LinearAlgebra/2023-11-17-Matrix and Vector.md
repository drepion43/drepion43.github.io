---
title:  "행렬과 벡터"
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
\end{bmatrix}$와 같이 $A^T = A$인 matrix는 **Symetric Matrix**라고 부릅니다.   
또한, 다음과 같이 conjugate를 갖고 있는 matrix인 $ A = \begin{bmatrix} 1 & 1 + i \newline 1-i & 2 \end{bmatrix}$의 경우 $(A^{\*})^T = A^H = A$를 만족하는 matrix를 **Hermitian Matrix**라고 부릅니다. **Hermitian Matrix**에 대해 하기에 추가적으로 더 설명하겠습니다.   
conjugate란 $a + bi$가 conjugate가 되면 $a - bi$로 바뀌게 됩니다. 즉, $A^{\*}$는 **Conjugate Matrix**가 됩니다. 예시를 들어 보겠습니다. $ A = \begin{bmatrix} 1 & -2 - i \newline 1+i & i \end{bmatrix}$인 matrix에 conjugate를 취하게 되면, $ A^* = \begin{bmatrix} 1 & -2 + i \newline 1 - i & -i \end{bmatrix}$가 됩니다. 여기서 전치(transpose)를 취해주면, $ (A^{\*})^T = A^H = \begin{bmatrix} 1 & 1 - i \newline -2 + i & -i \end{bmatrix}$가 됩니다. 즉, **Hermitian Matrix**는 $A = A^H$를 만족하는 matrix가 된다는 뜻입니다. 

이번에 전치의 성질에 대해 알아보겠습니다.   
① $(A^T)^T = A$   
② $(A + B)^T = A^T + B^T$   
③ $(AB)^T = B^T A^T$  
④ $(cA)^T = c A^T$  
⑤ $det(A^T) = det(A)$  
⑥ $(A^T)^{-1} = (A^{-1})^T$  
