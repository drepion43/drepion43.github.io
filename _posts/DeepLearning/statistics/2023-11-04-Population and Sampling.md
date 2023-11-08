---
title:  "표본 집단"
categories: statistics
tag: [DeepLearning,theory, python,statistics,math]
tags: [Jekyll, MathJax]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
---

## 자유도와 비편향추정량
자유도는 독립 변수의 개수를 의미합니다. 예를 들어, $x + y + z= 8$의 독립 변수의 개수를 구해보겠습니다. 만약, $x=3, y=2$로 변수 값을 지정했다고 하면 자연스럽게 $z=3$으로 결정이납니다. 즉, 설정한 변수값은 2개가 됩니다. **총 n개의 변수가 있을 때, 자유도는 n-1**이 됩니다.   
이번에는 비편향추정량에 대해 설명하겠습니다. 참고로 이전에는 비편향추정량과 불편추정량 이렇게 두개다 사용하긴 했지만, 최근에는 불편추정량이라는 말을 쓰지않는다고 합니다.   
<img src="../../../assets/images/statistics/2023-11-04-Population and Sampling/sample1-16994466706252.jpg" alt="sample1" style="zoom:80%;" />  
상기의 이미지에서도 볼 수 있듯이, 모집단의 통계량은 모수(parameter)라고 부르며, 표본 집단의 통계량은 추정량이라고 부릅니다. 쉽게 말하면 모집단에서 추출한 샘플을 가지고 모집단의 분포(모수)를 추정을 한다고 말할 수 있습니다. 표본 통계량 $(\bar{X}, S^2)$은 각각 표본 평균과 표본 분산입니다. 표본 평균의 경우 $\bar{X} = \sum_{i=1}^{n} \frac{x_i}{n}$이 됩니다.   
추정량과 모수간의 차이를 Bias(편향)라고 부릅니다. Bias의 정의는 아래와 같습니다.   
$Bias(\hat{\theta}) = E(\hat{\theta}) - \theta$   
만약, Bias값이 0일경우인, $E(\hat{\theta}) = \theta$에 $\hat{\theta}$은 비편향추정량이 됩니다.    
모수의 추정 방법에는 적률추정법, 최대가능도추정법, 최소분산비편향추정법 크게 3가지가 있습니다. 이 내용은 다음에 다시 자세하게 다뤄보도록 하겠습니다.

상기의 이미지에서 추출한 표본 평균의 기댓값은 $E(\bar{X})= \mu$가 됩니다. 즉, 추정량의 기댓값은 모수와 같아지니 표본 평균은 비편향추정량이 됩니다.   
표본 분산이 $S^2= \frac{\sum_{i=1}^{n}(x_i - \bar{x})^2}{n}$이라고 했을 때, 표본 분산의 기댓값은 $E(S^2) \ne \sigma^2$가 됩니다.. 따라서 비편향추정량이 되질 못합니다. 하지만, $S^2= \frac{\sum_{i=1}^{n}(x_i - \bar{x})^2}{n-1}$로 표본 분산을 변경했을 때, 표본 분산의 기댓값은 $E(S^2) \= \sigma^2$가 됩니다. 따라서 비편향추정량이 됩니다. 여기서 표본 분산의 분모는 n-1이 되는데 이것은 아마도 표본 분산을 비편향추정량으로 만들어주기 위해, n-1을 사용했으며, 아마도 자유도를 뜻하는게 아닐까라고 생각합니다.   
따라서, 표본 평균과 표본 분산을 정리하겠습니다.   
$\bar{X} = \sum_{i=1}^{n} \frac{x_i}{n}$   
$S^2= \frac{\sum_{i=1}^{n}(x_i - \bar{x})^2}{n-1}$   

## 표본 평균의 기댓값

<img src="../../../assets/images/statistics/2023-11-04-Population and Sampling/sample2.jpg" alt="sample2" style="zoom:80%;" />   
모수(parameter)가 ($\mu, \sigma^2$)인 모집단에서 표본을 **비복원** 추출했습니다. 이 때, 표본들의 평균은 $\bar{X}= \frac{\sum_{i=1}^{n} x_i}{n}$이 됩니다.   
이 표본들의 평균은 $\bar{X_1}, \bar{X_2}, ...$들의 평균입니다. 이제 각 표본1, 표본2들에 대해 살펴보겠습니다.   
<img src="../../../assets/images/statistics/2023-11-04-Population and Sampling/sample3.jpg" alt="sample3" style="zoom:80%;" />   
표본1의 표본들은 $x_1^1, x_2^1, x_3^1, ... , x_n^1$로 표현했습니다. 즉, 표본1은 $x^1_n$의 집합들입니다. 표본1의 평균을 구해보겠습니다. 표본1의 평균은 $\bar{X_1} = \frac{\sum_{i=1}^{n} x_i^1}{n}$이 됩니다. 이와 똑같이 표본k 등 무한정의 표본들이 존재한다고 해보겠습니다.   
그럼 모집단에 추출한 표본들 중에 $x_1^k$인 표본들의 대표를 $x_1$라고 해보겠습니다. 그러면 자연스럽게 $x_n$인 대표까지 생성되며, 표본 평균의 대표인 $\bar{X}$로 설정해보겠습니다. 그러면 $\bar{X}=\frac{\sum_{i=1}^{n} x_i}{n}$이 됩니다. 하기와 같이 설정하겠습니다.    
<img src="../../../assets/images/statistics/2023-11-04-Population and Sampling/sample4.jpg" alt="sample4" style="zoom:80%;" />

그럼 이제 표본 평균의 기댓값을 구해보겠습니다.   
<img src="../../../assets/images/statistics/2023-11-04-Population and Sampling/sample5.jpg" alt="sample5" style="zoom:80%;" />   
표본 평균의 기댓값은 말 그대로 표본 평균들의 기댓값을 뜻합니다. 이 표본 평균들은 표본k개 이후에 무한정으로 추출하게되니, 각 표본 평균을 다 더해준 후 k가 무한으로 갈때로 나눠준 평균을 통해구해줄 수 있습니다. 이 뜻은 $E(\sum_{i=1}^{n} \frac{x_i}{n})$이 됩니다.