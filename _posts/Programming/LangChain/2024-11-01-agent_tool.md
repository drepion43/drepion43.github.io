---
title:  "Agent1(Tools)"
categories: LangChain
tag: [theory, AI, LLM, LangChain, Agent, Tools]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: Prompt
comments: true
date: 2024-11-01
toc_sticky: true
---
하기의 내용은 <a href="https://wikidocs.net/233801" target="_blank">LangChain 노트</a> 기반으로 작성했습니다.

# Agent
이번 게시글에서는 langchain의 agent 기능에 대해 알아보겠습니다.   
agent의 개념은 어떤 목표를 달성하기 위해 환경과 상호작용하며 의사 결정을 내리고 행동을 취해주는 역할을 하는 것입니다. 그럼 LLM에서의 agent는 이 개념을 그대로 LLM이 더욱 자율적이고 목표 지향적으로 작업을 수행할 수 있게 해주는 컴포넌트라고 할 수 있습니다. agent는 다양한 **도구(Tool)**을 잘 활용하여 주어진 목표를 수행하게 할 수 있습니다. 이 **도구**는 예를 들어 검색 도구, PDF 파싱 등과 같은 것들이 될 수도 있으며 매우 다양할 수 있습니다.    
그럼 이제 Langchain에서의 Agent는 어떤 구성요소로 되어있는지 살펴보겠습니다. 크게 4가지로 구성되어있다고 볼 수 있습니다. **Agent(의사 결정을 담당하는 핵심 컴포넌트), Tools(에이전트가 사용할 수 있는 기능들의 집합), Toolkits(관련된 도구들의 그룹), AgentExecutor(에이전트의 실행을 관리하는 컴포넌트)**로 구성되어 있습니다.   
<br>
그럼 이제 각 구성요소들에 대해 알아보도록 하겠습니다.   

## Tools
Tools는 쉽게 agent, 체인 또는 LLM이 외부 세계와 상호작용하기 위한 인터페이스입니다. 즉, agent가 활용할 도구를 정의하여 Agent 가 추론(reasoning)을 수행할 때 활용하도록 할 수 있게 해주는 것입니다. 여기서 대표적인 도구는 검색 도구인 **Tavily Search**가 있다고 말씀드릴 수 있습니다. 하지만, 직접 custom한 도구를 정의하여 사용할 수도 있습니다. Langchain에서 제공하는 Tools는 <a href="https://python.langchain.com/v0.1/docs/integrations/tools/" target="_blank">Langchain Tools Document</a>에서 확인해볼 수 있습니다.
<br>

### Python REPL
우선 Langchain에서 제공해주는 REPL Tools에 대해 살펴보겠습니다.   
REPL은 Read-Eval-Print Loop의 약어로 유효한 Python 명령어를 입력으로 받아 실행시켜주는 도구입니다. 한 번 사용하는 예시를 살펴보겠습니다.   

```python
from langchain_openai import ChatOpenAI
from langchain_experimental.tools import PythonREPLTool
from langchain_core.prompts import ChatPromptTemplate
from langchain_core.output_parsers import StrOutputParser
from langchain_core.runnables import RunnableLambda

python_tool = PythonREPLTool()

def print_and_execute(code, debug=True):
    if debug:
        print("CODE:")
        print(code)
    return python_tool.invoke(code)

llm = ChatOpenAI(base_url="http://127.0.0.1:1234/v1", temperature=0, api_key="meta-llama-3.1-8b-instruct")


prompt = ChatPromptTemplate.from_messages(
    [
        (
            "system",
            "You are Raymond Hetting, an expert python programmer, well versed in meta-programming and elegant, concise and short but well documented code. You follow the PEP8 style guide. "
            "Return only the code, no intro, no explanation, no chatty, no markdown, no code block, no nothing. Just the code.",
        ),
        ("human", "{input}"),
    ]
)

chain = prompt | llm | StrOutputParser() | RunnableLambda(print_and_execute)

print(chain.invoke("로또 번호 생성기를 출력하는 코드를 작성하세요."))
```

상기의 코드는 LLM 모델에게 특정 작업을 수행하는 python 코드를 짠 후 실행시킨 결과를 요청하는 agent입니다. 그럼 agent에서는 요청한 코드를 작성 후 실행한 결과도 함께 제공해줍니다.    

### Tavily
Tavily에는 크게 2가지 클래스가 존재합니다. TavilySearchResults와 TavilyAnswer입니다. <a href="https://app.tavily.com/" target="_blank">Tavily API</a> 해당 링크에 회원가입 후 Tavily API Key를 발급받아야합니다. Tavily를 이용하려면 발급 받은 Key를 환경세팅 해줘야합니다. 환경세팅은 하기와같이 진행해주시면 됩니다.   
```python
import os

os.environ["TAVILY_API_KEY"] = "TAVILY API 키 입력"
```

우선 langchain에서 제공하는 TavilySearchResults의 인자들에 대해 살펴보겠습니다.   

```python
max_results (int): 반환할 최대 검색 결과 수 (기본값: 5)
search_depth (str): 검색 깊이 ("basic" 또는 "advanced")
include_domains (List[str]): 검색 결과에 포함할 도메인 목록
exclude_domains (List[str]): 검색 결과에서 제외할 도메인 목록
include_answer (bool): 원본 쿼리에 대한 짧은 답변 포함 여부
include_raw_content (bool): 각 사이트의 정제된 HTML 콘텐츠 포함 여부
include_images (bool): 쿼리 관련 이미지 목록 포함 여부
```

그럼 이 인자들을 포함하여 TavilySearch를 수행한 예시코드도 같이 살펴보겠습니다.   
```python
from langchain_openai import ChatOpenAI
import os

os.environ["TAVILY_API_KEY"] = "API-Key"

from langchain_community.tools.tavily_search import TavilySearchResults

# 도구 생성
tool = TavilySearchResults(
    max_results=6,
    include_answer=True,
    include_raw_content=True,
    # include_images=True,
    # search_depth="advanced", # or "basic"
    include_domains=["github.io", "wikidocs.net"],
    # exclude_domains = []
)

print(tool.invoke({"query": "LangChain Tools 에 대해서 알려주세요"}))
```

상기의 예시 코드에서는 include_domain에 github.io 와 wikidocs.net의 url을 넣어줬습니다. 그럼 이 2개의 url을 기반으로 검색을 수행하여 결과를 가져오게됩니다. 반대로 제외시킬 url인 exclude_domains는 비워놓았으며, 최대 가져올 검색 개수는 6으로 설정해 6개에 대한 겁색결과만 반환하게 됩니다.   

<br>
<br>
그럼 이번에는 살짝 Custom화를 한 TavilySearch에 대해 살펴보겠습니다. teddynote의 TavilySearch를 이용해보겠습니다.   
예제를 통해 어떤식으로 TavilySearch를 정의하고 잘 조작하여 사용할 수 있는지 살펴보겠습니다.   

```python
from langchain_teddynote.tools.tavily import TavilySearch

# 기본 예제
tavily_tool = TavilySearch()

# include_domains 사용 예제
# 특정 도메인만 포함하여 검색
tavily_tool_with_domains = TavilySearch(include_domains=["github.io", "naver.com"])

# exclude_domains 사용 예제
# 특정 도메인을 제외하고 검색
tavily_tool_exclude = TavilySearch(exclude_domains=["ads.com", "spam.com"])

# 다양한 파라미터를 사용한 검색 예제
result1 = tavily_tool.search(
    query="유튜버 테디노트에 대해서 알려줘",  # 검색 쿼리
    search_depth="advanced",  # 고급 검색 수준
    topic="general",  # 일반 주제
    days=7,  # 최근 7일 내 결과
    max_results=10,  # 최대 10개 결과
    include_answer=True,  # 답변 포함
    include_raw_content=True,  # 원본 콘텐츠 포함
    include_images=True,  # 이미지 포함
    format_output=True,  # 결과 포맷팅
)

# 뉴스 검색 예제
result2 = tavily_tool.search(
    query="최신 AI 기술 동향",  # 검색 쿼리
    search_depth="basic",  # 기본 검색 수준
    topic="news",  # 뉴스 주제
    days=3,  # 최근 3일 내 결과
    max_results=5,  # 최대 5개 결과
    include_answer=False,  # 답변 미포함
    include_raw_content=False,  # 원본 콘텐츠 미포함
    include_images=False,  # 이미지 미포함
    format_output=True,  # 결과 포맷팅
)

result3 = tavily_tool_with_domains.search(
    query="파이썬 프로그래밍 팁",  # 검색 쿼리
    search_depth="advanced",  # 고급 검색 수준
    max_results=3,  # 최대 3개 결과
)

# 특정 도메인 제외 검색 예제
result4 = tavily_tool_exclude.search(
    query="건강한 식단",  # 검색 쿼리
    search_depth="basic",  # 기본 검색 수준
    days=30,  # 최근 30일 내 결과
    max_results=7,  # 최대 7개 결과
)

print("기본 검색 결과:", result1)
print("뉴스 검색 결과:", result2)
print("특정 도메인 포함 검색 결과:", result3)
print("특정 도메인 제외 검색 결과:", result4)
```

사익의예시 코드에 대해 살펴보겠습니다. 우선 tavily_tool은 가장 기본적인 default의 TavilySearch가 되며, tavily_tool_with_domains은 지정한 2개의 url에 대해서만, tavily_tool_exclude은 지정한 2개의 url은 제외하고 검색하는 Tool입니다. 기존 langchain에서 제공하는 TavilySearch와 다른 점은 **topic과 days**의 인자들이 추가되었습니다. 이는 기존 TavilySearch를 오버라이딩 했으며 이에 추가적으로 주제가 어떤 것인지와 최근 몇일인지에 대해서 가져오는 것을 지정할 수 있습니다. 이런 오버라이딩 하는 방법은 추후에 다시 추가적으로 찾아보고 재작성을 하도록 하겠습니다.   

### Custom Tool
이번에는 Langchain에서 제공하는 Built in Tool 외에 필요한 Tool을 직접 정의하여 사용하는 방법에 대해 살펴보겠습니다. 이를 위해서는 langchain.tools 모듈에서 제공하는 tool 데코레이터를 이용하여 함수를 도구로 변환하여 사용해주면됩니다.   


```python
from langchain.tools import tool

@tool
def add_numbers(a: int, b: int) -> int:
    """Add two numbers"""
    return a + b

@tool
def multiply_numbers(a: int, b: int) -> int:
    """Multiply two numbers"""
    return a * b

print(add_numbers.invoke({"a": 3, "b": 4}))
print(multiply_numbers.invoke({"a": 3, "b": 4}))
```

상기의 예시처럼 더하기 도구와 곱하기 도구를 직접 함수로 생성한 후 Tool 데코레이터를 이용하여 도구로 변환하여 사용하실수도 있습니다. 좀 더 심화된 Custom Tool은 <a href="https://github.com/teddylee777/langchain-teddynote/tree/main" target="_blank">teddynote-langchain</a>을 참고하여 작성해보시면 좋을 것 같습니다. 이에 대해서도 추후에 정리를 해보겠습니다.   

## Binding Tools
이전까지 Tool이 어떤건지 Tool을 어떻게 정의할 수 있는지에 대해 배웠습니다. 그럼 이제 이 Tool을 사용하는 방법, 즉, LLM 모델이 Tool을 호출하려면 어떻게 해야하는지에 대해 알아봐야합니다. 이론적으로 말씀드리면, LLM 모델이 Tool Schema를 전달 받아야 Tool에 정의된 내용을 동작할 수 있습니다. 이 Tool을 호출하기 위해서 Langchain의 LLM 모델에서 .bind_tools()를 제공합니다. 즉, 해당 LLM 모델에 정의한 Tool을 묶어두어 해당 LLM 모델은 그 Tool을 이용한다는 의미가 됩니다.   
예시 코드를 작성하여 이에 대해 설명해보며 알아보도록 하겠습니다.   
```python
from langchain.tools import tool
import re
import requests
from bs4 import BeautifulSoup

@tool
def get_word_length(word: str) -> int:
    """Returns the length of a word."""
    return len(word)

@tool
def add_function(a: float, b: float) -> float:
    """Adds two numbers together."""
    return a + b

@tool
def naver_news_crawl(news_url: str) -> str:
    """Crawls a 네이버 (naver.com) news article and returns the body content."""
    # HTTP GET 요청 보내기
    response = requests.get(news_url)

    # 요청이 성공했는지 확인
    if response.status_code == 200:
        # BeautifulSoup을 사용하여 HTML 파싱
        soup = BeautifulSoup(response.text, "html.parser")

        # 원하는 정보 추출
        title = soup.find("h2", id="title_area").get_text()
        content = soup.find("div", id="contents").get_text()
        cleaned_title = re.sub(r"\n{2,}", "\n", title)
        cleaned_content = re.sub(r"\n{2,}", "\n", content)
    else:
        print(f"HTTP 요청 실패. 응답 코드: {response.status_code}")

    return f"{cleaned_title}\n{cleaned_content}"

# tool 정의
tools = [get_word_length, add_function, naver_news_crawl]

from langchain_openai import ChatOpenAI

llm = ChatOpenAI(base_url="http://127.0.0.1:1234/v1", temperature=0, api_key="meta-llama-3.1-8b-instruct")
llm_with_tools = llm.bind_tools(tools)

from langchain_core.output_parsers.openai_tools import JsonOutputToolsParser

chain = llm_with_tools | JsonOutputToolsParser(tools=tools)

tool_call_results = chain.invoke("What is the length of the word 'teddynote'?")
print(tool_call_results, end="\n\n==========\n\n")
```

상기의 예시 코드를 살펴보면, 직접 custom Tool을 정의했는데, 더하기 Tool, 곱하기 Tool, 네이버 뉴스 크롤링 Tool 3개를 정의했습니다. 그런 후 3개의 tool을 리스트로 묶어준 후, bind_tools을 이용하여 LLM 모델에 바인딩 시켜줬습니다. LLM의 out을 JsonOutputToolsParser의 형태로 바꿔서 출력되게 하면, LLM 모델이 어떤 Tool을 이용하여 출력을 냈는지를 확인해볼 수 있습니다.