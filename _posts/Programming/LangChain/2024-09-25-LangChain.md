---
title:  "LangChain이란"
categories: LangChain
tag: [theory, AI, LLM, LangChain]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: Retrieval
comments: true
date: 2024-09-25
toc_sticky: true
---
하기의 내용은 <a href="https://wikidocs.net/book/14473" target="_blank">LangChain 입문부터 응용까지</a> 기반으로 작성했습니다.

# LangChain이란
LangChain은 최근 ChatGPT와 같이 LLM이 뜨고 있는데, 이를 간편하게 LLM 애플리케이션을 개발할 수 있게 해주는 프레임워크이자 오픈소스입니다. LangChain을 이용하면 매우 간단하게 LLM모델을 이용하며, 애플리케이션을 개발할 수 있습니다. 따라서 2022년부터 크게 인기를 끌고 있는 프레임워크입니다.    
LangChain은 LLM 애플리케이션 개발을 도움을 주는 여러 구성 요소로 이루어졌습니다. 하기에 구성요소에 대해 알아보겠습니다.   
1. **랭체인 라이브러리**
    - 다양한 컴포넌트의 인터페이스들을 통합하며, 체인과 에이전트를 결합할 수 있는 기본 런타임, 그리고 체인과 에이전트의 사용 가능한 구현을 지원합니다.
2. **랭체인 템플릿**
    - 참조 아키텍처의 모음, 개발자들이 특정 작업에 맞춰 빠르고 편리하게 애플리케이션 개발을 할 수 있도록 지원합니다.
3. **랭서브**
    - 랭체인 체인을 REST API로 배포할 수 있게 지원해주는 라이브러리, 개발자들은 이를 통해 쉽게 외부 시스템과 통합할 수 있습니다.
4. **랭스미스**
    - 개발자 플랫폼으로, LLM 체인 디버깅, 평가, 모니터링이 가능하며, 랭체인과 원활한 통합을 지원합니다.   


# LLM Chain
LLM의 기본 체인은 Prompt + LLM 입니다. Chain은 LLM 애플리케이션 개발에 핵심적인 개념이라고 할 수 있습니다. 즉, 체인은 사용자의 입력(프롬프트)을 받아 LLM을 통해 적절한 응답이나 결과를 생성하는 구조를 말합니다.   
Chain의 구성요소는 하기와 같습니다.   
1. **Prompt**
    - 사용자 또는 시스템에서 제공하는 입력으로, LLM에게 특정 작업을 수행하도록 요청하는 지시문입니다. 프롬프트는 질문, 명령, 문장 시작 부분 등 다양한 형태를 취할 수 있으며, LLM의 응답을 유도하는 데 중요한 역할을 합니다.

2. **LLM** 
    - GPT, Gemini 등 대규모 언어 모델로, 대량의 텍스트 데이터에서 학습하여 언어를 이해하고 생성할 수 있는 인공지능 시스템입니다. LLM은 프롬프트를 바탕으로 적절한 응답을 생성하거나, 주어진 작업을 수행하는 데 사용됩니다.  
   

<br>
가장 보편화된 작동 방식은 하기와 같습니다.    

1. **프롬프트 생성**
    - 사용자의 요구 사항이나 특정 작업을 정의하는 프롬프트를 생성합니다. 이 프롬프트는 LLM에게 전달되기 전에, 작업의 목적과 맥락을 명확히 전달하기 위해 최적화될 수 있습니다.

2. **LLM 처리**
    - LLM은 제공된 프롬프트를 분석하고, 학습된 지식을 바탕으로 적절한 응답을 생성합니다. 이 과정에서 LLM은 내부적으로 다양한 언어 패턴과 내외부 지식을 활용하여, 요청된 작업을 수행하거나 정보를 제공합니다.

3. **응답 반환**
    - LLM에 의해 생성된 응답은 최종 사용자에게 필요한 형태로 변환되어 제공됩니다. 이 응답은 직접적인 답변, 생성된 텍스트, 요약된 정보 등 다양한 형태를 취할 수 있습니다.

<br>
그럼 이번에 간단하 ChatPrompt를 받아 답변하는 Chain을 구성한 예시에 대해 알아보겠습니다.   
```python
from langchain_core.prompts import ChatPromptTemplate
from langchain_openai import OpenAI
from langchain_core.output_parsers import StrOutputParser

# llm 모델 정의
llm = OpenAI(base_url="http://127.0.0.1:1234/v1", api_key="meta-llama-3.1-8b-instruct")

# prompt 생성
prompt = ChatPromptTemplate.from_template("You are an expert in astronomy. Answer the question. <Question>: {input}")

# out shape 정의
output_parser = StrOutputParser()

# chain 생성
chain = prompt | llm | output_parser

# chain 호출
chain.invoke({"input": "지구의 자전 주기는?"})
```

langchain_core의 prompts 모듈에서 ChatPromptTemplate 클래스를 사용하여 해당 분야의 전문가로서 질문에 답변하는 형식의 프롬프트 템플릿을 생성할 수 있습니다. ChatPromptTemplate.from_template() 메소드는 문자열 형태의 템플릿을 인자로 받으며, <Question>: {input} 부분에서 실제 질문을 받아 답변하도록 요청하실 수 있습니다.  
StrOutputParser는 모델의 출력을 문자열 형태로 파싱하여 최종 결과를 반환하도록 하는 메서드입니다.  
따라서 **LCEL**을 이용하여 chain을 구성했으며, prompt의 output이 LLM의 input으로 들어가고 LLM에서 뱉은 결과가 output_parser로 들어가 문자열 형태로 답변이 나오게 됩니다.   


## Multi-Chain
