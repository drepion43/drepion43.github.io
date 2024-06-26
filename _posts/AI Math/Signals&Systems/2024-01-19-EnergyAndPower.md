---
title:  "Energy&Power"
categories: SystemsAndSignals
tag: [theory, Systems, Signals]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: 에너지와 파워
comments: true
date: 2024-1-19
toc_sticky: true
---

## 힘
힘은 어떻게 정의되는지 알아보겠습니다. 힘은 $F=ma$로 다들 알고 있지만, 실상 이 힘은 뉴턴이 정의한 힘이 됩니다. 힘은 이거다라고 약속을 한 것입니다. 그럼  즉, 힘이 정의되어, 일을 정의할 수 있고, 에너지 또한 정의할 수 있게 되었습니다. 힘의 단위는 $[N]$이 됩니다.   
\begin{aligned}    
F=ma   
\end{aligned}   

## 일
일은 **물체가 이동하는 방향으로의 힘에 물체가 이동한 거리를 곱한 값**이 됩니다. 일의 단위는 $[J] = [N \cdot m]$가 됩니다. 

## 에너지(Energy)
그 당시 배웠던 에너지에 대해 되짚어보면 운동에너지, 위치에너지 등에 대한 얘기가 익숙하실 것 입니다. 그럼 에너지란 무엇인가라고 묻는다면, **물체에 일을 해줌으로써 발생하는 물리량**입니다.  그럼 이제 에너지에 대해 수식으로 정의해보겠습니다.   
\begin{aligned}    
Energy = \int_{t_1}^{t_2} | x(t) |^2 dt
\end{aligned}   

상기의 수식이 $t_1 \le t \le t_2$에서의 Total Energy를 의미합니다. 여기서 에너지는 **일**에 의해 영향을 받기 때문에, 일을 한만큼에 대해 에너지(일 운동 에너지) 또한 변하게 됩니다. 즉, 운동 에너지가 위치 에너지로 변화하지만, 전체 에너지의 크기는 변하지 않습니다. 왜냐하면 에너지는 에너지 보존 법칙을 따르기 때문입니다. 에너지의 단위는 $[J] = [N \cdot m]$이 됩니다. 즉, 힘과 거리의 곱이 일이며 에너지가 됩니다.   

## 일률(Power)
일률은 일의 미분이 됩니다. 일률의 단위는 일의 미분이 되니 $[J / s]=[W]$가 됩니다.   
전력에서 시간에 따른 변화하는 값은 전압이나 전류가 될 수 있습니다. 이를 통해 전력을 계산하면 아래와 같이 나타낼 수 있습니다.   
\begin{aligned}    
P = V I = I^2 R = \frac{V^2}{R}
\end{aligned}   

여기서 에너지는 일에 의해 나타나며, 일률은 일의 미분값이 됩니다. 그럼 반대로 일률을 적분해주면  일이 되며, 에너지가 됩니다. 상기의 수식에서 제곱 형태로 표현이 됩니다. 즉, 제곱을 시간에 따라 적분을 해주면 에너지를 구할 수가 있습니다.   
따라서 에너지는 **일률은 제곱 형태를 띄고 있기 때문에, 이 제곱 형태를 시간에 따라 적분을 해준 꼴**로 나타나게 됩니다.    

## 순간 Power
여기서 에너지는 일률(Power)의 시간에 따른 적분이라고 했습니다. 그럼 $|x(t)|^2$는 현시점에 대한 일률을 의미하며, 순간 파워를 의미하게됩니다. 절댓값은 복수소 영역에서도 처리하기 위해 취해준 것 입니다. 

## 평균 Power
여기서 미분에 대한 개념을 적용해주면, 적분에 대해 미분을 취해주면 적분하기 전의 값이 나옵니다. 즉, 에너지를 시간에 대하 나눠주면 평균 Power가 나온다는 의미가 답니다.   
\begin{aligned}    
Average \; Power = \frac{1}{t_2 - t_1}\int_{t_1}^{t_2} | x(t) |^2 dt
\end{aligned}   

그럼 여기서 보통 limit를 취해주는데, 시간에 대해 limit을 취해주면 하기와 같이 무한대에 대한 전체 에너지, 무한대에 대한 평균 power가 됩니다.   
\begin{aligned}    
lim_{T \rightarrow \infty} \int_{-T}^{T} | x(t) |^2 dt \newline   
lim_{T \rightarrow \infty} \frac{1}{2T}\int_{-T}^{T} | x(t) |^2 dt \newline   
\end{aligned}   