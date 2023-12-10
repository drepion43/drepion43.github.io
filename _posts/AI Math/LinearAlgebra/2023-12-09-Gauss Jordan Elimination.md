---
title:  "가우스 조던 소거법(Gauss Jordan Elimination)"
categories: LinearAlgebra
tag: [theory, math]
tags: [Jekyll, MathJax]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: Gauss Jordan Elimination
comments: true
date: 2023-12-09
toc_sticky: true
---

## 가우스 조던 소거법(Gauss Jordan Elimination)
이전 포스팅에서 배웠던 $A\underline{x} = \underline{b}$와 같은 형태로 바꾸어 푸는 과정을 **Linear system**이라고 합니다. 이렇게 연릭 방정식을 행렬로 변형하여 풀면 보다 쉽게 해결할 수 있습니다. 이번에는 이 Linear system을 해결하는 방법 중 하나인 **Gauss Elimination**에 대해 알아보겠습니다. Gaus Elimination은 미지수가 n개인 연립일차방정식을 다음과 같은 $A\underline{x}=\underline{b}$같은 행렬식으로 표현하여 해결하는 방법입니다. 하기에 예시를 들어보겠습니다.   
$\begin{Bmatrix} x + 2y = 4   \newline 2x + 5y = 9 \end{Bmatrix}$ &rarr; $\begin{bmatrix} 1 & 2  \newline 2 & 5 \end{bmatrix} \begin{bmatrix} x  \newline y \end{bmatrix} = \begin{bmatrix} 4  \newline 9 \end{bmatrix}$   
좌측의 연립일차방정식을 우측의 행렬연산꼴로 나타내서 해결하는 방법입니다. $A$에는 계수, $\underline{x}$에는 미지수, $\underline{b}$에는 답(상수항)을 표기한 꼴입니다.  
근데 여기서 **확장 행렬(Augmented Matrix)**를 이용하여 해결합니다. 확장 행렬(Augmented Matrix)이란 계수 $A$와 상수항을 담은 $\underline{b}$를 결합한 형태입니다. 하기와 같이 표기하면 됩니다.   
$\begin{bmatrix} 1 & 2 & \vdots & 4  \newline 2 & 5 & \vdots & 9 \end{bmatrix}$   
상기와 같이 $\[ A \| b \]$꼴로 나타내면 됩니다. row 순서대로 제일 상위의 row를 $L_1= \begin{bmatrix} 1 & 2 & \vdots & 4 \end{bmatrix}, \quad L_2 = \begin{bmatrix} 2 & 5 & \vdots & 9 \end{bmatrix}$을 나타냅니다. 

이 $L_1, L_2$에 대해 하기와 같은 연산을 취해줄 수 있습니다.   
①행끼리 자리르 교체할 수 있습니다.   
②하나의 행에 0이 아닌 상수 k를 곱할 수 있습니다. (행의 상수배)   
③한 행을 다른 행에 상수 k를 곱하여 이 행에 더한 행으로 교체할 수 있습니다. (행의 교체)   

우선 $L_2 - 2 \times L_1 \rightarrow L_2$를 취해줍니다.   
$\begin{bmatrix} 1 & 2 & \vdots & 4  \newline 0 & 1 & \vdots & 1 \end{bmatrix}$   

그 다음으로 $L_1 - 2 \times L_2 \rightarrow L_1$을 취해줍니다.   
$\begin{bmatrix} 0 & 1 & \vdots & 2  \newline 0 & 1 & \vdots & 1 \end{bmatrix}$   
그럼 여기서 $x=2, y=1$을 얻을 수 있습니다. 즉, $\[A \| b \]$에서 A를 항등행렬로 만들어주면 $b$가 $x$의 답이 됩니다.   

### 예시 문제
$\begin{Bmatrix} 2x + y - z= 8   \newline -3x -y + 2z = -11 \newline -2x + y + 2z = -3 \end{Bmatrix}$ &rarr; $\begin{bmatrix} 2 & 1 & -1 \newline -3 & -1 & 2 \newline -2 & 1 & 3 \end{bmatrix} \begin{bmatrix} x  \newline y \newline z \end{bmatrix} = \begin{bmatrix} 8  \newline -11 \newline -3 \end{bmatrix}$   
연립일차방정식을 $A\underline{x}=\underline{b}$의 꼴로 나타냈습니다.   
이제 확장 행렬로 나타내겠습니다.   
$\begin{bmatrix} 2 & 1 & -1 & \| & 8 \newline -3 & -1 & 2 & \| & -11 \newline -2 & 1 & 3 & \| & -3 \end{bmatrix}$   
row의 순서대로 $L_1, L_2, L_3$라고 명시하겠습니다.   

$\frac{3}{2} \times L_1 + L_2 \rightarrow L_2$을 취해줍니다.   
$\begin{bmatrix} 2 & 1 & -1 & \| & 8 \newline 0 & \frac{1}{2} & \frac{1}{2} & \| & 1 \newline -2 & 1 & 3 & \| & -3 \end{bmatrix}$   

$L_1 + L_3 \rightarrow L_3$을 취해줍니다.   
$\begin{bmatrix} 2 & 1 & -1 & \| & 8 \newline 0 & \frac{1}{2} & \frac{1}{2} & \| & 1 \newline 0 & 2 & 1 & \| & 5 \end{bmatrix}$   

$- 4 \times L_2 + L_3 \rightarrow L_3$을 취해줍니다.   
$\begin{bmatrix} 2 & 1 & -1 & \| & 8 \newline 0 & \frac{1}{2} & \frac{1}{2} & \| & 1 \newline 0 & 0 & -1 & \| & 1 \end{bmatrix}$   

이제는 아래에서 위로 취해줍니다. $L_2 + \frac{1}{2} \times L_3 \rightarrow L_2$을 취해줍니다.   
$\begin{bmatrix} 2 & 1 & -1 & \| & 8 \newline 0 & \frac{1}{2} & 0 & \| & \frac{3}{2} \newline 0 & 0 & -1 & \| & 1 \end{bmatrix}$   

$L_1$만 취해주면 항등행렬을 만들어 줄 수 있습니다. $L_1 - L_3 \rightarrow L_1$을 취해줍니다.   
$\begin{bmatrix} 2 & 1 & 0 & \| & 7 \newline 0 & \frac{1}{2} & 0 & \| & \frac{3}{2} \newline 0 & 0 & -1 & \| & 1 \end{bmatrix}$   

$L_1 - 2 \times L_2 \rightarrow L_1$을 취해줍니다.   
$\begin{bmatrix} 2 & 0 & 0 & \| & 4 \newline 0 & \frac{1}{2} & 0 & \| & \frac{3}{2} \newline 0 & 0 & -1 & \| & 1 \end{bmatrix}$   

이제 $\[A \| b \]$에서 $A$를 항등행렬로 바꿔줍니다. $\frac{1}{2} \times L_1 \rightarrow L_1$와 $2 \times L_2 \rightarrow L_2$와 $-1 \times L_3 \rightarrow L_3$을 취해줍니다.   
$\begin{bmatrix} 1 & 0 & 0 & \| & 2 \newline 0 & 1 & 0 & \| & 3 \newline 0 & 0 & 1 & \| & -1 \end{bmatrix}$   

그럼 이제 $x=2 \quad y=3 \quad z=-1$을 얻을 수 있습니다. 