---
title:  "Agent2(Call)"
categories: LangChain
tag: [theory, AI, LLM, LangChain, Agent, Tools]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: Prompt
comments: true
date: 2024-11-05
toc_sticky: true
---
하기의 내용은 <a href="https://wikidocs.net/233801" target="_blank">LangChain 노트</a> 기반으로 작성했습니다.

# Agent
이번 게시글에서는 langchain의 agent 기능에 대해 알아보겠습니다.   
agent의 개념은 어떤 목표를 달성하기 위해 환경과 상호작용하며 의사 결정을 내리고 행동을 취해주는 역할을 하는 것입니다. 그럼 LLM에서의 agent는 이 개념을 그대로 LLM이 더욱 자율적이고 목표 지향적으로 작업을 수행할 수 있게 해주는 컴포넌트라고 할 수 있습니다. agent는 다양한 **도구(Tool)**을 잘 활용하여 주어진 목표를 수행하게 할 수 있습니다. 이 **도구**는 예를 들어 검색 도구, PDF 파싱 등과 같은 것들이 될 수도 있으며 매우 다양할 수 있습니다.    
그럼 이어서 agent를 어떻게 정의하고 call하는지에 대해 알아보도록 하겠습니다.   
<br>
도구 호출을 사용하면 모델이 하나 이상의 도구가 호출되어야 하는 시기를 감지하고 해당 도구에 전달해야 하는 입력 으로 전달할 수 있습니다. 도구 API 의 목표는 일반 텍스트 완성이나 채팅 API를 사용하여 수행할 수 있는 것보다 더 안정적으로 유효하고 유용한 도구 호출을 반환합니다.    
가장 쉽게 이해할 수 있는 방법인 예시 코드를 작성하고 직접 실행해보며 설명해보겠습니다. 

## Tool Call
우선 Tool Call 예시부터 살펴보겠습니다. 하기의 코드는 GoogleNews의 url에서 RSS 프로토콜을 이용하여 최근에 올라간 게시글을 크롤링하여 가지고 오는 Tool을 정의한 예시 코드입니다.   

```python
from langchain.tools import tool
from typing import List, Dict, Annotated
from langchain_experimental.utilities import PythonREPL
from langchain_teddynote.tools import GoogleNews

@tool
def search_news(query: str) -> List[Dict[str, str]]:
    """Search Google News by input keyword"""
    news_tool = GoogleNews()
    return news_tool.search_by_keyword(query, k=5)

# 도구 생성
@tool
def python_repl_tool(
    code: Annotated[str, "The python code to execute to generate your chart."],
):
    """Use this to execute python code. If you want to see the output of a value,
    you should print it out with `print(...)`. This is visible to the user."""
    result = ""
    try:
        result = PythonREPL().run(code)
    except BaseException as e:
        print(f"Failed to execute. Error: {repr(e)}")
    finally:
        return result

# tool 정의
print(f"도구 이름: {search_news.name}")
print(f"도구 설명: {search_news.description}")
print(f"도구 이름: {python_repl_tool.name}")
print(f"도구 설명: {python_repl_tool.description}")
```

도구의 이름과 Description이 출력되는 것을 확인할 수 있습니다.   
<br>
그럼 이제 도굴들을 정의했으니 Agent에 들어갈 프롬프트와 Agent를 생성해보겠습니다.   

```python
tools = [search_news, python_repl_tool]
from langchain_core.prompts import ChatPromptTemplate

## 프롬프트 생성
prompt = ChatPromptTemplate.from_messages(
    [
        (
            "system",
            "You are a helpful assistant. "
            "Make sure to use the `search_news` tool for searching keyword related news.",
        ),
        ("placeholder", "{chat_history}"),
        ("human", "{input}"),
        ("placeholder", "{agent_scratchpad}"),
    ]
)

## Agent 생성
from langchain_openai import ChatOpenAI
from langchain.agents import create_tool_calling_agent

# LLM 정의
llm = ChatOpenAI(base_url="http://127.0.0.1:1234/v1", temperature=0, api_key="meta-llama-3.1-8b-instruct")

# Agent 생성
agent = create_tool_calling_agent(llm, tools, prompt)
```

상기의 코드는 정의했던 tool들을 리스트로 묶어주고 이 Tool 리스트를 Agent에넘겨주는 작업이 포함되어 있습니다. langchain에서는 create_tool_calling_agent라는 tool을 포함한 agent를 생성해주는 API가 존재합니다.    

<br>
그럼 이제 Agent까지 생성했으니, 이 Agent를 실행시키는 작업을 수행해보겠습니다. langchain에서 제공하는 AgentExecutor는 Tool을 사용하는 Agent를 실행시켜주는 클래스입니다. AgentExecutor의 인자값들부터 우선 살펴보겠습니다.   

```python
agent: 실행 루프의 각 단계에서 계획을 생성하고 행동을 결정하는 에이전트
tools: 에이전트가 사용할 수 있는 유효한 도구 목록
return_intermediate_steps: 최종 출력과 함께 에이전트의 중간 단계 경로를 반환할지 여부
max_iterations: 실행 루프를 종료하기 전 최대 단계 수
max_execution_time: 실행 루프에 소요될 수 있는 최대 시간
early_stopping_method: 에이전트가 AgentFinish를 반환하지 않을 때 사용할 조기 종료 방법. ("force" or "generate")
"force" 는 시간 또는 반복 제한에 도달하여 중지되었다는 문자열을 반환합니다.
"generate" 는 에이전트의 LLM 체인을 마지막으로 한 번 호출하여 이전 단계에 따라 최종 답변을 생성합니다.
handle_parsing_errors: 에이전트의 출력 파서에서 발생한 오류 처리 방법. (True, False, 또는 오류 처리 함수)
trim_intermediate_steps: 중간 단계를 트리밍하는 방법. (-1 trim 하지 않음, 또는 트리밍 함수)
```

기존 LLM과 동일하게 invoke를 통해 Agent를 실행시킬 수 있으며, 추가적으로 **stream**을 호출하면, 최종 출력까지 도달하는데 필요했던 단계들을 확인해볼 수도 있습니다. 이 AgentExecutor는 **정의한 도구가 agent와 호환되는지 검증**, **최대 반복 횟수 및 실행 시간 제한 기능**, **출력 파싱 오류에 대한 다양한 처리**, **단계별 과정 스트리밍**, **비동기 실행** 등과 같은 기능도 제공해주고 있습니다.    

```python
## Agent 실행
from langchain.agents import AgentExecutor

# AgentExecutor 생성
agent_executor = AgentExecutor(
    agent=agent,
    tools=tools,
    verbose=True,
    max_iterations=10,
    max_execution_time=10,
    handle_parsing_errors=True,
)

# AgentExecutor 실행
result = agent_executor.invoke({"input": "AI 투자와 관련된 뉴스를 검색해 주세요."})

print("Agent 실행 결과:")
print(result["output"])
```

<br>

그럼 이번에는 어떤 단계를 거치는지에 대해 확인할 수 있는 stream을 통해 확인해보겠습니다. 보통 agent는 크게 **Thought**, **Action**, **Observation** 이 3단계를 거쳐서 실행이됩니다. 하지만, stream의 출력에서는 Action, Observation을 계속 반복하여 목표 달성에 맞는지 evaluation을 통해 최종 결과를 도출합니다. 

```python
## Agent 실행
from langchain.agents import AgentExecutor

# AgentExecutor 생성
agent_executor = AgentExecutor(
    agent=agent,
    tools=tools,
    verbose=False,
    handle_parsing_errors=True,
)
# 스트리밍 모드 실행
result = agent_executor.stream({"input": "AI 투자와 관련된 뉴스를 검색해 주세요."}) 

for step in result:
    # 중간 단계 출력
    print(step)
```

## AgentCallbacks
이번에는 stream을 통해 단계별로 Agent의 Action과 Observation을 확인할 수 있다고 말씀을 드렸습니다. 이를 내가 원하는 형태로 확인해보고 싶은경우에 대해 Custom Function을 구현하여 직접 호출하는 작업에 대해 알아보겠습니다.   

```python
from langchain_teddynote.messages import AgentCallbacks, AgentStreamParser
# 도구 호출 시 실행되는 콜백 함수입니다.
def tool_callback(tool) -> None:
    print("<<<<<<< 도구 호출 >>>>>>")
    print(f"Tool: {tool.get('tool')}")  # 사용된 도구의 이름을 출력합니다.
    print("<<<<<<< 도구 호출 >>>>>>")


# 관찰 결과를 출력하는 콜백 함수입니다.
def observation_callback(observation) -> None:
    print("<<<<<<< 관찰 내용 >>>>>>")
    print(
        f"Observation: {observation.get('observation')[0]}"
    )  # 관찰 내용을 출력합니다.
    print("<<<<<<< 관찰 내용 >>>>>>")


# 최종 결과를 출력하는 콜백 함수입니다.
def result_callback(result: str) -> None:
    print("<<<<<<< 최종 답변 >>>>>>")
    print(result)  # 최종 답변을 출력합니다.
    print("<<<<<<< 최종 답변 >>>>>>")


# AgentCallbacks 객체를 생성하여 각 단계별 콜백 함수를 설정합니다.
agent_callbacks = AgentCallbacks(
    tool_callback=tool_callback,
    observation_callback=observation_callback,
    result_callback=result_callback,
)

# AgentStreamParser 객체를 생성하여 에이전트의 실행 과정을 파싱합니다.
agent_stream_parser = AgentStreamParser(agent_callbacks)

# 질의에 대한 답변을 스트리밍으로 출력 요청
result = agent_executor.stream({"input": "AI 투자관련 뉴스를 검색해 주세요."})

for step in result:
    # 중간 단계를 parser 를 사용하여 단계별로 출력
    agent_stream_parser.process_agent_steps(step)
```

상기의 예시코드는 크게 3가지인 도구를 호출하는 callback, observation을 확인하는 callback, 최종 답변을 확인하는 callback 이렇게 3개의 callback 함수를 정의하고, 이를 묶어준 후, AgentStreamParser에 파싱했습니다. AgentStreamParser는 stream의 중간단계를 streamlit을 통해 출력하게 해주는 메서드입니다.   
<br>
이런 agent를 어떻게 사용할지 등에 대해 잘 고민하고 잘 구성하여 작성한다면 엄청나게 효율적인 agent를 이용할 수 있습니다. 추후 독자들이 조금 더 잘 이해하여 사용할 수 있도록 하는, multi-turn과 유사한 memory를 이용하여 이전 대화내용을 담고 답변을 찾아내는 agent에 대해 마지막으로 예시를 들어 설명해보겠습니다.  

```python
from langchain.tools import tool
from typing import List, Dict, Annotated
from langchain_experimental.utilities import PythonREPL
from langchain_teddynote.tools import GoogleNews

## 도구 생성
@tool
def search_news(query: str) -> List[Dict[str, str]]:
    """Search Google News by input keyword"""
    news_tool = GoogleNews()
    return news_tool.search_by_keyword(query, k=5)

# 도구 생성
@tool
def python_repl_tool(
    code: Annotated[str, "The python code to execute to generate your chart."],
):
    """Use this to execute python code. If you want to see the output of a value,
    you should print it out with `print(...)`. This is visible to the user."""
    result = ""
    try:
        result = PythonREPL().run(code)
    except BaseException as e:
        print(f"Failed to execute. Error: {repr(e)}")
    finally:
        return result

tools = [search_news, python_repl_tool]
from langchain_core.prompts import ChatPromptTemplate

## 프롬프트 생성
prompt = ChatPromptTemplate.from_messages(
    [
        (
            "system",
            "You are a helpful assistant. "
            "Make sure to use the `search_news` tool for searching keyword related news.",
        ),
        ("placeholder", "{chat_history}"),
        ("human", "{input}"),
        ("placeholder", "{agent_scratchpad}"),
    ]
)

## Agent 생성
from langchain_openai import ChatOpenAI
from langchain.agents import create_tool_calling_agent

# LLM 정의
llm = ChatOpenAI(base_url="http://127.0.0.1:1234/v1", temperature=0, api_key="meta-llama-3.1-8b-instruct")

# Agent 생성
agent = create_tool_calling_agent(llm, tools, prompt)

## Agent 실행
from langchain.agents import AgentExecutor

# AgentExecutor 생성
agent_executor = AgentExecutor(
    agent=agent,
    tools=tools,
    verbose=False,
    handle_parsing_errors=True,
)

from langchain_teddynote.messages import AgentCallbacks, AgentStreamParser


# 도구 호출 시 실행되는 콜백 함수입니다.
def tool_callback(tool) -> None:
    print("<<<<<<< 도구 호출 >>>>>>")
    print(f"Tool: {tool.get('tool')}")  # 사용된 도구의 이름을 출력합니다.
    print("<<<<<<< 도구 호출 >>>>>>")


# 관찰 결과를 출력하는 콜백 함수입니다.
def observation_callback(observation) -> None:
    print("<<<<<<< 관찰 내용 >>>>>>")
    print(
        f"Observation: {observation.get('observation')[0]}"
    )  # 관찰 내용을 출력합니다.
    print("<<<<<<< 관찰 내용 >>>>>>")


# 최종 결과를 출력하는 콜백 함수입니다.
def result_callback(result: str) -> None:
    print("<<<<<<< 최종 답변 >>>>>>")
    print(result)  # 최종 답변을 출력합니다.
    print("<<<<<<< 최종 답변 >>>>>>")
    
# AgentCallbacks 객체를 생성하여 각 단계별 콜백 함수를 설정합니다.
agent_callbacks = AgentCallbacks(
    tool_callback=tool_callback,
    observation_callback=observation_callback,
    result_callback=result_callback,
)

# AgentStreamParser 객체를 생성하여 에이전트의 실행 과정을 파싱합니다.
agent_stream_parser = AgentStreamParser(agent_callbacks)

from langchain_community.chat_message_histories import ChatMessageHistory
from langchain_core.runnables.history import RunnableWithMessageHistory

# session_id 를 저장할 딕셔너리 생성
store = {}

# session_id 를 기반으로 세션 기록을 가져오는 함수
def get_session_history(session_ids):
    if session_ids not in store:  # session_id 가 store에 없는 경우
        # 새로운 ChatMessageHistory 객체를 생성하여 store에 저장
        store[session_ids] = ChatMessageHistory()
    return store[session_ids]  # 해당 세션 ID에 대한 세션 기록 반환


# 채팅 메시지 기록이 추가된 에이전트를 생성합니다.
agent_with_chat_history = RunnableWithMessageHistory(
    agent_executor,
    # 대화 session_id
    get_session_history,
    # 프롬프트의 질문이 입력되는 key: "input"
    input_messages_key="input",
    # 프롬프트의 메시지가 입력되는 key: "chat_history"
    history_messages_key="chat_history",
)

response = agent_with_chat_history.stream(
    {"input": "안녕? 내 이름은 테디야!"},
    # session_id 설정
    config={"configurable": {"session_id": "abc123"}},
)


# 출력 확인
for step in response:
    agent_stream_parser.process_agent_steps(step)
    
    
# 질의에 대한 답변을 스트리밍으로 출력 요청
response = agent_with_chat_history.stream(
    {"input": "내 이름이 뭐라고?"},
    # session_id 설정
    config={"configurable": {"session_id": "abc123"}},
)

# 출력 확인
for step in response:
    agent_stream_parser.process_agent_steps(step)
    
# 질의에 대한 답변을 스트리밍으로 출력 요청
response = agent_with_chat_history.stream(
    {
        "input": "내 이메일 주소는 teddy@teddynote.com 이야. 회사 이름은 테디노트 주식회사야."
    },
    # session_id 설정
    config={"configurable": {"session_id": "abc123"}},
)

# 출력 확인
for step in response:
    agent_stream_parser.process_agent_steps(step)
    
# 질의에 대한 답변을 스트리밍으로 출력 요청
response = agent_with_chat_history.stream(
    {
        "input": "최신 뉴스 5개를 검색해서 이메일의 본문으로 작성해줘. "
        "수신인에는 `셜리 상무님` 그리고, 발신인에는 내 인적정보를 적어줘."
        "정중한 어조로 작성하고, 메일의 시작과 끝에는 적절한 인사말과 맺음말을 적어줘."
    },
    # session_id 설정
    config={"configurable": {"session_id": "abc123"}},
)

# 출력 확인
for step in response:
    agent_stream_parser.process_agent_steps(step)
```

store라는 session을 계속관리하며, 이 session에 이전 대화내용들을 담고 이를 프롬프팅에 들어가 LLM은 history를 기억하고 답변할 수 있게해주는 간단한 예시입니다. 