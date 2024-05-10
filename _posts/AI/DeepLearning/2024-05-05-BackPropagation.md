---
title:  "역전파(BackPropagation)"
categories: DeepLearning
tag: [theory, AI, DeepLearning]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: DNN구조와 DNN 연산 방법
comments: true
date: 2024-05-05
toc_sticky: true
---

## Deep Neural Network
Deep Neural Network는 보통 이전에 배웠던 Neural Network 구조에서 Hidden Layer가 2개 이상인 구조를 가지는 Network를 Deep Neural Network라고 부릅니다. Input Layer $\rightarrow$ Hidden Layers $\rightarrow$ Output Layer의 구조를 가집니다. 여기서 각 Hidden Layer의 Node들의 개수는 직접 설정할 수 있는 하이퍼 파라미터입니다.   
<img src="../../../assets/images/DeepLearning/2024-05-05-BackPropagation/DNN 1.jpg" alt="DNN 1" style="zoom:80%;" />    
상기의 이미지는 매우 간단한 DNN 모델 구조입니다. 이미지에서도 보이듯이 매우 많은 Weight들이 존재합니다. 그럼 이 예측값에 대한 Optimization을 하기 위해서는 이전에 배웠던 Gradient Descent를 이용하는 것으로 가정해보겠습니다. Gradient는 미분한 방향이 가장 가파른 방향으로 해당 반대 방향으로 Weight들을 업데이트해주면 됩니다. 그럼 이 모둔 Weight들에대해 미분을 수행해야할 것입니다. 이를 간단하게 하기 위해서 **Chain Rule**을 적용하여 편미분을 수행해보겠습니다.   

## 오차 역전파란?
<img src="../../../assets/images/DeepLearning/2024-05-05-BackPropagation/DNN 2.jpg" alt="DNN 2" style="zoom:80%;" />    
상기의 매우 작으며 간단한 DNN 구조를 가지고 직접 풀어보면서 오차 역전파가 어떤 것인지, Gradient를 구하기 위해 미분은 어떻게 해야하는지(Weight Update)에 대해 알아보겠습니다.   

각각 왼쪽의 첫번째 Node Layer부터 0번째 Layer, 1번째 Layer, 2번째 Layer, 3번째 Layer라고 부르겠습니다. 그리고 Node들을 위에서부터 0,1,2로 번호를 매겨놓겠습니다. 그럼 0번째 Layer에서 0번째 Node에서 1번째 Layer의 1번째 Node로 가는 Weight는 $w_{01}^{(0)}$으로, 1번째 Layer 2번째 Node로 가는 Weight는 $w_{02}^{(0)}$으로 표기할 수 있습니다. 그리고 각 Node에서는 Weighted Sum으로 받은 Output을 $S$로 표기하는데, 0번째 Layer에 존재하는 Node들과 Weighted sum으로 구해져 1번째 Layer의 1번째 Node로 들어가는 것을 $S_1^{(0)}$으로 설정하여 표기하겠습니다. 또한, 각 Node에는 activation function을 거치고 다시 다음 Node로의 Input으로 나오게 되는데, 이 때 activation function을 $g$로 표기하겠습니다. 그리고 activation function을 거치고 나와 다음 Node로 들어가는 Output은 $x$로 표기하겠습니다. 만약, $x_1^{(2)}$라면 2번째 Layer의 1번째 Node에서 나오는 출력이며 이 출력이 바로 Weight Sum이 이루어져 다음 Node로 들어가게됩니다.       

그럼 상기의 이미지에서의 최종 출력인 $x_1^{(3)}, x_2^{(3)}$은 어떤 값을 가지는지 표기해보겠습니다.   
\begin{aligned}    
x_1^{(3)} =& g(S_1^{(2)}) \newline   
S_1^{(2)} =& W_{01}^{(2)} + W_{11}^{(2)}x_{1}^{(2)} + W_{21}^{(2)}x_{2}^{(2)} \newline   
x_2^{(3)} =& g(S_2^{(2)}) \newline   
S_2^{(2)} =& W_{02}^{(2)} + W_{12}^{(2)}x_{1}^{(2)} + W_{22}^{(2)}x_{2}^{(2)} \newline   
\end{aligned}   

상기의 Neural Network가 간단하게 Linear Regression이라고 한다면, activation function은 $y=g(x)=x$가 될 것이며, Loss인 Error 함수는 MSE가 될 것입니다. $err=f=(y_1 - x_1^{(3)})^2 +(y_2 - x_2^{(3)})^2$인 이 <span style='color:blue'>**함수 $f$를 최소화 하는 것이 바로 Learning의 목표**</span>가 될 것 입니다. 이 $f$를 최소화할 수 있도로고 $w$의 값들을 잘 조정해줘야 합니다.    
그럼 이 Loss함수를 최소화하기 위해 Gradient 방향으로 가야하니 미분을 수행하겠습니다.   
<br>
\begin{aligned}    
f =& (y_1 - x_1^{(3)})^2 +(y_2 - x_2^{(3)})^2 \newline   
\frac{\partial f}{\partial w_{21}^{(2)}} =& \frac{\partial f}{\partial x_1^{(3)}} \frac{\partial x_1^{(3)}}{\partial S_1^{(2)}}  \frac{\partial S_1^{(2)}}{\partial w_{21}^{(2)}} \newline   
\frac{\partial S_1^{(2)}}{\partial w_{21}^{(2)}} =& x_2^{(2)} \newline   
\frac{\partial f}{\partial w_{22}^{(2)}} =& \frac{\partial f}{\partial x_2^{(3)}} \frac{\partial x_2^{(3)}}{\partial S_2^{(2)}}  \frac{\partial S_2^{(2)}}{\partial w_{22}^{(2)}} \newline   
\frac{\partial S_2^{(2)}}{\partial w_{22}^{(2)}} =& x_2^{(2)}
\end{aligned}   

우선 $w_{21}^{(2)}$에 대해 미분을 할 때, $f$에서 $w_{21}^{(2)}$의 변화에 따라 영향을 받는 것은 $x_1^{(3)}$밖에 없습니다. 따라서 Chain Rule을 적용하였습니다.    
<br>
그럼 이번에는 2번째 Layer가 아닌, 1번째 Layer의 weight에 대해 미분을 해보겠습니다. 상기의 이미지에서 보면 $w_{12}^{(1)}$가 벼한다면 2번째 Layer의 2번째 Node에 영향을 주게될 것이고, 2번째 Layer의 2번째 Node는 3번째 Layer에 있는 모든 Node들에게 영향을 주게됩니다. 따라서, Loss함수에서 $x_1^{(3)}, x_2^{(3)}$ 모두한테 영향을 주게됩니다.   
\begin{aligned}    
f =& (y_1 - x_1^{(3)})^2 +(y_2 - x_2^{(3)})^2 \newline   
\frac{\partial f}{\partial w_{12}^{(1)}} =& \frac{\partial f}{\partial x_1^{(3)}} \frac{\partial x_1^{(3)}}{\partial S_1^{(2)}}  \frac{\partial S_1^{(2)}}{\partial x_2^{(2)}} \frac{\partial x_2^{(2)}}{\partial S_2^{(1)}} \frac{\partial S_2^{(1)}}{\partial w_{12}^{(1)}} +  \frac{\partial f}{\partial x_2^{(3)}} \frac{\partial x_2^{(3)}}{\partial S_2^{(2)}}  \frac{\partial S_2^{(2)}}{\partial x_2^{(2)}} \frac{\partial x_2^{(2)}}{\partial S_2^{(1)}} \frac{\partial S_2^{(1)}}{\partial w_{12}^{(1)}} \newline   
\frac{\partial S_2^{(1)}}{\partial w_{12}^{(1)}} =& x_1^{(1)} \newline   
\frac{\partial S_2^{(1)}}{\partial w_{12}^{(1)}} =& x_1^{(1)}   
\end{aligned}   

상기의 같이 쭉 진행될 것 입니다. 근데 여기서 보면 Chain Rule에도 계속 똑같은 것이 반복되는 것을 확인할 수 있으실겁니다. $\frac{\partial f}{\partial x_1^{(3)}} \frac{\partial x_1^{(3)}}{\partial S_1^{(2)}}$부분의 모양이 계속 똑같습니다. 그럼 이를 한번 치환해보겠습니다.   
\begin{aligned}    
\delta_1^{(2)} \triangleq& \frac{\partial f}{\partial x_1^{(3)}} \frac{\partial x_1^{(3)}}{\partial S_1^{(2)}} \newline   
\frac{\partial f}{\partial w_{21}^{(2)}} =& \delta_1^{(2)} x_2^{(2)} \newline   
\delta_2^{(2)} \triangleq& \frac{\partial f}{\partial x_1^{(3)}} \frac{\partial x_1^{(3)}}{\partial S_2^{(2)}} \newline   
\frac{\partial f}{\partial w_{22}^{(2)}} =& \delta_2^{(2)} x_2^{(2)} \newline   
\frac{\partial f}{\partial w_{12}^{(1)}} =& \delta_1^{(2)} \frac{\partial S_1^{(2)}}{\partial x_2^{(2)}} \frac{\partial x_2^{(2)}}{\partial S_2^{(1)}} \frac{\partial S_2^{(1)}}{\partial w_{12}^{(1)}} + \delta_2^{(2)} \frac{\partial S_2^{(2)}}{\partial x_2^{(2)}} \frac{\partial x_2^{(2)}}{\partial S_2^{(1)}} \frac{\partial S_2^{(1)}}{\partial w_{12}^{(1)}} \newline   
=& \delta_1^{(2)} w_{21}^{(2)} \frac{\partial x_2^{(2)}}{\partial S_2^{(1)}} x_1^{(1)} + \delta_2^{(2)} w_{22}^{(2)} \frac{\partial x_2^{(2)}}{\partial S_2^{(1)}} x_1^{(1)} \newline   
=& \begin{bmatrix} \delta_1^{(2)} & \delta_2^{(2)} \end{bmatrix} \begin{bmatrix} w_{21}^{(2)} \newline w_{22}^{(2)} \end{bmatrix} \frac{\partial x_2^{(2)}}{\partial S_2^{(1)}} x_1^{(1)} \newline   
=& \underline{\delta}^{(2)T} \underline{w_2}^{(2)} \frac{\partial x_2^{(2)}}{\partial S_2^{(1)}} x_1^{(1)} \newline   
=& \frac{\partial f}{\partial S_2^{(1)}} x_1^{(1)} = \delta_2^{(1)} x_1^{(1)}    
\end{aligned}   

상기의 수식을 보면 미분에서 패턴이 존재하는 것을 알 수 있습니다. 즉, $w_{21}^{(2)}$을 보면 $w$하단의 의미가 2번째 Node에서 다음 Layer읠 1번째 Node로라는 의미가 되니, 이는 첫번째 Node에 대한 미분인 $\delta_1^{(2)}$가 들어갑니다. 그럼 반대로 $w_{22}^{(2)}$은 $\delta_2^{(2)}$가 들어갑니다. 또한, 뒤에 $x$도 패턴을 따라가는데, 모두 2번째 Node에서 출발하니 $x_2^{(2)}$가 들어가는 것을 확인할 수 있습니다. 그럼 똑같이, 1번째 Layer에 대한 미분인 $\frac{\partial f}{\partial w_{12}^{(1)}}$에서는 $\delta_2^{(1)}$가 들어가며 $x_1^{(1)}$도 들어가는 패턴을 확인할 수 있습니다. 그럼 이 패턴을 일반화해보겠습니다.     
\begin{aligned}    
\frac{\partial f}{\partial w_{ij}^{(l)}} =& \delta_{j}^{(l)} x_{i}^{(l)}
\end{aligned}   

즉, l번째 Layer이 i번째 Node에서 l+1번째 Layer의 j번째 Node로 가는 곳에 대한 미분은 $\delta_{j}^{(l)}$와 $x_{i}^{(l)}$값이 필요하다는 의미가 됩니다. 그럼 $\delta_{j}^{(l)}$은 어떻게 구해지는지 좀 더 구체적으로 알아보겠습니다.   
\begin{aligned}    
\delta_{j}^{(l)} =& (\underline{\delta}^{(l+1)})^T \underline{w_j^{(l+1)}} \frac{\partial x_j^{(l+1)}}{\partial S_j^{(l)}}
\end{aligned}   

<br>   

그럼 이제 backpropagation이 무엇인지 알알보겠습니다. 최종적으로 weight를 업데이트하기 위해서는 $\frac{\partial f}{\partial w_{ij}^{(l)}}$것이 필요합니다. 근데 $\frac{\partial f}{\partial w_{ij}^{(l)}}$을 얻기 위해서는 $\delta_{j}^{(l)}$을 알아야하는데, $\delta_{j}^{(l)}$을 알아야합니다. 우선 l번째 Layer의 weight를 업데이트 한다고 해보겠습니다. 그럼 $\delta_{j}^{(l)}$이 필요하는데 이를 얻기위해서는 또한 l+1번째의 Layer의 정보도 필요합니다. 또한, l+1번째 Layer의 정보를 얻기위해서는 l+2번째 Layer의 정보도 필요합니다. 즉, 재귀적으로 필요하게되니, 만약 0번째 layer의 weight를 업데이트하기위해서는 결국 마지막 layer의 정보가 필요하다는 의미가됩니다. 마지막 layer를 n번째 layer라고 하겠습니다. 그럼 n-1번째 layer의 weight를 업데이트하기 위해서는 n번째 layer의 정보가 필요하게되고, n-2번째 layer의 weight를 업데이트하기 위해서는 n-1번째, n번째 layer의 정보가 필요해집니다. 즉, 거꾸로 n번째의 정보를 이용하여 n-1번째 업데이트, n-1번째 정보를 이용해 n-2번째 업데이트를 수행하게됩니다. 따라서 거꾸로 간다고 하여 backpropagation이라고 부릅니다.    
근데 여기서 backpropgation을 수행하기전에 우선적으로 정방향인 0번째에서 n번째 방향으로의 값을 먼저얻어야하는데, 이를 forward propgation이라고 부릅니다.   
<br>   

그럼 Neural Network의 동작과정을 간단하게 말해보겠습니다.   
① 우선 weight들의 초기화를 먼저 설정해줍니다.   
② 그 후, 입력데이터 x와 weight들간의 weight sum을 통해 0번째에서 n번째까지를 거쳐 최종 output값을 얻습니다.(forward propagation)   
③ 그 후 error를 구한 후, Gradient방향으로 backward propagation을 수행해줍니다.(n-1번째 layer 업데이트 후, n-2번째 layer 업데이트 $\rightarrow$ 반대 방향으로) : $w_{ij}^{(l)} \leftarrow w_{ij}^{(l)} - \alpha \frac{\partial f}{\partial w_{ij}^{(l)}}$    
④ backpropgagtion으로 업데이트된 weight들을 기반으로 다시 ②과 ③을 수행해 error를 가장 최소화해주는 weight들을 찾아줍니다.    
