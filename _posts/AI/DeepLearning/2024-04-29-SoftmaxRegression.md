---
title:  "Softmax Regression"
categories: DeepLearning
tag: [theory, AI, DeepLearning]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: 
comments: true
date: 2024-04-29
toc_sticky: true
---

## Softmax Function
이전에 Sigmoid Function에 대해 알아보았습니다. 우선 Sigmoid Function의 경우에는 x값이 조금만 커져도 1에 매우 근접하게 수렴하거나 x값이 조금만 작아져도 0으로 근접하게 수렴하게 됩니다. 그리고 출력이 scalr인 하나만을 나타내기 때문에 상대적인 평가가 불가능하다는 단점이 존재했습니다. 따라서 강아지이다 아니다인 binary로만의 평가만 가능했었습니다.     
<img src="../../../assets/images/DeepLearning/2024-04-29-SoftmaxRegression/softmax function.png" alt="softmax function" style="zoom:80%;" />    
\begin{aligned} 
y_i = \frac{e^{x_i}}{\sum_j^K e^{x_j}}
\end{aligned}   

상기의 수식은 softmax function이며 전체 K차원의 vector에서 i번째의 확률의 값을 의미합니다. 예를들어 $K=3$이라고 가정을 해보겠습니다.    
\begin{aligned} 
softmax = \begin{bmatrix} \frac{e^{x_1}}{\sum_j^3 e^{x_j}} & \frac{e^{x_2}}{\sum_j^3 e^{x_j}} & \frac{e^{x_3}}{\sum_j^3 e^{x_j}} \end{bmatrix} 
\end{aligned}   

상기와 같이 이렇게 각각의 label에대한 확률의 출력값을 나타내게 됩니다. 따라서, 이전 sigmoid와 다르게 출력이 vector형태가 되며 Neural Network에서도 들어온 입력 노드 개수 그대로가 출력 노드 개수로 나타내지게 됩니다.    
그럼 이제 softmax function의 장점에 대해 설명하겠습니다. 우선, 위의 그래프 사진에서도 볼 수 있듯이 softmax는 sigmoid와 다르게 값이 커져도 1이나 0으로 빠르게 근접하는 saturation 문제가 없습니다. 또한, 분모에 전체에 대한 값이 들어가 있기 때문에 각 label에 대한 확률이 상대적인 평가를 내포하고 있습니다. 그리고 exponention을 취해주어 해당 확률에 대한 값이 더욱더 두드러지게 나타납니다.    

## Softmax Regression
이전에는 Logistic Regression에 대해 알아보았습니다. 우선 Logistic Regression은 binary classification으로 강아지이다 아니다에 대한 확률만을 구할 수 있다고 말씀드렸습니다. 그럼 여러가지 동물에 대한 분류의 경우에는 어떻게 수행하면 될까요? 그 답이 바로 Softmax Regression입니다. 이전에 Logistic Regression의 출력은 0.8 또는 0.2 등과 같이  Scalar값이었습니다. 따라서 강아지일 확률에 대한 의미만 나타내게 되었습니다. 하지만, 만약 출력이 vector로 나타나진다면 어떻게 될까요? 만약 강아지, 고양이, 개구리, 말에 대한 확률을 한번에 vector로 $\begin{bmatrix} 0.1 & 0.2 & 0.5 & 0.2 \end{bmatrix}$로 나타냈다면 이 때 사진은 개구리일 확률이 0.5로 가장 높으니 개구리일 거라고 예측을 할 수 있게 될 것입니다.  

### Loss Function
우선 Softmax Regression의 Loss함수는 **Cross-Entropy**라는 함수를 이용합니다. 우선 해당 수식부터 알아보면 하기와 같습니다.   
\begin{aligned} 
BCE = - \sum_i p_i log(q_i)
\end{aligned}   

상기의 수식에서 $p$는 정답인 확률 즉, 내가 만들어내고 싶은 확률입니다. 하지만 $q$는 내가 계산해서 얻은 확률을 의미합니다. 즉, $q$를 최대한 $p$와 근사하게 하는 것이 학습의 목표가 됩니다. 그럼 당연히 BCE또한 최소화가 될 것 입니다. 그럼 여기서 $q$가 내가 Neural Network를 통과해서 얻은 즉, Softmax function을 통과해서 얻은 vector가 된다는 의미입니다. 여기서 내가 원하는 $p$는 만약 강아지를 원한다면 $\begin{bmatrix} 1 & 0 & 0 & 0 \end{bmatrix}$, 개구리를 원한다면 $\begin{bmatrix} 0 & 0 & 1 & 0 \end{bmatrix}$이 될 것 입니다. 근데 여기서 $p_i$는 강아지일 때만 1이고 나머지는 모두 0으로 되어 곱해지면 나머지에 대한 것은 사라지게됩니다. 따라서 $- \sum_i p_i log(q_i)=log(q_k)$만 살아남게 됩니다. 여기서 $k$는 강아지일 때 0, 고양이일 때 1, 개구리일 때 2, 말 일때 3을 의미합니다. 따라서 모든 데이터들에 대한 확률의 가능도를 최소화하는 것이 목적이기 때문에 최종적으로 $Loss = - sum_j log(q_j)$을 얻을 수 있습니다.