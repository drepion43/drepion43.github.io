---
title:  "수리 통계1 정리1"
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

## 확률

 \- 확률의 구성 : 표본공간($\Omega$), 사건(A), 확률(P)   

 \- 확률의 공리 :    
&nbsp;&nbsp;&nbsp;&nbsp;① $0 \le P(A) \le 1$    
&nbsp;&nbsp;&nbsp;&nbsp;② $P(\Omega) = 1$   
&nbsp;&nbsp;&nbsp;&nbsp;③ $A_1, A_2, A_3, ...$이 서로 배반일 때, $P(\cup^{\infty}_{i=1} A_i) = \sum P(A_i)$   

 \- 조건부 확률 : 사건 A가 발생했다는 정보가 주어져있을 때, 사건 B가 발생할 확률   
&nbsp;&nbsp;&nbsp;&nbsp; $P(B \| A) = \frac{P(A \cap B)}{P(A)}$   

## 확률 변수

\- X : 표본 공간에서 정의된 실수로의 함수   

\- 누적 분포 함수(cdf) : 확률 밀도 함수의 적분(pdf, = 확률), $F(x) = P(X \le x) = \int_{- \infty}^{x} f(x)dx$   

\- 이산형 확률 변수 : X가 가질 수 있는 값이 유한개 혹은 셀 수 있는 확률 변수, $p(x_i) = P(X = x_i)$인 확률 질량 함수(pmf)   
&nbsp;&nbsp;&nbsp;&nbsp;① $p(x_i) \ge 0$  
&nbsp;&nbsp;&nbsp;&nbsp;② $\sum_i p(x_i) = 1$  
&nbsp;&nbsp;&nbsp;&nbsp;③ $P(a \le X \le b) = \sum_{i:a \le x_i \le b} p(x_i)$ 

\- 연속형 확률 변수 : X가 일정한 구간에 속하는 모든 값을 가지는 확률 변수, $f(x) = \frac{d}{dx}F(x)$인 확률 밀도 함수(pdf)
&nbsp;&nbsp;&nbsp;&nbsp;① $f(x) \ge 0$   
&nbsp;&nbsp;&nbsp;&nbsp;② $\int_{- \infty}^{\infty} f(x) dx = 1$   
&nbsp;&nbsp;&nbsp;&nbsp;③ $P(a \le X \le b) = F(b) - F(a) = \int_{a}^{b} f(x) dx$

## 확률 분포

### 이산형 확률 분포

\- 베르누이 분포 : $X \; \sim \; Bernoulli(p)$   
&nbsp;&nbsp;&nbsp;&nbsp; $p(k)=p^k (1-p)^{1-k}$(k= 0 or 1)   
&nbsp;&nbsp;&nbsp;&nbsp; $E(X)=p, Var(X)=p(1-p)$   

\- 이항 분포 : $X \; \sim \; B(n, p)$   
&nbsp;&nbsp;&nbsp;&nbsp; $p(k) = nC_k p^k (1-p)^{n-k}$(k = 0,1,2,3,..., n)   
&nbsp;&nbsp;&nbsp;&nbsp; $E(X) = np, Var(X) = np(1-p)$

\- 포아송 분포 : $X \; \sim \; Poisson(\lambda)$   
&nbsp;&nbsp;&nbsp;&nbsp; $p(k) = \frac{\lambda^k e^{- \lambda}}{k!}$  
&nbsp;&nbsp;&nbsp;&nbsp; $E(X) = \lambda , Var(X) = \lambda$

### 연속형 확률 분포

\- 균등 분포 : $X \; \sim \; Uniform\[a, b\]$   
&nbsp;&nbsp;&nbsp;&nbsp; $f(x) = \frac{1}{b-a} (a \le x \le b)$  
&nbsp;&nbsp;&nbsp;&nbsp; $E(X) = \frac{a+b}{2} , Var(X) = \frac{(b-a)^2}{12}$   

\- 지수 분포 : $X \; \sim \; Exp(\lambda)$   
&nbsp;&nbsp;&nbsp;&nbsp; $f(x) = \lambda e^{- \lambda x} ( x \ge 0)$  
&nbsp;&nbsp;&nbsp;&nbsp; $E(X) = \frac{1}{\lambda} , Var(X) = \frac{1}{\lambda^2}$   

\- 감마 분포 : $X \; \sim \; Gamma(\alpha, \lambda)$   
&nbsp;&nbsp;&nbsp;&nbsp; $f(x) = \frac{1}{\Gamma(\alpha)} \lambda^{\alpha} x^{\alpha - 1} e^{- \lambda x} (x \ge 0)$  
&nbsp;&nbsp;&nbsp;&nbsp; $E(X) = \frac{\alpha}{\lambda} , Var(X) = \frac{\alpha}{\lambda^2}$   
&nbsp;&nbsp;&nbsp;&nbsp; $Gamma(1, \lambda) = Exp(\lambda), Gamma(\frac{n}{2}, \frac{1}{2}) = X_n^2(chi)$   
&nbsp;&nbsp;&nbsp;&nbsp; $\frac{\sum_{i=1}^{n} (x_i - \bar{x})^2}{\sigma^2} \; \sim \; X_{n-1}^2(Chi)$이며, $E(x) = n-1, V(x) = 2(n-1)$

\- 정규 분포 : $X \; \sim \; N(\mu, \sigma^2)$   
&nbsp;&nbsp;&nbsp;&nbsp; $f(x) = \frac{1}{\sqrt{2 \pi \sigma^2}} e^{- \frac{(x - \mu)^2}{2 \sigma^2}} (- \infty \< x \< \infty)$  
&nbsp;&nbsp;&nbsp;&nbsp; $E(X) = \mu , Var(X) = \sigma^2$   

## 변수 변환

\- X의 확률 분포로부터 $Y = g(X)$의 확률 분포를 구하는 방법

\- X가 연속형 확률 변수이고 $g(.)$의 역함수가 존재하고 미분가능할 때, $Y=g(X)$의 확률 밀도 함수는 하기와 같습니다
&nbsp;&nbsp;&nbsp;&nbsp; $f_Y(y) = f_X(g^{-1}(y)) \| \frac{d}{dy} g^{-1}(y) \|$   

\- example)   
&nbsp;&nbsp;\* $X \; \sim \; N(\mu, \sigma^2)$일 때, $Y = aX + b \; \sim \; N(a \mu + b, a^2 \sigma^2)$   
&nbsp;&nbsp;\* $X \; \sim \; N(0, 1)$일 때, $Y = X^2 \; \sim \; X_1^2(Chi)$   
&nbsp;&nbsp;\* $X \; \sim \; Gamma(\alpha, \lambda)$일 때, $Y = cX \; \sim \; Gamma(\alpha, \frac{\lambda}{c})$   

## 합의 분포
서로 독립인 두 확률 변수 $X_1$과 $X_2$에 대하여 하기와 같습니다.   
\- $X_1 \; \sim \; Poisson(\lambda_1)$이고 $X_2 \; \sim \; Poisson(\lambda_2)$일 때, $X_1 + X_2 \; \sim \; Poisson(\lambda_1 + \lambda_2)$   
\- $X_1 \; \sim \; N(\\mu_1, \sigma_1^2)$이고 $X_2 \; \sim \; N(\mu_2, \sigma_2^2)$일 때, $X_1 + X_2 \; \sim \; N(\mu_1 + \mu_2, \sigma_1^2 + \sigma_2^2)$   
\- $X_1 \; \sim \; Gamma(\alpha_1, \lambda)$이고 $X_2 \; \sim \; Gamma(\alpha_2, \lambda)$일 때, $X_1 + X_2 \; \sim \; Gamma(\alpha_1 + \alpha_2, \lambda)$   
\- $X_1 \; \sim \; X_{n_1}^2(Chi)$이고 $X_2 \; \sim \; X_{n_2}^2(Chi)$일 때, $X_1 + X_2 \; \sim \; X_{n_1 + n_2}^2 (Chi)$   
