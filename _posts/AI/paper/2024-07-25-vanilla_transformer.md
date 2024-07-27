---
title:  "[논문리뷰]Attention is All you need"
categories: paper
tag: [theory, AI, DeepLearning]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: self-attention, encoder, decoder
comments: true
date: 2024-07-25
toc_sticky: true
---

2023년 3월 이후 openai의 GPT가 매우 큰 호황을 끌고 있습니다. GPT는 생성형 AI라는 대목으로 큰 관심을 받으며 있으며, 이 GPT의 기본 구조는 Transformer의 구조를 따르고 있습니다. 그 이후 다양한 기업에서 생성형 AI를 연구 후 발표하고 있습니다. 대부분의 생성형 AI는 LLM측에서는 Transformer, vision분야에서는 Diffusion Model를 띄고 있습니다. 이번 절에서는 이 Transformer의 구조와 기존 DeepLearning 모델에서의 어떤 한계점을 돌파하고 어떤 차이점이 있는지에 대해 알아보도록 하겠습니다.   
- <https://papers.nips.cc/paper_files/paper/2017/hash/3f5ee243547dee91fbd053c1c4a845aa-Abstract.html>    
본 글은 필자의 주관적인 생각이 담겨있으므로 의역 및 오역이 존재할 수 있다는 점 양해 부탁드립니다.   

---

### Abstract
기존에 주로 사용되던 시퀀스 모델은 인코더와 디코더가 포함된 복잡한 RNN 또는 CNN기반의 모델을 사용했었습니다.    
여기서 이 논문에서는 기존 인코더, 디코더 구조에서 RNN 또는 CNN을 사용하지 않고 attention mechanism을 사용하는 새로운 아키텍처 구조인 Transformer 모델을 제안했습니다.   
실험 결과로 기존 RNN과 CNN기반의 모델보다 더 좋은 성능을 나타내며, 불가능하던 병렬처리도 가능하게 되며 학습 시간을 크게 단축할 수 있었다고 합니다. 

### 1.Introduction
RNN, LSTM, GRU 등과 같은 모델은 언어 모델링이나 기계 번역과 같은 시퀀스 모델링 및 변환 문제에 좋은 성과를 나타내왔었습니다. 추가적으로 인코더, 디코더를 아키텍처까지 확장하여 추가적으로 성능을 더 개선해왔습니다.   
<br>
**RNN**   
<img src="../../../assets/images/paper/2024-07-25-vanilla_transformer/RNN.png" alt="RNN" style="zoom:80%;" />    
하지만, RNN 계열의 모델에서는 한계점이 존재했습니다. 상기의 이미지에서 볼 수 있듯이 RNN계열의 모델은 데이터들을 position t에 따라 순차적으로 처리해야합니다. 즉, hidden state인 $h_2$를 구하기 위해서는 입력으로 $h_1$이 필요합니다. 이를 일반화하면 $h_t$를 구하기 위해서는 입력으로 $h_{t-1}$이 필요하다는 의미가 됩니다. 이런 조건으로 인해 RNN계열은 시퀀스의 길이가 길어질수록 Gradient Vanishing 문제를 야기하며, 긴 시퀀스 데이터를 병렬 처리도 어려우며, 메모리와 엄청난 큰 계산 비용이 듭니다.   
<br>
**Attention Mechanism**   
사실 attention mechanism은 이번 논문에서 처음 소개된 기술 스택은 아닙니다. 기존 인코더, 디코더 시퀀스 모델에서도 RNN모델과 함께 attention mechanism을 이용하기도 했었습니다. attention은 input 또는 output 데이터에서 sequence distance에 무관하게 서로 간의 dependencies를 모델링합니다.    
여기서 차이점은 이번 논문에서는 **전적으로 Attention만을 이용하여 아키텍처를 구축**해서 엄청난 효율성과 성능을 확인했다는 점입니다.   

### 2.Background
CNN기반의 Extended Neural GPU, ByteNet, ConvS2S 등에서도 모두 시퀀스 연산을 줄이기 위한 연구를 수행했었습니다. 이 모델들은 입출력간의 관련성을 파악하기 위해 거리에 따라 계산량이 증가합니다. 따라서 입출력간의 거리가 멀수록 관련성을 파악하는데 어려움이 존재합니다. 하지만, Transformer는 attention의 가중치의 평균을 이용하는 **Multi-head Attention을 이용하여 상수 시간의 연산**을 가능하게 합니다.   
<br>
**Self-Attention(intra-attention)**은 단일 시퀀스 안에서 서로 다른 위치에 있는 요소들의 관련성을 찾아내는 방법입니다. 즉, 이름 그대로 자기 자신에게 attention을 수행하는 기법입니다.   
<br>
End-to-end memory network에서는 단순 시퀀스 기반의 recurrence를 이용하는것 대신에 attention기반의 recurrence인 recurrent attention mechanism을 사용했는데, 단순 언어 질문 응답 및 언어 모델링 작업에서 좋은 성능을 보이는 것으로 입증되었습니다.   
<br>
**Sequence Model**   
여기서 기존 시퀀스 모델의 가장 큰 문제점인 **장거리 의존성**이 존재합니다.   
대부분 시퀀스 모델에서는 인코더-디코더 구조를 가지는 recurrent model인 RNN이나 CNN 등을 주로 사용했습니다. 하지만, 이는 시퀀스의 특성상 **순차적인 특성이 유지되지만 시간이 지날수록 과거에 대한 의존성에 대해서는 취약**해진다는 단점이 존재합니다.    
- RNN : 시퀀스의 길이가 길어질수록 정보 압축 문제 존재
- CNN : Convolution의 filter 크기를 넘어서는 시퀀스에 대해 알기는 어려움

**Transformer**   
Transformer는 recurrence없이 **Attention Mechanism**만을 이용해 의존성을 찾을 수 있습니다. 즉, Self-Attention에만 의존하므로 기존의 RNN이나 CNN에서 존재하던 문제점을 무시할 수 있습니다.   

### 3.Model Architecture
대부분의 좋으 성능을 띄는 시퀀스 변환 모델은 인코더-디코더 구조를 가지고 있습니다.    
우선 인코더-디코더의 구조를 띄는 가장 대표적인 Sequent-to-Sequence 모델에 대해 간략하게 설명하여 추후에 설명할 Transformer의 이해를 도와주도록 하겠습니다.   
<br>
**Sequence-to-Sequence**   
<img src="../../../assets/images/paper/2024-07-25-vanilla_transformer/seq2seq.png" alt="https://wikidocs.net/22893" style="zoom:80%;" />    
Seq2Seq는 사실상 Transformer의 논문에 나오진 않지만, 많은 분들이 Transformer의 장점을 설명할 때, 해당 모델의 방법과 비교하여 설명하곤 합니다.    
계속하여 설명했듯이 RNN은 시퀀스 순서대로 데이터가 입력되는데, hidden state $h_{t+1}$을 구하기 위해서는 hidden state $h_{t}$가 입력으로 필요하게 됩니다. 이 말은 즉슨, 현재 $h_t$에는 이전 시간대에 대한 모든 정보들인 $t=1,2,...,t-1$까지의 정보를 모두 내포하고 있다고 할 수 있습니다.   
상기의 이미지를 보면 **인코더의 최종 output은 Context라는 vector**라고 합니다. 이 context vector를 디코더의 입력으로 넣어주게 됩니다. 이 때 context vector는 메모리와 계산 비용 때문에 **context의 길이는 제한**되어야 합니다. 따라서 긴 시퀀스 데이터를 처리할 때는 **제한된 context의 길이 때문에 엄청난 정보 손실**이 일어날 수 있습니다.    
이러한 문제를 해결하기 위해 Transformer는 인코더의 모든 state를 디코더에 참조시키기 위해 **Attention**기법을 이용했습니다.   

#### 3-1.Encoder and Decoder Stacks
<img src="../../../assets/images/paper/2024-07-25-vanilla_transformer/transformer.jpg" alt="transformer" style="zoom:80%;" />    

