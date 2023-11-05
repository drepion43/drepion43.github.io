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

## 자유도와 불편추정량
자유도는 독립 변수의 개수를 의미합니다. 예를 들어, $x + y + z= 8$의 독립 변수의 개수를 구해보겠습니다. 만약, $x=3, y=2$로 변수 값을 지정했다고 하면 자연스럽게 $z=3$으로 결정이납니다. 즉, 설정한 변수값은 2개가 됩니다. **총 n개의 변수가 있을 때, 자유도는 n-1**이 됩니다.