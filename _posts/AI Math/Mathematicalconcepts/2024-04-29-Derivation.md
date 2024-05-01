---
title:  "vector 미분"
categories: MathematicalConcepts
tag: [theory, math]
tags: [Jekyll, MathJax]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: scalr to vector 미분, vector to vector 미분
comments: true
date: 2024-04-29
toc_sticky: true
---


## Scalr to Vector 미분
이번 절에서는 Scalr를 Matrix로 미분하는 방법에 대해 알아보겠습니다.   
입력이 vector이고 출력이 Scalar인 경우에 출력을 입력으로 미분하는 경우를 하기를 통해 알아보겠습니다.   
\begin{aligned} 
f(\begin{bmatrix} x_1 \newline x_2 \end{bmatrix}) =& f(\underline{x}) = f(x_1, x_2) \newline   
\frac{\partial f}{\partial \underline{x}^T} \triangleq& \begin{bmatrix} \frac{\partial f}{\partial x_1 } & \frac{\partial f}{\partial x_2} \end{bmatrix}
\end{aligned}  

미분의 정의는 상기와 같이 $\frac{\partial f}{\partial \underline{x}} \triangleq \begin{bmatrix} \frac{\partial f}{\partial x_1 } & \frac{\partial f}{\partial x_2} \end{bmatrix}$ 정의됩니다. 근데 만약 $\underline{x}$의 element가 많아지면서 식이 길어진다면, 그 만큼 미분을 해야하는 연산이 길어지며 시간이 오래걸리게됩니다. 이를 쉽게 해결하는 방법에 대해 알아보겠습니다.   

한번 $df$에 대해 생각해보겠습니다. $df$는 표기 그대로 $f$의 변화량을 뜻합니다. 즉, $df = f(\underline{x} + d\underline{x}) - f(\underline{x}), d\underline{x} = \begin{bmatrix} x_1 \newline x_2 \end{bmatrix}$입니다. 이를 편미분 개념으로 생각을 해보면 $x_1, x_2$에 대해 편미분을 수행한 결과를 vector로 쌓은 것입니다.    
\begin{aligned} 
df =& f(\underline{x} + d\underline{x}) - f(\underline{x}), d\underline{x} = \begin{bmatrix} x_1 \newline x_2 \end{bmatrix} \newline   
=& \frac{\partial f}{\partial x_1} dx_1 + \frac{\partial f}{\partial x_2} dx_2 \newline   
=& \begin{bmatrix} \frac{\partial f}{\partial x_1} & \frac{\partial f}{\partial x_2} \end{bmatrix} \begin{bmatrix} dx_1 \newline dx_2 \end{bmatrix} \newline   
=& \frac{\partial f}{\partial \underline{x}^T} d\underline{x}
\end{aligned}  

상기의 수식에서 $df$에 대한 식을 정의한 후, 이를 vector의 내적연산으로 바꿔서 표현했습니다. 그럼 $df = \frac{\partial f}{\partial \underline{x}^T} d\underline{x}$와 같은 결과를 얻을 수 있습니다. 즉, 최종적으로 우리가 알고자 하는 것은 Scalar를 vector로 미분한 것인 $\frac{\partial f}{\partial \underline{x}^T} \triangleq \begin{bmatrix} \frac{\partial f}{\partial x_1 } & \frac{\partial f}{\partial x_2} \end{bmatrix}$입니다. **Scalar를 vector로 미분한 것은 $df$를 구했을 때, $d\underline{x}$앞에 있는 것**입니다. 즉, 만약 한개의 vector에 대해 미분한 것은 $df = \frac{df}{dx} dx$가 되어 $df$를 구할 수 있습니다.    

참고로 $dx^2$의 경우에는 $x$를 0으로 보내기 때문에 $\frac{f(x + dx) - f(x)}{dx}$이 0이 됩니다. 따라서, 1차식에 대해서만 고려를 하기 위해 **$df = f.o(\frac{\partial f}{\partial \underline{x}^T} d\underline{x})$와 같이 first order**를 취해줍니다. 제곱 term은 vector로 표현하면 자기 자신과이 내적이기 때문에 $d\underline{x}^T d\underline{x}$가 됩니다.    

이번에 간단한 linear regression 문제의 예시를 통해 vector 미분을 해보겠습니다.   
\begin{aligned}   
f(\underline{x}) =& (\underline{y} - A \underline{x})^T(\underline{y} - A \underline{x}) \newline   
df =& frac{\partial f}{\partial \underline{x}^T} d\underline{x} \newline   
=& (\underline{y} - A (\underline{x} + d\underline{x}))^T(\underline{y} - A (\underline{x} + d\underline{x}) - (\underline{y} - A \underline{x})^T(\underline{y} - A \underline{x})\newline   
=& ((\underline{y} - A\underline{x}) - Ad\underline{x})^T ((\underline{y} - A\underline{x}) - Ad\underline{x}) - (\underline{y} - A\underline{x})^T (\underline{y} - A\underline{x}) \newline   
=& -(\underline{y} - A\underline{x})^T A d\underline{x} - d\underline{x}^T A^T (\underline{y} - A \underline{x}) \newline   
=& -2(\underline{y} - A\underline{x})^T A d\underline{x} \newline   
=& \frac{\partial f}{\partial \underline{x}^T} d\underline{x}
\end{aligned}  

이전에 제곱 term의 경우에는 $x$가 0으로가니 무시를 한다고 했습니다. 즉, $d\underline{x}^T A^TA d\underline{x}$가 붙은 경우 0으로 가니 상기의 수식에서 표기를 하지 않았습니다. 이렇게 소거할 것들을 소거한 후 나온 수식이 $-(\underline{y} - A\underline{x})^T A d\underline{x} - d\underline{x}^T A^T (\underline{y} - A \underline{x})$이 됩니다. 여기서 $d\underline{x}^T A^T (\underline{y} - A \underline{x})$의 경우, $\underline{x}$를 n x 1 vector라고 생각을 해보면, $\underline{x}^T$ : 1 x n , $A^T$ : m x n, $(\underline{y} - A \underline{x})$ : n x 1 의 vector 연산으로 최종적으로 scalar의 결과를 얻게됩니다. Scalar에는 Tranpose를 취해줘도 같은 결과가 나오기 때문에, 취해주면 앞의 수식과 같은 결과가 나와 묶어주게됩니다. 따라서 최종적으로 Least Squares의 수식인 $-2(\underline{y} - A\underline{x})^T A d\underline{x}$을 얻을 수 있습니다.   

최종적으로 vector를 편미분해서 결과를 얻으나 $-2(\underline{y} - A\underline{x})^T A$를 통해 얻을 결과나 동일합니다. 하지만, 직접 편미분을 해서 하면 많은 시간과 노력이 들기 때문에, 다음과 같이 쉽게 얻을 수 있습니다.   

추가적으로 vector의 미분에서도 우리가 알고 있는 편미분 개념들이 적용 가능합니다. 즉, 곱의 미분에서 앞미그와 그뒤미를 적용할 수 있습니다.    
즉, $(\underline{y} - A \underline{x})^T(\underline{y} - A \underline{x})$를 미분하면, $(-A\underline{x})^T(\underline{y} - A\underline{x}) - (\underline{y} - A \underline{x})^T A d\underline{x} = -2(\underline{y} - A\underline{x})^T A d\underline{x}$를 얻을 수 있습니다.    

## vector를 vector로 미분
방금까지 scalr를 vector로 간단하게 미분하는 법에 대해 알아보았습니다. scalr를 vector로 미분을 하면 최종적으로 vetor의 결과를 얻었습니다. 그럼 이 얻은 vector를 또 vector로 미분을 하는 일이 발생하기도 합니다. 그럼 이 결과는 미리 말씀드리면, Matrix가 나옵니다. 또 이를 쉽게 구하는 방법에 대해 이번 절에서 알아보겠습니다.   
이전에는 $f$가 scalar였습니다. 그럼 이번에는 $\underline{f}$가 vector라고 생각해보겠습니다. 그럼 하기와같이 정의할 수 있습니다.   
\begin{aligned}    
\underline{f} = \begin{bmatrix} f_1 \newline f_2 \end{bmatrix} \newline   
\frac{\partial f_1}{\partial \underline{x}^T} \triangleq& \begin{bmatrix} \frac{\partial f_1}{\partial x_1 } & \frac{\partial f_1}{\partial x_2} \end{bmatrix} \newline   
\frac{\partial f_2}{\partial \underline{x}^T} \triangleq& \begin{bmatrix} \frac{\partial f_2}{\partial x_1 } & \frac{\partial f_2}{\partial x_2} \end{bmatrix} \newline   
\frac{\partial \underline{f}}{\partial \underline{x}^T} \triangleq& \begin{bmatrix} \frac{\partial f_1}{\partial \underline{x}} newline \frac{\partial f_2}{\partial \underline{x}} \end{bmatrix}    
\end{aligned}  

그럼 이제 scalar를 vector로 미분할 때처럼, $df$를 정의해보겠습니다. 여기서 $f$가 vector이니 $d\underline{f}$를 정의해보겠습니다.   
\begin{aligned} 
d\underline{f} =& \begin{bmatrix} f_1 \newline f_2 \end{bmatrix} \newline   
=& \begin{bmatrix} \frac{\partial f_1}{\partial x_1} dx_1 + \frac{\partial f_1}{\partial x_2} dx_2 \newline \frac{\partial f_2}{\partial x_1} dx_1 + \frac{\partial f_2}{\partial x_2} dx_2 \end{bmatrix} \newline   
=& \begin{bmatrix} \frac{\partial f_1}{\partial x_1} dx_1 & \frac{\partial f_1}{\partial x_2} dx_2 \newline \frac{\partial f_2}{\partial x_1} dx_1 & \frac{\partial f_2}{\partial x_2} dx_2 \end{bmatrix} \begin{bmatrix} dx_1 \newline dx_2 \end{bmatrix} \newline   
=& \frac{\partial \underline{f}}{\partial \underline{x}^T} d \underline{x}    
\end{aligned}     

그럼 linear regression에서 scalar to vector 미분한 결과가 vector로 나왔습니다. 이를 또 다시 vector로 미분해보겠습니다. 여기서 이 결과를 2번 미분한 Matrix인 **Hessian Matrix**라고 부릅니다.    
$-2(\underline{y} - A\underline{x})^T A$는 행 vector가 됩니다. vector를 vector로 미분하기 위해서는 열 vector여야 하기 때문에, 이를 Tranpose를 취해주겠습니다. ($(-2(\underline{y} - A\underline{x})^T A)^T = -2 A^T (\underline{y} - A\underline{x})$)    
\begin{aligned}    
\underline{f} \triangleq& -2 A^T (\underline{y} - A\underline{x}) \newline   
d\underline{f} =& f.o(\underline{f}(\underline{x} + d\underline{x}) - \underline{f}(\underline{x})) \newline    
=& -2 A^T (\underline{y} - A(\underline{x} + d\underline{x})) + 2 A^T (\underline{y} - A\underline{x}) \newline   
=& 2 A^T A d\underline{x} \newline   
=& \frac{\partial \underline{f}}{\partial \underline{x}^T} d\underline{x}   
\end{aligned}   

## Scalr to Matrix 미분
이번에는 Scalr를 Matrix로 미분하는 법에 대해 알아보겠습니다. 즉, 출력이 scalar인데, 입력이 matrix인 경우 이를 어떻게 미분하는지에 대해 알아보는 것입니다.   
\begin{aligned} 
f(\begin{bmatrix} x_{11} $ x_{12} \newline x_{21} & x_{22} \end{bmatrix}) =& f(X) = f(x_{11}, x_{12}, x_{21}, x_{22}) \newline   
df =& \frac{\partial f}{\partial x_{11}} dx_{11} + \frac{\partial f}{\partial x_{12}} dx_{12} + \frac{\partial f}{\partial x_{21}} dx_{21} + \frac{\partial f}{\partial x_{22}} dx_{22} \newline   
\frac{\partial f}{\partial X} \triangleq& \begin{bmatrix} \frac{\partial f}{\partial x_{11}} & \frac{\partial f}{\partial x_{12}} \newline \frac{\partial f}{\partial x_{21}} & \frac{\partial f}{\partial x_{22}} \end{bmatrix}   
\end{aligned}   

여기서 $\partial X = \begin{bmatrix} dx_{11} & dx_{12} \newline dx_{21} & dx_{22} \end{bmatrix}$입니다.  그럼 여기서 $df$를 $\frac{\partial f}{\partial X} dX$형태로 나타내보겠습니다. 이런 꼴라 나타내주기 위해서는 $\frac{\partial f}{\partial X}$에 **tranpose**를 취해주고 최종 결과는 **trace**를 통해 구해주면 $df$와 같은 결과를 얻을 수 있습니다.    
\begin{aligned} 
df =& \frac{\partial f}{\partial x_{11}} dx_{11} + \frac{\partial f}{\partial x_{12}} dx_{12} + \frac{\partial f}{\partial x_{21}} dx_{21} + \frac{\partial f}{\partial x_{22}} dx_{22} \newline    
=& tr(\begin{bmatrix} \frac{\partial f}{\partial x_{11}} & \frac{\partial f}{\partial x_{12}} \newline \frac{\partial f}{\partial x_{21}} & \frac{\partial f}{\partial x_{22}} \end{bmatrix}^T \begin{bmatrix} dx_{11} $ dx_{12} \newline dx_{21} & dx_{22} \end{bmatrix}) \newline   
=& tr(\frac{\partial f}{\partial X^T} dX)
\end{aligned}   

