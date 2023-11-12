---
title:  "수리 통계1 정리2"
categories: statistics
tag: [DeepLearning,theory, python,statistics,math]
tags: [Jekyll, MathJax]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: 수리 통계1의 내용 정리
comments: true
date: 2023-11-09
toc_sticky: true
---

## 조건부 분포

\- $X \| Y = y: Y = y$로 주어졌을 때, X의 조건부 분포

\- $f_{X \| Y}(x \| y) = \frac{f(x, y)}{f_Y (y)} : Y = y$로 주어졌을 때, X의 조건부 확률 믿도 함수   

\- 조건부 확률 밀도 함수의 성질   
&nbsp;&nbsp;&nbsp;&nbsp;① $f_{X \| Y} (x \| y) \ge 0$    
&nbsp;&nbsp;&nbsp;&nbsp;② $\int_{- \infty}^{\infty} f_{X \| Y} (x \| y) dx = 1$   
&nbsp;&nbsp;&nbsp;&nbsp;③ $P(a \le X \le b \| Y = y) = \int_{a}^{b} f_{X \| Y} (x \| y) dx$

## 기댓값

\- $E(X) = \int_{- \infty}^{\infty} x f(x) dx$

\- $E(g(X)) = \int_{- \infty}^{\infty} g(x) f_X() dx$

\- $E(X \| Y = y) = \int_{- \infty}^{\infty} x f_{X \| Y}(x \| y) dx$

\- $E(E(X \| Y)) = E(X)$

\- $Var(X) = Var(E(X \| Y)) + E(Var(X \| Y))$

\- 적률 생성 함수(mgf) : $M(t) = E(e^{tX})$   
&nbsp;&nbsp;① $X \; \sim \; B(n,p)$일 때, $M(t) = (1 - p + pe^t)^n$   
&nbsp;&nbsp;① $X \; \sim \; Poisson(\lambda)$일 때, $M(t) = e^{\lambda(e^t - 1)}$   
&nbsp;&nbsp;① $X \; \sim \; Gamma(\alpha, \lambda)$일 때, $M(t) = (\frac{\lambda}{\lambda - t})^{\alpha}$   
&nbsp;&nbsp;④ $X \; \sim \; N(\mu,\sigma^2)$일 때, $M(t) = e^{\mu t + \frac{1}{2} \sigma^2 t^2}$   
&nbsp;&nbsp;⑤ $X \; \sim \; X_n^2(Chi)$일 때, $Gamma(\frac{n}{2}, \frac{1}{2})$인 $M(t) = (\frac{\frac{1}{2}}{\frac{1}{2} - t})^{\frac{n}{2}}$   

## 랜덤 샘플
\- $X_1, X_2, X_3, ..., X_n$ : 랜덤 샘플 &rarr; 서로 독립이며 같은 분포를 가지는 확률 변수, i.i.d(independent and identically distribution)   

\- 자유도 n인 t-분포($t_n$)   
&nbsp;&nbsp;&nbsp;&nbsp; $\frac{N(0, 1)}{\sqrt{X_n^2(Chi) / n}} \; \sim \; t_n$   

\- 자유도 m, n인 F-분포($F_{m, n}$)   
&nbsp;&nbsp;&nbsp;&nbsp; $\frac{X_m^2(Chi) / m}{X_n^2(Chi) / n} \; \sim \; F_{m, n}$   

\- $X_1 \; \sim \; X_m^2(Chi), \; X_2 \; \sim \; X_n^2(Chi)$이고 서로 독립일 때, $X_1 + X_2 \; \sim \; X_{m+n}^2$   

## 정규 랜덤 샘플

$X_1, X_2, X_3, ..., X_n$이 정규 분포 $N(\mu, \sigma^2)$으로부터의 랜덤 샘플일 때,   
&nbsp;&nbsp; ① $\bar{X} \; \sim \; N(\mu, \frac{\sigma^2}{n}$   
&nbsp;&nbsp; ① $\frac{(n-1)S^2}{\sigma^2} \; \sim \; X_{n-1}^2(Chi)$   
&nbsp;&nbsp; ① $\bar{X}$와 $S^2$은 서로 독립     
&nbsp;&nbsp; ① $\frac{\sqrt{n} (\bar{X} - \mu)}{S} \; \sim \; t_{n-1}$   

## 순서 통계량
\- $X_1, X_2, X_3, ..., X_n$ : pdf와 cdf의 랜덤 샘플   

\- $X_{(1)} \le X_{(2)} \le X_{(3)} \le ... \le X_{(n)}$ : 순서 통계량   

\- $X_{(k)}$의 확률 분포   
&nbsp;&nbsp; \* $f_{X_{(k)}}(x) = \frac{n!}{(k-1)!(n-k)!}\[F(x)\]^{k-1} \[1 - F(x) \]^{n-k} f(x)$