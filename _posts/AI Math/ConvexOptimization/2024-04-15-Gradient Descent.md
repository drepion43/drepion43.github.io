---
title:  "Gradient Descent"
categories: ConvexProblem
tag: [theory, Optimization]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
comments: true
date: 2024-04-15
toc_sticky: true
---

## Gradient Descent
Gradient Descent는 최적화 문제를 풀 수 있는 알고리즘입니다. Gradient Descent는 우선 최적화 문제에서 찾고자 하는 Global Minimum을 찾는 것이 아닌 현재 지점에서의 Local Minimum을 찾는 방법입니다. 즉, initial point의 위치에 따라 가장 가까운 Local Minimum을 찾아주기 때문에, 이 initial point가 매우 중요합니다.    
Graident Descent 현재 point보다 더 작아지는 point로 이동하는 것이 목적입니다. 그럼 여기서 **어느 방향**으로 가야 더 작아지는지와 또 **얼만큼 가야할까**이 핵심이 됩니다.   
여기서 Gradient는 **방향**을 의미하고, Descent가 얼만큼인 **step size**를 의미합니다. 
방향인 Gradient는 그냥 **미분**한 결과인 $\frac{df}{dx}$가 가장 가파른 방향이 됩니다. 그럼 왜 미분한 결과가 가장 가파른지에 대해 알아보겠습니다.   


<span style='color:blue'>**Driection(Gradient)**</span>   
한 번 error 함수인 $(\underline{y} - A \underline{x})^T (\underline{y} - A \underline{x})$을 최적화 하는 문제로 예를 들어 설명해보겠습니다.   
일단 Gradient의 정의는 미분이라고 했습니다. 따라서 $\begin{bmatrix} \frac{\partial f}{\partial x_1} \newline \frac{\partial f}{\partial x_2} \end{bmatrix}$가 됩니다.   
그럼 여기서 가장 가파른 방향으로 가는지를 설명하기 위해 **특정 방향으로의 미분인 Directional Derviative**의 개념을 가져와보겠습니다. 그럼 어떤 방향인 $\underline{u}$로 가는 이분을 살펴보겠습니다.   
\begin{aligned}    
Directional \; Derviative \; :& lim_{h \rightarrow 0 } \frac{f(\underline{x}+h\underline{u}) - f(\underline{x})}{h} \newline   
f(\underline{x}) =& f(\begin{bmatrix} x_1 \newline x_2 \end{bmatrix}) = f(\begin{bmatrix} x_1^{\*} + h u_1 \newline x_2^{\*} + h u_2 \end{bmatrix}) = f(h) \newline
\frac{df}{dh} =& \frac{\partial f}{\partial x_1} \frac{dx_1}{dh} + \frac{\partial f}{\partial x_2} \frac{dx_2}{dh} \newline   
=& \bigtriangledown f \underline{u}
\end{aligned}   

상기의 수식에서 $f(\underline{x})$에서 상수인 $(x_1^{\*}, x_2^{\*})$지점에서의 $\underline{u}$방향으로 가는 함수를 본따서 만들면 $f(h)$의 함수가 됩니다. 이 $f$함수를 h에대 미분을 해보겠습니다. 이 때 미분을 할 때, chain rule을 적용하여 표현하면 상기와 같이 나타내집니다. 그럼 여기서 $\frac{\partial f}{\partial x_1},  \frac{\partial f}{\partial x_2}$는 Gradient가 되고, $\frac{dx_1}{dh}, \frac{dx_2}{dh}$는 $x_1 = x_1^{\*} + hu_1, x_2 =x_2^{\*} + hu_2$에 대해 미분을 하면 $u_1, u_2$가 나오게 됩니다. 즉, $\underline{u} = \begin{bmatrix} u_1 & u_2 \end{bmatrix}$인 지정한 방향이 됩니다. 즉, Gradient와 $\underline{u}$의 방향과의 내적이 그 쪽 방향의 미분이 된다는 뜻입니다. 그럼 정의한 $\underline{u}$를 바꿔가며 어느 방향이 가장 가파른지 알아내보겠습니다. 즉, Gradient와 $\underline{u}$간의 내적값이 가장 커지려면 $\underline{u}$가 어떤 방향이어야 하는지 생각해보겠습니다. $\bigtriangledown f$와 $\underline{u}$사이의 각이 0도 일 때, 즉, $\underline{u}$이 $\bigtriangledown f$의 방향일 때 가장 큰 값을 가지게 됩니다. 따라서 **$\underline{u}$가 Gradient 방향**일 때 가장 커지게 됩니다.   

<br>
<br>
이제 이어서 Gradient Descent에 대해 설명하겠습니다. Gradient Descent의 동작 순서는 하기와 같습니다.   
① Initialize $X_0$   
② $x_{k+1} = x_{k} - \alpha \frac{d f}{d x}$   
③ ②을 반복   

근데 여기서 또 step size인 $\alpha$를 어떻게 설정하냐에 따라 local minimum을 얼마나 빨리 찾는지도 결정납니다. 
<img src="../../../assets/images/ConvexProblem/2024-04-15-Gradient Descent/Gradient Descent 1.jpg" alt="Gradient Descent 1" style="zoom:80%;" />    

### 예시
내가 찾고자 하는 $x$가 있고 이 $x$에 noise $n$이 더해졌다고 가정해보겠습니다. 그래서 얻은 측정값인 $z$를 얻었다고 해보겠습니다. 그럼 해당 수식은 하기와같이 나타낼 수 있습니다. 즉, 우리가 실제로 측정한 값인 $z$를 알고 있는데, 이 $z$는 얻고자 하는 값에 noise가 추가된 값입니다.      
\begin{aligned}    
z = x + n
\end{aligned}   

우리가 측정한 값인 $z$가 만약에 $3.2, 2.8, 3.1, 3.5, 2.9$라고 해보겠습니다. 그럼 대략적으로 $x$값은 3이라고 생각할 수 있습니다. 이 때에 대한 error를 수식으로 나타내면 하기와 같습니다.   
\begin{aligned}    
err = (z - x)^2
\end{aligned}    

근데 여기서 저희는 한 개의 값으로 표현했습니다. 이를 vector로 표현하면 여러개의 값을 표현할 수 있습니다.   
\begin{aligned}    
\underline{z} =& \underline{x} + \underline{n} \newline    
err =& (\underline{z} - \underline{x})^T (\underline{z} - \underline{x}) = ||\underline{z} - \underline{x}||_2^2
\end{aligned}    

상기의 수식에서 조금 더 일반화를 해보겠습니다. 만약 $x$앞에 matrix A가 곱해보겠습니다.   
\begin{aligned}    
\underline{z} =& A\underline{x} + \underline{n} \newline    
err =& (\underline{z} - A\underline{x})^T (\underline{z} - A\underline{x}) = ||\underline{z} - A\underline{x}||_2^2
\end{aligned}    

상기의 err 함수는 목적 함수이면서 Scalar가 됩니다. 우리가 최적화를 하고자 하는 목적 함수는 Scalar이여야 그 크기를 논할 수 있어, 값을 최소화하거나 최대화를 할 수 있습니다.    
Gradient는 미분이니 해당 Scalr를 vector로 미분해야합니다. 그 내용은 [vector 미분](https://drepion43.github.io/mathematicalconcepts/Derivation/)를 참고하시면 될 것 입니다.    
따라서 err 함수인 $f$의 변화량을 구해서 $dx$앞에 있는 것이 미분이 됩니다.   
\begin{aligned}    
df =& \frac{\partial f}{\partial \underline{x}^T} d\underline{x} \newline   
=& (-Ad\underline{x})^T (\underline{z} - A \underline{x}) - (\underline{z} - A \underline{x})^T A d\underline{x} \newline   
=& -2(\underline{z} - A \underline{x})^T A d\underline{x}
\end{aligned}    

즉, Gradient는 $d\underline{x}$앞에 있는 $-2(\underline{z} - A \underline{x})^T A$인데, 이는 $\frac{\partial f}{\partial \underline{x}^T}$인 tranpose가 들어가 있습니다. 하지만 우리가 알고 싶은 것은 $\frac{\partial f}{\partial \underline{x}}$가 되니 이를 다시 tranpose를 취해주면 우리가 구하고 싶은 $\frac{\partial f}{\partial \underline{x}}$을 얻을 수 있습니다.   

그럼 최종적으로 Gradient를 구해보겠습니다.   
\begin{aligned}    
x_{k+1} =& x_k - \alpha \frac{df}{dx} \newline   
\underline{x_{k+1}} =& \underline{x_k} + 2 \alpha A^T (\underline{z} - A \underline{x_k})
\end{aligned}    

