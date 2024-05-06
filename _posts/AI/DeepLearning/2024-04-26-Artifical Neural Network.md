---
title:  "ANN(Artificial Neural Network)"
categories: DeepLearning
tag: [theory, AI, DeepLearning]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: Linear란?, Neural Network의 정의
comments: true
date: 2024-04-26
toc_sticky: true
---

## Linear란?
linear를 해석해보면 선형이라는 의미를 가집니다. 그럼 linear한 수식들은 어떤 것일까요? linear 수식의 특징은 **중첩의 원리(superposition principle)**을 적용할 수 있다는 것입니다. 그럼 중첩의 원리가 어떤 것인지 궁금할 것 입니다. 예전 신호 및 시스템에서 LTI 시스템을 배우신 기억이 있으실 겁니다. 여기서 Linearity는 **Scaling과 Additivity**를 모두 만족해야하며 이 두가지를 합쳐서 **superposition**이라고 부른다고 했습니다. 즉, scaling과 addivity를 만족해야 해당 수식을 linear하다고 할 수 있습니다. 그럼 가장 간단한 예제인 $y=ax$의 식에 대해 생각해보겠습니다. 이 함수 $x$에 대해서 linear합니다. $y_1 = ax_1$과 $y_2=ax_2$에서 $y_1 + y_2 = ax_1 + ax_2$를 만족하는 것을 바로 알 수 있기 때문입니다. 이것이 scaling과 additivity를 모두 만족한 가장 쉬운 예시입니다. 그럼 반대로 이 함수에서의 parameter들에 대해 linear하다고 할 수 있을 까요?   
<br>
이번에는 다른 예시인 $y=ax^2 + bx + c$인 2차 함수를 들어 설명해보겠습니다. 먼저 결론부터 말씀드리면 이 2차함수는 parameter들에 대해 linear합니다. 여기서 parameter는 $a,b,c$가 됩니다. 즉, 가능도와 같이 여러개의 $x_1,x_2,x_3...$에 대해 $a,b,c$는 linear하다는 의미가 됩니다.ㅣ만약에 상기의 2차 함수를 선형대수로 표현하면 하기와 같이 표현할 수 있습니다.    
\begin{aligned} 
\begin{bmatrix} y_1 \newline y_2 \newline y_3 \newline \vdots \end{bmatrix} = 
\begin{bmatrix} x_1^2 & x_1 & 1 \newline x_2^2 & x_2 & 1 \newline x_3^2 & x_3 & 1 \newline & \vdots & \end{bmatrix} 
\begin{bmatrix} a \newline b \newline c \end{bmatrix}
\end{aligned}    

## Neural Network
실제 사람의 신경 세포를 모방하여 단순화하여 만든 네트워크 구조가 바로 Neural Network입니다. 사람의 신경세포를 여러 다발로 구성되어 있는데, 이 신경세포들 간의 자극 전달은 축삭돌기 끝의 시냅스라는 틈을 통해 신경전달물질을 분비하여 전달하는 식으로 자극을 다음 신경세포로 전달합니다. 이런 구조를 컴퓨터 네트워크에서 모방하여 만든 것이 Nerual Network입니다.   

### Linear Regression
키와 몸무게에 대한 linear regression을 구해보는 예제를 들어 설명하겠습니다.   
<img src="../../../assets/images/DeepLearning/2024-04-26-Artifical Neural Network/Linear Regression 1.jpg" alt="Linear Regression 1" style="zoom:80%;" />    
상기의 그림에서 x는 키를 나타내며 y는 몸무게를 의미합니다. 여기서 우리가 구하고자 하는 것은 parameter인 $a,b$가 됩니다. 현재 여려명의 키와 몸무게인 $(x_i,y_i)$가 주어진 데이터라고 가정을 하겠습니다. 상기의 파란색 선인 키와 몸무게에 대한 linear regression이 되는데 이를 신경망으로 표현하면 하기와 같이 나타낼 수 있습니다.   
<img src="../../../assets/images/DeepLearning/2024-04-26-Artifical Neural Network/Linear Regression 2.jpg" alt="Linear Regression 2" style="zoom:80%;" />    
즉, 상기의 신경망 구조는 하기의 linear regression을 벡터로 표현한것과 동일한 것입니다.   
\begin{aligned} 
\begin{bmatrix} y_1 \newline y_2 \newline y_3 \newline \vdots \end{bmatrix} = 
\begin{bmatrix} x_1 & 1 \newline x_2 & 1 \newline x_3 & 1 \newline \vdots \end{bmatrix} 
\begin{bmatrix} a \newline b \end{bmatrix}
\end{aligned}    

즉, $(a,b)$의 어떤 값이 Object function(Cost function)을 최소화하도록 잘 조정해주면 된다는 의미입니다. 가장 간단한 cost function인 MSE를 표현하면 $er=\sum_i (y_i - (ax_i + b))^2$이 됩니다. 여기서 $a,b$ 부분을 Nerual Network에서는 weight라고 부르며 이 연산을 weighted sum(곱하고 더하기)라고 부릅니다. 그래서 해당 weight들의 표기는 $a=w_1, b=w_2$로 표기합니다. 그리고 이 $(1, x_1),(w_1,w_2)$에서 나온 결과들이 또 다시 신경 세포로 들어가는 구조를 가진다면 다층을 이루게됩니다. 그래서 이 때의 한 층을 한 개의 layer라고 부릅니다. 

<br>   

Neurla Network 구조를 이용하면 우리가 알고 있던 Non-linear 함수의 대부분을 표현할 수 있습니다. 이전에 말했던 parameter에 대해서는 linear하면 된다는 의미가 바로 이 의미가 됩니다. 따라서 똑같이 2차 함수도 상기의 Nerual Network 구조로 표현할 수 있게됩니다. 

### Deep Nerual Network
한번 이전에 배웠던 Nerual Network 구조에서 layer 층이 더 깊게 나타내보겠습니다.    
<img src="../../../assets/images/DeepLearning/2024-04-26-Artifical Neural Network/NN 1.jpg" alt="NN 1" style="zoom:80%;" />    
상기의 신경망 구조를 수식으로 표현하면 하기와 같이 나타낼 수 있습니다.   
\begin{aligned} 
w_1^{(1)} + w_2^{(1)}x_1 =& y^{(1)} \newline    
w_1^{(2)} + w_2^{(2)}y^{(1)} =& y^{(2)} \newline    
w_1^{(3)} + w_2^{(3)}y^{(2)} =& y^{(3)} \newline    
\end{aligned}    

즉, layer층이 더 깊어진다고 해서 Non-linear를 표현하지는 못하고 이전과 같이 Linear만을 표현을 할 수 밖에 없어집니다. 그럼 Non-linear를 표현하고 싶다면 어떻게 해야할까요?   
결론부터 말씀드리면 <span sytle='color:red'>**Activation Function**</span>을 추가한다면 Non-linear 함수 또한 나타낼 수 있게됩니다. 이전에는 1개의 layer 층에서 나온 출력을 그대로 다음 입력으로 전달했습니다. 즉, 이전에는 activation function이 그대로 내뱉는 $y=x$함수였다고 생각하시면 이해가 편하실 겁니다. $y=x$는 아주 일반적인 linear함수이기 때문에 이 activation function을 이용했기 때문에 Non-linear 함수를 표현할 수가 없었습니다.    
그럼 이전에는 activation function을 $y=x$를 썼을 경우에는 **Linear Regression**이라고 불렀습니다. 그럼 activation function을 **step function**을 이용한다면 **Perceptron**이 됩니다. 또한 activation function을 **sigmoid function**을 이용하면 **Logistic Regression**이라고 부릅니다.    
<img src="../../../assets/images/DeepLearning/2024-04-26-Artifical Neural Network/activation function 1.jpg" alt="activation function 1" style="zoom:80%;" />    
