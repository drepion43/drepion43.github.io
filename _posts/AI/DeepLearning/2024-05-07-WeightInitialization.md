---
title:  "가중치 초기화(Weight Initialization)"
categories: DeepLearning
tag: [theory, AI, DeepLearning]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: 가중치 초기화
comments: true
date: 2024-05-07
toc_sticky: true
---

## Weight Initialization
Neural Network의 목적은 **Error 함수(Loss 함수)를 최소화하는 것**입니다. 보통, 이 Loss 함수를 최소화하기 위해 Gradient Descent방법을 적용하여, 현재의 Parameter들이 어떤 방향으로 바뀌어야 이전 Loss함수보다 최소화가 될 수 있는지를 구하여 그 방향으로 Weight를 업데이트합니다. 그럼 여기서 이 Weight의 초기값은 어떻게 설정하는게 좋은지와 Weight 초기값이 어떤 의미를 가지는지에 대해 알아보겠습니다.    
<img src="../../../assets/images/DeepLearning/2024-05-07-WeighIInitialization/Loss Gradient.png" alt="이미지 출처 : medium.com/coinmonks" style="zoom:80%;" />    
예를 들어, 어떤 Neural Network의 Loss함수가 상기의 이미지의 그래프와 같이 생겼다고 가정해보겠습니다. 그럼, Neural Network의 목적에 따라 이 그래프의 최저점을 향해 가는 것이 목적이 될 것 입니다. 상기의 이미지는 빨간색 봉우리에서 시작해서 Gradient 방향으로 내려가며 Loss함수를 최소화시키고 있습니다. 근데 여기서 만약 주황색 봉우리에서 시작할 경우, 빨간색 봉우리에서 출발하여 도착하는 최저점과 다른 곳에 안착하게됩니다. 즉, Local Minimum에 빠져서 나오지 못하는 문제가 발생할 수 있습니다. 시작점에 따라 Local Saddle Point에 도달할 수 있고, Global Saddle Point에 도달할 수 있다는 의미가됩니다. 따라서 어떤 가중치 초기화를 쓰는지에 따라 달라지기 때문에, 적절한 가중치 초기화법이 필요하게됩니다.   

### Zero Initialization
가장 먼저 생각해볼 수 있는 가중치 초기화는 모두 0으로 초기화하는 방법일 것 입니다. 이전에 배웠던 weight sum을 생각해보겠습니다. 이 경우 가중치를 모두 0으로 초기화를 시켜놓는다면 어떤 Input이 들어오던 Output은 항상 0으로 나올 것 입니다. 즉, 0으로 초기화를 하면 안된다는 의미입니다.    
그럼 Zero Initialization을 조금 다른 의미로 해석해보겠습니다. 모든 가중치의 값이 동일한 값이라고 생각해보겠습니다. 그럼 backpropagation을 통해 weight를 업데이트할 때, 모두 똑같은 값으로 변하게될 것 입니다. 즉, training의 의미가 없어지며, Neural Network의 구조자체가 의미가 없어질 것 입니다. 따라서, 저는 Zero Initialization의 초기화 방법은 배제하는 것이 좋다고 생각합니다.   

### Random Initialization

### Xavier Initialization

### He Initialization
