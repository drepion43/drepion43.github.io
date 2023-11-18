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
추론 통계학은 **표본의 정보를 통해 모집단의 모수를 추정(estimation)하고 검증(test)하는 것**입니다. 즉, 추론 통계학은 **추정(estimation)과 검정(test)**으로 구성됩니다. 여기서 추정은 표본을 추출하여 측정한 결과값을 바탕으로 모집단에 대한 측정 결과로 사용하는 것입니다. 이 추정방법에는 **점 추정(point estimation)**과 **구간 추정(interval estimation)**이 있습니다. 추정을 사용하는 가장 큰 이유는 모집단 전체를 조사할 경우 큰 비용과 시간이 들기 때문입니다.   

### 점 추정(point estimation)
점 추정은 표본을 이용하여 모집단의 모수를 단일한 값으로 추정하는 방법입니다. 예를 들어, 이전에 배웠던 표본 평균과 표본 분산으로 모집단의 평균과 분산을 추정하는 것입니다. 단일한 값들을 추정량이라 합니다.     
요약하자면 점추정은 $X= (X_1, X_2, X_3, ..., X_n)$을 이용하여 모수 $\theta$를 한 값으로 추측하는 것입니다.   
\- 모수(parameter) : $\theta$(unknown)   
\- 추정량(estimator) : $\hat{\theta} = \hat{\theta}(X_1, X_2, X_3, ... , X_n)$   
\- 추정값(estimate) : 자료를 이용하여 구한 추정량의 값

### 적률 추정법
이전 절에서 배운 적률 생성 함수(mgf)의 미분을 통해 n차 적률을 얻을 수 있었습니다. 이렇게 얻은 적률은 적률 추정법을 통해 모수를 추정하기 위해 사용됩니다. 즉, 적률 추정법은 n차 모적률과 n차 표본 집단의 적률을 일치시켜 모수를 추정하는 방법입니다.   
\- n차 모적률(population moment) : $\mu_n(\theta) = E(X^n)$   
\- n차 표본 적률(sample moment) : $\bar{X^n} = \frac{1}{k} \sum_{i=1}^{k} X_i^n$   
즉, $\mu_n(\theta) = \bar{X^n}$을 만족하는 $\theta$의 추정량을 정하는 방법입니다. 이제 적률 추정법의 몇 문제를 풀어보겠습니다.   

- $X_1, X_2, X_3, ..., X_n$이 $X \sim B(m, \theta)$로부터 추출한 샘플일 때, $\theta$의 적률 추정량을 구해보겠습니다.   
우선 이항 분포의 모적률은 $\mu(\theta) = m \theta$입니다. 따라서, $m \hat{\theta} = \bar{X}$이므로, $\hat{\theta} = \frac{\bar{X}}{m}$이 됩니다.   

- $X_1, X_2, X_3, ..., X_n$이 확률 밀도 함수 $f(x \| \theta) = \frac{1}{2 \theta} e^{- \frac{\| x \|}{\theta}}, - \infty < x < \infty$인 랜덤 샘플일시, $\theta$의 적률 추정량을 구해보겠습니다.   
해당 확률 분포의 기댓값은 $E(X) = 0$이어 1차 적률을 사용하여 모수 추정을 할 수가 없습니다. 그래서 2차 모적률을 구해보겠습니다.   
$E(X^2) = \int_{- \infty}^{\infty} \frac{x^2}{2 \theta} e^{- \frac{\| x \| }{\theta}} dx = 2 \theta^2$이 됩니다. 따라서, $2 \hat{ \theta^2} = \bar{X^2}$이 됩니다. $\hat{\theta} = \sqrt{ \frac{1}{2n} \sum_{i=1}^{n} X_i^2 }$이 됩니다.

- $X_1, X_2, X_3, ..., X_n$이 $X \sim N( \mu, \sigma^2)$로부터 추출한 샘플일 때, $\mu, \sigma^2$의 적률 추정량을 구해보겠습니다.   
정규 분포의 1차 적률과 2차 적률은 $E(X) = \mu, E(X^2) = \mu^2 + \sigma^2$이 됩니다. 그럼 $\mu$와 $\sigma^2$의 적률 추정량을 구해보겠습니다. $\hat{\mu} = \bar{X}$와 $\hat{\mu^2} + \hat{\sigma^2} = \bar{X^2}$이 됩니다. 따라서 $\mu$의 적률 추정량은 $\hat{\mu} = \bar{X}$입니다. $\sigma^2$의 적률 추정량은 $\hat{\sigma^2} = \bar{X^2} - (\bar{X}) ^ 2 = \frac{1}{n} \sum_{i=1}^{n} (X_i - \bar{X})^2$이 됩니다.

적률 추정법은 모수의 추정 방법과 직관적이고 간단하지만 추정량이 가지는 성질이 다른 추정 방법에 비해 우수하지 않을 수도 있습니다. 