---
title:  "Causal & Non-Causal"
categories: SystemsAndSignals
tag: [theory, Systems, Signals]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: Causal & Non Causal
comments: true
date: 2024-1-20
toc_sticky: true
---

## Causal & Non-Causal
2017년도에 Attention is all you need의 논문이 게재되면서 Transformer가 엄청난 각광을 받았습니다. 그 이후 Transformer의 attention에 다양한 방법들을 취하는 추가적인 연구들이 진행되어 논문도 많이 게재되었습니다. 많은 attention기법들을 본 사람들 중에 신호 및 시스템 공부를 하지 않은 사람들도 있었을 텐데, 이 때 Causal Attention Mask 등에 대해 처음 접해서 따로 공부해보신 분들도 계실거라고 생각이 듭니다. 이번 포스팅에서는 Causal이 무엇인지, 그 반대인 Non-Causal이 무엇인지에 대해 간략하게 알아보겠습니다.    

**Causal**이란?   
<span style='color:red'>**인과관계의**</span>라는 매우 간단한 정의입니다.   
그럼 Causal System이란? 즉, **인과관계가 있는 System**이라느 의미가 됩니다. 그럼 반대로, Non-Causal System이란? **인과관계가 없는 System**이라는 의미입니다.   
아직 감이 안잡히신 분들도 계실겁니다. 인과관계가 있는 System? 이렇게 의문을 가지실 수도 있으니, 조금 더 쉽게 설명을 해보겠습니다.   
<span style='color:red'>**Causal System은 과거와 현재의 인과관계에 의해 현재가 결정되는 것, 즉, 현재와 과거의 입력에 의해 출력이 결정나는 시스템**</span>입니다.   
<span style='color:red'>**Non-Causal System은 인과관계 없이 미래에 의해 현재가 결정되는 것, 즉, 미래의 입력에 의해 출력이 결정나는 시스템**</span>입니다.   

Causal System의 수식은 하기와 같습니다.   
\begin{aligned}    
y(t) = \frac{1}{C} \int_{- \infty}^{t} x(\tau) d \tau
\end{aligned}  

Causal System은 수식에서 볼 수 있듯이, 아주 먼 과거에서 부터 현재(t)까지 출력(output)에 영향을 줍니다. 그럼 Non-Causal System의 수식도 알아보겠습니다.   
\begin{aligned}    
y(t) = \int_{t}^{\infty} x(\tau) d \tau
\end{aligned}   

Non-Causal System도 수식에서 볼 수 있듯이, 현재(t)부터 아주 먼 미래까지가 출력(output)에 영향을 줍니다. 
### 예제
예제를 통해 좀 더 쉽게 이해해보도록 하겠습니다.   
\begin{aligned}    
① y(t) = x(t) \newline   
② y(t) = x(t)cos(3t) \newline   
③ y(t) = x(t)cos(t + 1) \newline    
④ y(t) = x(-t) 
\end{aligned}    

①부터 확인해보도록 하겠습니다. 가장 쉽게 판단할 수 있는 방법이 $t$값을 대입하는 방법입니다. 한번 $t=1$을 대입해보겠습니다. 그럼 $y(1)=x(3)$이 됩니다. 해당 식의 의미는 $x$는 입력이고 $y$는 출력이 되는데, 3초에서의 입력이 들어와 1초에서의 출력이 나왔다는 의미가 됩니다. 즉, 출력 입장에서는 현재 1초에서 미래인 3초의 입력에 의해 현재 1초의 출력이 결정이 난 것이기 때문에, 해당 시스템은 **Non-Causaul**이 됩니다.   

③도 확인해보겠습니다. 이번도 똑같이 $t=1$을 대입해보겠습니다. 그럼 $y(1)=x(1)cos(3)$가 됩니다. 여기서 $x$는 입력이지만, $cos$은 그냥 값인 상수가 됩니다. 따라서 현재 1초에서의 출력인 $y$는 현재 1초에서의 입력인 $x$에 의해 결정이 나게된다는 뜻이됩니다. 따라서 해당 시스템은 **Causal**이 됩니다.    

③도 확인해보겠습니다. 이번도 똑같이 $t=1$을 대입해보겠습니다. 그럼 $y(1)=x(1)cos(2)$가 됩니다. 여기서 $x$는 입력이지만, $cos$은 그냥 값인 상수가 됩니다. 따라서 현재 1초에서의 출력인 $y$는 현재 1초에서의 입력인 $x$에 의해 결정이 나게된다는 뜻이됩니다. 따라서 해당 시스템은 **Causal**이 됩니다.   

마지막으로 ④을 확인해보겠습니다. 이번에도 똑같이 $t=1$을 대입해보겠습니다. 그럼 $y(1)=x(-1)$가 됩니다. 즉, 현재 1초에서의 출력은 과거인 -1초에서의 입력에 의해 결정이 납니다. 그럼 이번에는 $t=-1$을 대입해보겠습니다. 그럼 $y(-1)=x(1)$가 됩니다. 여기서는 -1초에서의 출력이 미래인 1초에서의 입력에 의해 나타납니다. 즉, 해당 시스템은 **Non-Causal**이라는 뜻이 됩니다.