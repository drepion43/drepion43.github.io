---
title:  "극좌표계"
categories: test
tag: [theory, math]
tags: [Jekyll, MathJax]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
---

## 극좌표계

이번에는 극좌표계에 대해 알아보겠습니다. 우선 극좌표계는 직교좌표계인 (x, y)를 r(반지름)과 $\theta$(각도)로 나타낸 좌표계입니다. 매우 간단한 개념입니다.   
<img src="../../assets/images/test/2023-11-04-Polar Coordinate/polar coordinate1.jpg" alt="polar coordinate1" style="zoom:80%;" />   
좌표계 (0, 0)에서 r만큼 떨어진 점의 좌표는 상기의 이미지처럼 $(x, y)=(r cose(\theta), r sine(\theta))$로 표현할 수 있습니다. $r^2 = x^2 + y^2 = r^2 cos^2(\theta) + r^2 sin^2 (\theta)$로 정의됩니다. 또한, $tan (\theta) = \frac{y}{x}$는 역을 취해주면 $arctan(\frac{y}{x})=tan^{-1}(\frac{y}{x}) = \theta$가 됩니다. 

## 호의 길이
반지름이 r인 원에서 중심각 크기가 $\theta$인 부채꼴의 호의 길이를 $l$이라 하고 넓이를 $S$라고 해보겠습니다.    
<img src="../../assets/images/test/2023-11-04-Polar Coordinate/polar coordinate2.jpg" alt="polar coordinate2" style="zoom:80%;" />   
원의 둘레길이와 부채꼴의 길이에(호의 길이) 대한 비례식을 세워보겠습니다.   
$2 \pi : 2 \pi r = \theta : l$   
상기의 비례식에서 $l=r \theta$임을 알 수 있습니다.   
이번에는 원의 넓이와 부채꼴의 넓이에 대한 비례식을 세워보겠습니다.   
$2 \pi : \pi r^2 = \theta : S$   
$S = \frac{ \pi r^2 \theta}{2 \pi} = \frac{1}{2}r^2 \theta$   
부채꼴의 넓이에 호의 길이를 이용하여 나타내면 $S= \frac{1}{2}l r$이 됩니다.  

## 극좌표계 이중 적분
<img src="../../assets/images/test/2023-11-04-Polar Coordinate/polar coordinate3.jpg" alt="polar coordinate3" style="zoom:80%;" />   
우선 $d \theta$의 적분이 증가할수록 사분면 원의 넓이가 채워집니다. 부채꼴의 넓이는 호의 길이 $l$과 반지름의 길이 $r$을 통해 구할 수 있습니다.   
우선 반지름에 대한 $dr$을 적분해준 다음 호에 대한$dl$을 적분해주면 되는데, $dl = rd \theta$가 됩니다.    
$dxdy$ &rarr; $rdr d\theta$가 됩니다.    
예시를 들어 보겠습니다.   
<img src="../../assets/images/test/2023-11-04-Polar Coordinate/polar coordinate4.jpg" alt="polar coordinate4" style="zoom:80%;" />
$\int_{- \infty}^{\infty} \int_{0}^{\infty} e^{- (x^2 + y^2)} dx dy$   
상기의 식은 직교좌표계로 풀리지 않습니다. 따라서, 극좌표계로 변형을 해줘야합니다.   
범위를 우선 생각해보겠습니다. x는 (0, $\infty$)이고, y는 ($- \infty$, $\infty$)의 범위를 가집니다. 이 범위를 극좌표계의 범위로 변형을 해주면, 반지름 $r$의 범위는 항상 양수이니 (0, $\infty$)가 됩니다. 그리고 $\theta$의 범위는 1사분면과 4사분면으로 구성되니 (- $\frac{\pi}{2}$, $\frac{\pi}{2}$)이 됩니다.   
$\int_{- \frac{\pi}{2}}^{\frac{\pi}{2}} \int_{0}^{\infty} e^{- r^2} r dr d\theta$가 됩니다.
