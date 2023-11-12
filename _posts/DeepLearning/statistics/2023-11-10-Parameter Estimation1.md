---
title:  "모수 추정1"
categories: statistics
tag: [DeepLearning,theory, python,statistics,math]
tags: [Jekyll, MathJax]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: 적률 추정법에 대한 정리
comments: true
date: 2023-11-09
toc_sticky: true
---

## 모수 추정
통계학에는 크게 추론 통계학(Inference Statistical)과 기술 통계학(Descriptive Statistical)이 존재합니다. 여기서 추론 통계학에서도 모수적 추론, 비모수적 추론과 베이지안 추론이 존재합니다.   
추론 통계학은 **표본의 정보를 통해 모집단의 모수를 추정(estimation)하고 검증(test)하는 것**입니다. 즉, 추론 통계학은 **추정(estimation)과 검정(test)**으로 구성됩니다. 여기서 추정은 표본을 추출하여 측정한 결과값을 바탕으로 모집단에 대한 측정 결과로 사용하는 것입니다. 이 추정방법에는 **점 추정(point estimation)과 구간 추정(interval estimation)**이 있습니다. 추정을 사용하는 가장 큰 이유는 모집단 전체를 조사할 경우 큰 비용과 시간이 들기 때문입니다.   

### 점 추정(point estimation)
점 추정은 표본을 이용하여 모집단의 모수를 단일한 값으로 추정하는 방법입니다. 예를 들어, 이전에 배웠던 표본 평균과 표본 분산으로 모집단의 평균과 분산을 추정하는 것입니다. 단일한 값들을 추정량이라 합니다.     
요약하자면 점추정은 $X= (X_1, X_2, X_3, ..., X_n)$을 이용하여 모수 $\theta$를 한 값으로 추측하는 것입니다. 
\- 모수(parameter) : $\theta$(unknown)   
\- 추정량(estimator) : $\hat{\theta} = \hat{\theta}(X_1, X_2, X_3, ... , X_n)$   
\- 추정값(estimate) : 자료를 이용하여 구한 추정량의 값

### 적률 추정법