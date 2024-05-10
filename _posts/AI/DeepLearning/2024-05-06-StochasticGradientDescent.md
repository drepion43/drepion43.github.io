---
title:  "SGD(Stochastic Gradient Descent)"
categories: DeepLearning
tag: [theory, AI, DeepLearning]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: SGD란, GD와 SGD차이
comments: true
date: 2024-05-06
toc_sticky: true
---

## SGD(Stochastic Gradient Descent)와 GD(Gradient Descent)
<img src="../../../assets/images/DeepLearning/2024-05-06-StochasticGradientDescent/DNN 1.jpg" alt="DNN 1" style="zoom:80%;" />    
이전절인 BackPropagation에서 설명했던 간단한 DNN구조를 가지고 설명을 하겠습니다.   
상기의 그림에서의 Error 함수(Loss 함수)와 업데이트될 weight는 하기와 같이 나타낼 수 있습니다.   
\begin{aligned}    
f =& (x_1^{(3)} - y_1)^2 + (x_2^{(3)} - y_2)^2 \newline   
w_{ij}^{(l)} \leftarrow& w_{ij}^{(l)} - \alpha \frac{\partial f}{\partial w_{ij}^{(l)}} 
\end{aligned}   

<br>
<span style='color:blue'>**Gradient Descent**</span>   
우선 Gradient Descent에 대해 먼저 설명하겠습니다. 상기의 수식은 1개의 데이터에 대한 Error 함수이며 이 1개의 데이터에 대한 weight를 업데이트하는 것입니다. 하지만, 실제로 Input으로는 수많은 데이터가 들어옵니다. 즉, 1번째 데이터에 대한 Error 함수를 $f_1$이라고 한다면, $f_2,f_3,..., f_n$들과 같이 수많은 Error 함수들에 대해 계산을 한 후, 이에 대한 최소화를 할 수 있는 방향인 Gradient방향으로 weight를 업데이트해줘야합니다. 즉, 하기와 같이 구해지는 것이 Gradient Descent입니다.   
\begin{aligned}    
f_i =& (x_{i1}^{(3)} - y_{i1})^2 + (x_{i2}^{(3)} - y_{i2})^2 \newline   
f =& \sum_i f_i \newline   
w_{ij}^{(l)} \leftarrow& w_{ij}^{(l)} - \alpha \sum_i \frac{\partial f_i}{\partial w_{ij}^{(l)}} 
\end{aligned}   

즉, Gradient Descent를 이용하려면 만약 데이터가 1만개라고 한다면, 1만개의 데이터를 다 이용해서 계산한 후 weight를 업데이트하는 방식으로 수행이 되어집니다. 이 경우, 현재 데이터가 1만개라고만 가정을 해도 엄청난 연산량이 요구되는데, 만약 데이터가 기하급수적으로 증가한다면 이 연산을 수행하는 것은 거의 불가능하다고해도 무방해집니다. 즉, **주어진 데이터 모두를 이용하여 Error함수를 게산한 후 모든 데이터에 대해 Error를 최소화하는 방향으로 weight를 업데이트**하게됩니다. 따라서 이런 Gradient Descent의 문제를 해결하기 위해 SGD(Stochastic Gradient Descent)의 알고리즘을 이용하게 되었습니다.   
<br>   

<span style='color:blue'>**Stochastic Gradient Descent**</span>   
SGD는 우선 모든 데이터들에 대해 계산을 한 후 weight를 업데이트하는 것이 아닌, **우선 1개의 데이터만을 이용하여 Error함수를 구한 후, 1개의 데이터를 구한 이 Error함수가 최소가 되는 방향으로 우선 weight를 업데이트 구하는 방법**입니다. 하기와 같이 구해지는 것이 SGD입니다.    
\begin{aligned}    
f_1 =& (x_{1}^{(3)} - y_{1})^2 + (x_{2}^{(3)} - y_{2})^2 \newline   
w_{ij}^{(l)} \leftarrow& w_{ij}^{(l)} - \alpha \frac{\partial f_1}{\partial w_{ij}^{(l)}} \newline   
f_2 =& (x_{1}^{(3)} - y_{1})^2 + (x_{2}^{(3)} - y_{2})^2 \newline   
w_{ij}^{(l)} \leftarrow& w_{ij}^{(l)} - \alpha \frac{\partial f_2}{\partial w_{ij}^{(l)}} \newline   
\vdots
\end{aligned}   

상기의 수식에서 볼 수 있듯이, 우선 첫번째 데이터를 이용하여 $f_1$을 구한 후, 이 $f_1$을 최소화할 수 있는 방향으로 weight를 업데이트 해줍니다. 그 후, 업데이트한 weight를 가지고 다시 두번째 데이터를 이용하여 $f_2$를 구해준 후, 똑같이 $f_2$를 최소활 수 있는 방향으로 weight를 업데이트 해줍니다. 이 작업을 모든 데이터에 대해 반복해주는 것이 SGD입니다.    
데이터가 너무 많을 때 SGD가 효과적이며, 또한 Online Learning에서도 좋은 효과를 나타냅니다. 그리고 SGD는 한개의 데이터에 대해 계속해서 weight를 업데이트해주니 Local Minimum에서도 빠져나올 수 있는 장점이 생깁니다.   
<br>   
이전에 했었던 Linear Regression문제의 예시를 들어 설명해보겠습니다.    
<img src="../../../assets/images/DeepLearning/2024-05-06-StochasticGradientDescent/GD 1.jpg" alt="GD" style="zoom:80%;" />    
우선 상기의 그림은 Gradient Descent에 대한 설명입니다. GD는 보이는 6개의 데이터들에 대해 모든 Error(MSE) 값을 구한 후, 이 값을 최소화하는 방향으로 weight를 업데이트(a, b 업데이트)합니다.   
<img src="../../../assets/images/DeepLearning/2024-05-06-StochasticGradientDescent/SGD 1.jpg" alt="SGD" style="zoom:80%;" />    
상기의 이미지는 Stochastic Gradient Descent의 경우입니다. SGD는 GD와 다르게 우선 1번째 데이터인 보라색 데이터에 대해서만 고려하여 weight를 업데이트(a, b 업데이트)합니다. 그럼 우선 보라색 데이터에 대해서 weight를 업데이트한 상태인데, 2번째 데이터인 빨간색 데이터에 대해서 다시 weight를 업데이트하게 됩니다. 이런 방식으로 1개의 데이터를 고려하여 총 n번 동작하여 모든 데이터에 대해 weight를 fitting하게 됩니다. 여기서 $\alpha$값에 따라 변화의 크기가 조절되니, 이 $\alpha$값을 잘 조정해줘야합니다.    
<img src="../../../assets/images/DeepLearning/2024-05-06-StochasticGradientDescent/GD SGD.jpg" alt="GD SGD" style="zoom:80%;" />    
상기의 이미지는 GD와 SGD가 Error를 최소화하기 위해 가는 방향을 나타낸 것입니다. 이미지에서 볼 수 있듯이, GD는 바로 Error를 최소화는 방향으로 쭉 이어나가지만, SGD는 왔다갔다 하는 것을 볼 수 있습니다. GD는 모든 데이터에 대해 고려하기 때문에, 상기와 같은 방향으로 갑니다. 하지만, 모든 데이터에 대해 고려해야하기 때문에 **속도가 느리며, Local Minimum에 빠지면 나오질 못하게됩니다.** 하지만, SGD는 1개의 데이터에 대해서만 고려한 방향으로 이동하기 때문에 상기와 같은 들쭉날쭉한 방향으로 이동합니다. 근데 여기서 들쭉날쭉한 방향이라도 각각의 데이터에 대해서는 Error를 최소화하는 방향입니다. 1개의 데이터에 대해서만 계속해서 고려해서 이동하기 때문에 **속도도 GD보다 빠르며, Local Minimum을 빠져나올 수 있습니다.**

<br>
<span style='color:blue'>**Mini-batch Stochastic Gradient Descent**</span>   
그럼 이제 SGD가 어떤 것인지 이해하셨을 거라고 생각합니다. SGD는 1개의 데이터에 대해 수행하니, 데이터가 많을 경우, 이 반복작업을 많이 수행해야하는 문제점이 발생합니다. 즉, 데이터가 1만개라고 한다면, 1만번의 weight 업데이트가 일어나게 됩니다. 이런 반복을 줄이기 위해 Mini-batch SGD가 등장하게됩니다. 기존 SGD와 GD를 섞은 방법이며, 1개의 데이터에 대해 weight 업데이트를 수행하는 것이 아닌, 설정한 특정 몇개(batch)만큼의 데이터를 가지고 weight를 업데이트하는 방법입니다.    
<br>   
정리를 해보겠습니다.   
GD는 모든 데이터에 대해 Error를 구한 후, 이 Error가 최소화될 수 있는 방향으로 weight를 업데이트합니다.   
SGD는 1개의 데이터에 대해 Error를 구한 후, weight를 업데이트한 후, 다시 업데이트한 weight를 가지고 2번째 데이터에 대해서도 weight를 fitting하는 작업을 수행합니다. 이 작업을 모든 데이터에 대해 반복수행됩니다.   
Mini-batch SGD는 SGD에서는 1개의 데이터에 대해 수행한 것을 설정한 batch size만큼의 데이터를 가지고 수행합니다.    
