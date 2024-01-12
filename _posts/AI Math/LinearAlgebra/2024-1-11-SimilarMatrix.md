---
title:  "유사 행렬(Similar Matrix)"
categories: LinearAlgebra
tag: [theory, math]
tags: [Jekyll, MathJax]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: Similar Matrix
comments: true
date: 2024-1-11
toc_sticky: true
---

## Similar Matrix
우선 Similar Matrix의 정의부터 살펴보겠습니다.   
**Similar Matrix는 n x n 짜리 행렬 $A$에 대하여 $B=P^{-1}AP$를 만족하는 n x n 짜리 P가 존재하면, A와 B는 Similar Matrix**라고 합니다.    

Similar Matrix의 Property에 대해 알아보겠습니다.   
① $A$는 스스로는 Similar Matrix합니다.   
&rarr; $A=P^{-1}AP$에서 $P=I$가 존재해서 해당 수식을 만족합니다.     

② $B$가 $A$와 Similar하면, $A$도 $B$와 Similar합니다.(교환 법칙)   
&rarr; $B=P^{-1}AP$를 만족하는 $P$가 존재한다면, 넘겨준다면 $A=PBP^{-1}$가 되어 상기의 꼴과 동일하게 됩니다.($PBP^{-1}$에서 만약 $P=K^{-1}$라고 한다면 $K^{-1}BK$가 되어 동일한 형태가 됩니다.)     

③ $A$가 $B$와 Similar하고, $B$가 $C$와 Similar하다면, $A$와 $C$도 Similar합니다.(삼단 논법)   
&rarr; $B=P_1^{-1}AP_1$, $C=P_2^{-1}BP_2$인데, $B$를 넣어보겠습니다. $C = P_2^{-1}(P_1^{-1}AP_1)P_2$가 됩니다. 만약 $P=P_1P_2$라고 한다면, $P^{-1} = P_2^{-1}P_1^{-1}$가 되어 $C=P^{-1}AP$꼴이 됩니다.     

④ $A$와 $B$가 Similar하다면, 둘의 rank도 동일합니다.   
&rarr; 만약, $B=P^{-1}AP$일 때, rank의 성질 중 **Full-Rank끼리의 행렬 곱에서 큰 rank**를 따라갑니다. 따라서 $A,P$가 Full-Rank이고 $rank(A) > rank(P)$라면, $rank(AP) = rank(A)$를 만족합니다. 따라서, $P^{-1}$도 Full-Rank가 되며, $P$와 같은 rank를 가집니다. $rank(A) = rank(AP) = rank(P^{-1}AP) = rank(B)$가 됩니다.   

⑤ $A$와 $B$가 Similar하다면, **trace가 동일**합니다.   
&rarr; trace의 cyclic property를 이용하면 $tr(B) = tr(P^{-1}AP) = tr(APP^{-1}) = tr(AI) = tr(A)$가 되어 둘의 trace는 동일합니다.   

⑥ $A$와 $B$가 Similar하다면, 둘의 determinant도 동일합니다.   
&rarr; determinant의 property인 행렬 곱의 determinant는 쪼개질 수 있습니다. $det(B) = det(P^{-1}AP) = det(P^{-1})det(A)det(P) = \frac{1}{det(P)}det(A)det(P) = det(A)$가 됩니다.   

⑦ $A$와 $B$가 Similar하다면, **eigen value도 동일**합니다.(하지만, **eigen vector는 동일하지 않습니다.**)    
\begin{aligned}    
A \underline{v} =& \lambda \underline{v} \newline   
P^{-1} A PP^{-1} \underline{v} =& P^{-1} \lambda \underline{v} \newline   
B P^{-1} \underline{v} =& P^{-1} \lambda \underline{v} \; (B=P^{-1} A P) \newline   
\end{aligned}    

&rarr; 상기의 수식에서 $B P^{-1} \underline{v} = P^{-1} \lambda \underline{v}$을 얻을 수 있습니다. 즉, eigen value인 $\lambda$는 $A$와 동일합니다. 하지만, eigen vector는 $A$의 eigen vector에서 $P^{-1}$만큼 선형변환이 일어난 것이 $B$의 eigen vector가 됩니다.    

⑧ $A$와 $B$가 Similar하다면, $A^n$과 $B^n$도 Similar합니다.   
&rarr; $B=P^{-1}AP$에서 $B^2=P^{-1}APP^{-1}AP = P^{-1}AIAP = P^{-1}A^2 P$을 만족합니다.
