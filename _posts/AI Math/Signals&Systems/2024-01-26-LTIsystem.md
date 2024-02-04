---
title:  "LTI(Linear Time-invariant)"
categories: SystemsAndSignals
tag: [theory, Systems, Signals]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: LTI system
comments: true
date: 2024-1-26
toc_sticky: true
---

## LTI System
시스템은 입력을 받아서 출력을 내보내기 위해 형태를 바꿔주는 곳을 말합니다. 즉, 하기의 이미지에서 처럼 input signal이 들어오면 input signal에 어떤 형태를 취해주어 우리가 원하는 모양의 output signal을 얻게 해주는 기능을 하는 것이 시스템입니다.   
<img src="../../../assets/images/Signals&Systems/2024-01-26-LTIsystem/system Archtitecture.jpg" alt="system Archtitecture" style="zoom:80%;" />    
쉽게 이해하기 위해 수학으로 생각을 하시면, 함수와 같으며 함수도 시스템입니다. 이와 같이 시스템은 입력을 출력으로 만들어주는 모든 것들을 말하며, 특정 시스템의 성질에 따라 특정 시스템을 분류할 수 있습니다. 이번 포스팅에서 배울 LTI(Linear Time Invariant) 시스템은 **Linearity(선형성)**와 **Time-invariant(시간 불변성)**을 갖는 시스템을 말합니다. 이 두 가지 특성은 **시스템을 예측 가능(Predictable)**하게 해준다는 장점이 있습니다.   

### Linear
LTI 시스템은 <span style='color:blue'>**Linearity + Time-invariant**</span>라고 말씀드렸습니다. 그럼 우선 Linearity가 무엇인지에 대해 알아보겠습니다.   
Linearity은 Scaling 과 Additivity을 모두 만족해야합니다. 이 두 가지 특성인 Scaling과 Additivity를 합쳐 SuperPosition이라고 합니다.   
#### Scaling
Scaling이 어떤 것인지 예시를 들어 설명해보겠습니다. 만약 어떤 자판기라는 시스템이 있다고 가정해보겠습니다. 이 자판기에 500원 동전 한 개를 넣으면 콜라 1개가 나오게됩니다. 그럼 500원 동전 10개를 넣으면 어떻게 될까요? 콜라 10개가 나올 것 입니다. 이렇게 입력의 크기가 늘어난만큼 출력의 크기도 똑같은 배수로 늘어나는 것이 Scaling입니다.   
즉 연속시간 신호에 임의의 상수 a를 곱한 입력을 시스템에 입력하여 얻어진 출력 신호는 임의의 상수를 곱하기 전 연속시간 신호를 입력하고 출력 신호에 상수 a를 곱한 것과 같아집니다.   

이제 Scaling이 어떤 것인지 대충 감을 잡으셨을 것이라고 생각합니다. 그럼 수식으로 알아보겠습니다.   
\begin{aligned}    
x_1(t) \rightarrow& System \rightarrow y_1(t) \newline   
x_2(t) = 6x_1(t) \rightarrow& System \rightarrow 6y_1(t) = y_2(t)
\end{aligned}  

#### Additivity
Additivity도 똑같이 예시를 들어 우선 설명하겠습니다. 자판기라는 시스템이 똑같이 있는데, 이 자판기는 500원을 넣으면 콜라, 1000원 넣으면 과자가 나오는 자판기라고 가정을 해보겠습니다. 그럼 500원을 넣을시 콜라 1개, 1000원을 넣을시 과자 1개를 얻을 수 있습니다. 그럼 1500원을 넣게 되면, 콜라 1개와 과자 1개를 얻을 수 있습니다. 이렇게 서로 다른 입력 2개를 더할 경우, 서로 다른 입력에 대한 출력들도 더한 결과인 새로운 출력으로 나온다는 뜻입니다.    
두 연속시간 신호가 더해져서 시스템에 입력되고 이를 통해 얻어진 출력 신호가 두 연속시간 신호를 분리해서 각각 시스템에 입력하고 얻어진 출력 신호를 합친 것과 동일합니다. 즉, 더해서 넣어서 출력한 거랑, 각각을 출력한 것를 더한 것이랑 서로 같다는 의미입니다.   

이제 Additivity도 어떤 것인지 대충 감을 잡으셨을 것이라고 생각합니다. 그럼 수식으로 알아보겠습니다.   
\begin{aligned}    
x_1(t) \rightarrow& System \rightarrow y_1(t) \newline   
x_2(t) \rightarrow& System \rightarrow y_2(t) \newline   
x_3(t) = x_1(t) + x_2(t) \rightarrow& System \rightarrow y_1(t) + y_2(t) = y_3(t)
\end{aligned}   

#### SuperPosition
SuperPosition은 방금 설명한 Scaling과 Additivity의 특성을 모두 만족, 즉 합친 것이라고 말씀드렸습니다. 그럼 시스템으로 알아보겠습니다.   
\begin{aligned}    
\alpha x_1(t) + \beta x_2(t) \rightarrow& System \rightarrow \alpha y_1(t) + \beta y_2(t)   
\end{aligned}   

다음과 같이 Scaling인 $\alpha, \beta$만큼의 배수가 늘어난 출력과 Additivity인 두 출력의 더하기로 나타납니다.   

### Time-invariant
위에서 Linearity에 대해 알아보았으니 이번에는 **Time-invariant**에 대해 알아보겠습니다.   
Time-invariant는 입력이 time shift가 되었을 때, 출력 또한 동일한 크기만큼 time-shift가 일어나는 것을 의미합니다.    
그럼 간단한 예시를 통해 설명해보겠습니다. $x_1(t) \rightarrow System \rightarrow y_1(t)$인 시스템이 있다고 가정해보겠습니다. 그럼 이 시스템에 $x_1(t - t_0)$의 입력을 넣어주면 출력은 어떻게 될까요? 출력은 $y_1(t - t_0)$와 같은 출력이 나타납니다. 이런 시스템은 TI(Time-invariant) 시스템이 됩니다. $t_0$만큼 지연된 입력이 들어오면, 출력 또한 $t_0$만큼 지연된 출력이 나타납니다.    

그럼 이번엔 $y(t) = x(-2t + 2)$인 시스템이 있을 때, 이 시스템이 TI(Time-invariant)인지 한번 확인해보겠습니다. 입력 $x(-2t + 2)$는 하기와 같은 시스템의 과정을 거쳐 변경되었습니다.   
① y축 대칭 : $x(t) \rightarrow x(-t)$   
② 2배 감소 : $x(-t) \rightarrow x(-2t)$   
③ 1 shift : $x(-2t) \rightarrow x(-2(t - 1)) = x(-2t + 2)$   
즉, 이 시스템은 입력으로 $x(t)$가 들어오면, 출력으로 $x(-2t + 2)=y(t)$가 나오는 시스템입니다.   

그럼 이번에 Time Delay를 시킨 입력을 시스템에 넣어보겠습니다. $t_0$만큼 지연된 시간인 $x(t - t_0)$의 입력을 해당 시스템에 넣어보겠습니다. 만약 **시스템이 TI(Time-invariant)라면 출력도 $t_0$만큼 지연된 $y(t - t_0) = x(-2(t - t_0) + 2) = x(-2t + 2t_0 + 2)$가 나올 것 입니다.** 시스템 과정을 상기와 같이 거쳐보겠습니다.   
① y축 대칭 : $x(t- t_0) \rightarrow x(-t - t_0)$   
② 2배 감소 : $x(-t - t_0) \rightarrow x(-2t - t_0)$   
③ 1 shift : $x(-2t - t_0) \rightarrow x(-2(t - 1) - t_0) = x(-2t + 2 - t_0)$   
상기와 같이 출력으로 $x(-2t + 2 - t_0)$이 나오게됩니다. TI일 경우에 출력에 shift시킨 결과인 $x(-2t + 2t_0 + 2)$와는 다른 결과가 나오니 해당 시스템은 TI가 아니게됩니다.    

