---
title:  "모수 추정3"
categories: statistics
tag: [DeepLearning,theory, python,statistics,math]
tags: [Jekyll, MathJax]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: 최소 분산 비편향 추정량
comments: true
date: 2023-11-19
toc_sticky: true
---

## 모수 추정
통계학에는 크게 추론 통계학(Inference Statistical)과 기술 통계학(Descriptive Statistical)이 존재합니다. 여기서 추론 통계학에서도 모수적 추론, 비모수적 추론과 베이지안 추론이 존재합니다.   
추론 통계학은 **표본의 정보를 통해 모집단의 모수를 추정(estimation)하고 검증(test)하는 것**입니다. 즉, 추론 통계학은 **추정(estimation)과 검정(test)**으로 구성됩니다. 여기서 추정은 표본을 추출하여 측정한 결과값을 바탕으로 모집단에 대한 측정 결과로 사용하는 것입니다. 이 추정방법에는 **점 추정(point estimation)**과 **구간 추정(interval estimation)**이 있습니다. 추정을 사용하는 가장 큰 이유는 모집단 전체를 조사할 경우 큰 비용과 시간이 들기 때문입니다.   


### 추정량
이번에는 **비편향 추정량(unbiased estimator)**과 **일치 추정량(consistent estimator)**에 대해 알아보겠습니다. 
먼저 설명했던 비편향 추정량(unbiased estimator)에 대해 간단히 설명하겠습니다. 비편향 추정량은 $E(\hat{\theta}) = \theta$일 때의 $\hat{\theta}$를 비편향 추정량(unbiased estimator)라고 합니다.  
\- 편향(bias) : $Bias(\hat{\theta}) = E(\hat{\theta}) - \theta$  

이제 일치 추정량에 대해 알아보도록 하겠습니다. 일치 추정량을 간단하고 말하면 **표본의 크기 n이 증가함에 따라 $\hat{\theta} \rightarrow \theta$일 때, $\hat{\theta}$를 일치 추정량(consistent estimator)**이라고 합니다.   
