---
title:  "순서 통계량"
categories: statistics
tag: [DeepLearning,theory, python,statistics,math]
tags: [Jekyll, MathJax]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: 순서 통계량이란?
comments: true
date: 2023-11-07
toc_sticky: true
---

## 순서 통계량
이번절에서는 순서 통계량에 대해 알아보도록 하겠습니다. 우선 순서 통계량의 쉬운 이해를 위해 예시를 들어 설명해보겠습니다.  

n개의 어떤 제품이 있다고 가정해보겠습니다. 해당 제품이 고장이 난 시기(사용했던 기간)을 확률 변수 $X$라고 하겠습니다. $X$는 서로 독립이며, 동일한 확률 분포를 따릅니다. 이제 해당 제품을 동시에 사용했을 때, 가장 먼저 고장난 제품의 시기를 $X_{(1)}$, 두번 째로 고장났을 때의 시기를 $X_{(2)}$라고 했을 때, 가장 마지막에 고장난 제품의 시기를 $X_{(n)}$라고 하겠습니다. 이럴 때, 이 $X_{(1)}, X_{(2)}, ..., X_{(n)}$들도 새로운 확률 변수라고 정의할 수 있습니다. 이 고장났을 때의 시점을 기준으로 **순서대로** 새롭게 얻어진 $X_{(n)}$의 확률 변수를 **순서 통계량**이라고 합니다. 즉, $X_{(1)}$은 가장 작은 확률 변수, $X_{(2)}$은 두번째로 작은 확률 변수이며, $X_{(n)}$이 가장 큰 확률 변수가 됩니다. 따라서, $X_{(1)} \le X_{(2)} \le X_{(3)} ... \le X_{(n)}$을 만족합니다.    

여기서 중요한 점이 있습니다. 이전 기존 확률 변수들인 $X_1, X_2, ... X_n$는 **서로 모두 독립**이라고 하더라도, $X_{(1)}, X_{(2)}, ..., X_{(n)}$은 **독립이 아닙니다.** $X_{(1)}$보다 $X_{(2)}$의 값이 더 커야합니다. 즉, $X_{(2)}$은 $X_{(1)}$보다 커야한다는 영향을 받는 **종속**의 성질을 갖게 됩니다. 따라서, **순서 통계량은 종속**이 됩니다.   

### 순서 통계량 확률 분포
먼저 최솟값을 가지는 $X_{(1)}$의 확률을 구해보겠습니다.   
$P(X_{(1)} \le x) = 1 - P(X_{(1)} \> x)$   
최솟값인 $X_{(1)}$보다 어떤 $x$가 더 크려면, 모든 값들이 $x$보다 크면 됩니다.   
그럼 $P(X_{(1)} \> x)$을 구해보겠습니다. 모든 확률 변수들 중 가장 최솟값을 나타내면 하기와 같이 나타낼 수 있습니다.   
$P(X_{(1)} \> x) = P(X_1 > x, X_2 > x, X_3 > x, ... , X_n > x)$   
$X_1, X_2, ..., X_n$은 서로 모두 독립이니 $P(X_1 > x)P(X_2 >x)P(X_3 > x)...P(X_n > x)$가 됩니다.    
확률 밀도 함수의 확률은 누적 분포 함수(cdf)이니 하기와 같이 나타낼 수 있습니다.   
$P(X_{(1)} > x) = (1 - F(x))(1 - F(x))(1 - F(x))... = (1 - F(x))^n$   
따라서, $P(X_{(1)} \le x) = 1 - (1 - F(x))^n$이 됩니다.   

이번에는 최댓값인 $X_{(n)}$의 확률을 구해보겠습니다.   
최솟값과 반대로 생각하면 됩니다. $X_{(n)}$보다 모두 작으면 됩니다.   
$P(X_{(n)} \le x) = P(X_1 \le x, X_2 \le x, X_3 \le x, ... , X_n \le x)$   
$X_1, X_2, ..., X_n$은 서로 모두 독립이니 $P(X_1 \le x)P(X_2 \le x)P(X_3 \le x)...P(X_n \le x)$가 됩니다.   
확률 밀도 함수의 확률은 누적 분포 함수(cdf)이니 하기와 같이 나타낼 수 있습니다.   
$P(X_{(n)} \le x) = (F(x))(F(x))(F(x))... = (F(x))^n$   
따라서, $P(X_{(n)} \le x) =(F(x))^n$이 됩니다.    

이제 마지막으로 최솟값과 최댓값 사이에 존재하는 순서통계량의 확률을 구해보겠습니다. 즉, 2번째로 크거나 n-1번째로 큰 확률 변수를 구하면 됩니다.    
$X_{(k)}(2 \le k \le n-1)$   
그럼 **n중 적어도 k개가 x보다 작을 확률**은 하기와 같습니다.   
$nC_k (F(x))^k (1-F(x))^{n - k}$   
따라서, $P(X_{(k)} \le x) = \sum_{i=k}^{n} \binom{n}{i} (F(x))^i (1 - F(x))^{n - i} $이 됩니다.

이제 상기의 유도한 3개의 확률 종합하여 일반화를 해보겠습니다. 모두 확률은 뜻하니 곧 누적 분포 함수(cdf)를 뜻합니다. 확률 분포를 얻기 위해서는 누적 분포 함수(cdf)를 미분해주면 됩니다. 따라서 미분을 해서 최종적으로 하기와같은 **순서 통계량의 확률 밀도 함수**를 얻을 수 있습니다.   
$f_{X_{(k)}} (x) = n \times \binom{n-1}{k-1} \times f(x) \times (F(x))^{k-1} \times (1 - F(x))^{n - k}$