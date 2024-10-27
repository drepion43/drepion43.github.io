---
title:  "Retrieval"
categories: LangChain
tag: [theory, AI, LLM, LangChain]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: Retrieval
comments: true
date: 2024-09-29
toc_sticky: true
---

하기의 내용은 <a href="https://github.com/wikibook/langchain" target="_blank">LangChain Repository</a>의 코드의 기반과 랭체인 완벽입문이라는 책의 내용을 토대로 작성되었습니다.   


# Retrieval란
쉽게 RAG(Retrieval-Augmented Generation)은 언어 모델이 모르는 정보에 대해 대답하게 하는 기법이라고 정의할 수 있습니다.   
즉, RAG는  프롬프트에 정보원의 문장, 질문, 문장을 기반으로 답변하라는 지시를 포함시킴으로써 언어 모델이 알지못하는 정보에 기반한 답변을 생성하게 할 수 있습니다.   

하기에는 문서의 내용입니다.

```python
비행 자동차 고도 제한법
~~
~~
제3조(일반 비행 고도 제한)
도심지에서 비행차가 비행하는 경우 최대 고도는 지상에서 300m로 한다.
도시 외의 지역에서 비행차가 비행하는 경우 최대 고도는 지상에서 500m로 한다.

제4조(특례 비행 고도)
~~
~~~
```   

이 문서에서 "비행 자동차는 지상에서 몇 미터까지 비행할 수 있는가?" 라는 질문에 답하기 위해 RAG는 다음과 같은 정보를 다시 검색하여 가져온 후 프롬프트를 만들어냅니다.   

```python
[프롬프트]
제3조(일반 비행 고도 제한)
도심지에서 비행차가 비행하는 경우 최대 고도는 지상에서 300m로 한다.
도시 외의 지역에서 비행차가 비행하는 경우 최대 고도는 지상에서 500m로 한다.


질문 : 비행 자동차는 지상에서 몇 미터까지 비행할 수 있는가? 

답변 : 도시 지역에서 비행 자동차가 비행하는 경우 최대 고도는 지상에서 300미터입니다.
도시 외의 지역에서 비행차량이 비행하는 경우 최대 고도는 지상에서 500미터입니다. 
단, 긴급 차량, 공공기관 차량 및 관련 공적 임무를 수행하는 차량에 대해서는 이러한 제한을 초과하는 고도에서 비행이 허용될 수 있습니다.
```

<br>
그럼 RAG에서 가장 중요한 것은 **답변에 필요한 문장을 어떻게 검색하고 가져오는지**입니다.   

## 유사 문장 검색
RAG에서는 주어진 데이터에서 질문과 유사한 부분을 찾아내어 프롬프트를 만들어냅니다. 근데, 인간의 언어는 컴퓨터가 이해하기 어렵기때문에 이를 **벡터화**하여 수치적으로 계산하여 유사성을 측정합니다.   

## 벡터 유사도 검색

```python
from langchain.embeddings import OpenAIEmbeddings  #← OpenAIEmbeddings를 가져오기
from numpy import dot  #← 벡터의 유사도를 계산하기 위해 dot을 가져오기
from numpy.linalg import norm  #← 벡터의 유사도를 계산하기 위해 norm을 가져오기

embeddings = OpenAIEmbeddings( #← OpenAIEmbeddings를 초기화
    model="text-embedding-ada-002"
)

query_vector = embeddings.embed_query("비행 자동차의 최고 속도는?") #← 질문을 벡터화

print(f"벡터화된 질문: {query_vector[:5]}") #← 벡터의 일부를 표시

document_1_vector = embeddings.embed_query("비행 자동차의 최고 속도는 시속 150km입니다.") #← 문서 1의 벡터를 얻음
document_2_vector = embeddings.embed_query("닭고기를 적당히 양념한 후 중불로 굽다가 가끔 뒤집어 주면서 겉은 고소하고 속은 부드럽게 익힌다.") #← 문서 2의 벡터를 얻음

cos_sim_1 = dot(query_vector, document_1_vector) / (norm(query_vector) * norm(document_1_vector)) #← 벡터의 유사도를 계산
print(f"문서 1과 질문의 유사도: {cos_sim_1}")
cos_sim_2 = dot(query_vector, document_2_vector) / (norm(query_vector) * norm(document_2_vector)) #← 벡터의 유사도를 계산
print(f"문서 2와 질문의 유사도: {cos_sim_2}")

```   
상기의 코드에서는 코사인 유사도를 측정했습니다. $sim(A,B) = cos(\theta) = \frac{A \dot B}{||A||_2 ||B||_2}$   


## RAG를 위한 사전 준비
그럼 RAG를 적용하기 위해 수행하기전 **사전 절차**에 간단하게 소개하겠습니다.   
① **텍스트 추출**   
텍스트 데이터를 벡터화하기 위해서는 텍스트 데이터를 추출해야합니다. 다행히 텍스트 데이터로 주어진다면 괜찮지만, 예를들어, 표, 이미지 등과 같은 데이터나 PDF와 같은 파일로 주어질 경우, 이를 텍스트 데이터로 잘 파싱해야합니다.   
② **텍스트 분할**   
RAG에 무턱대고 모든 텍스트를 넣으면 안될뿐더라, 보통 언어모델에는 텍스트 길이에 제약이 존재합니다. 이를 위해, 의미가 잘 연결될 수 있도록 쪼개야합니다.   
③ **텍스트 벡터화**   
이제 쪼개고 추출한 텍스트를 컴퓨터가 이해할 수 있는 수치적 표현으로 변환시킬 수 있는 embedding을 적용해 벡터화로 텍스트를 변환시켜줍니다.   
④ **벡터 DB**   
이제 벡터화한 텍스트 데이터를 잘 가지고 있어야하니, 이를 DB에 저장해주는 작업을 해줍니다.   

<br>
사전절차가 끝났다면, 이번에는 RAG의 절차에 대해 간단하게 소개하겠습니다.   
① **사용자의 입력을 벡터화**   
이전 텍스트 벡터화와 동일하게 이번에는 사용자의 입력을 벡터화해줍니다.   
② **문장 가져오기**   
사용자의 입력을 벡터화했다면, 해당 벡터를 가지고 저장했던 벡터DB 유사한 문장을 가져옵니다.   
③ **유사 문장과 질문 조합 후 프롬프트 작성**   
'PromptTemplate'의 변수를 이용하여 유사 문장과 질문을 조합해 새로운 프롬프트를 생성해줍니다.    
④ **언어 모델 호출**   
이제 새로 생성한 프롬프트를 언어모델에 입력해 답변을 얻어냅니다.  

# PDF기반
PDF 데이터를 불러오는 방법은 다양하게 존재합니다. 이번에는 **PyMuPDFLoader**를 이용하는 방법에 대해 알아보겠습니다.    

우선 사전 절차 부분인 텍스트 데이터 추출/분할/벡터화/DB 저장까지의 코드부터 확인해보겠습니다.

```python
from langchain.document_loaders import PyMuPDFLoader
from langchain.embeddings import OpenAIEmbeddings  #← OpenAIEmbeddings 가져오기
from langchain.text_splitter import SpacyTextSplitter
from langchain.vectorstores import Chroma  #← Chroma 가져오기

loader = PyMuPDFLoader("./sample.pdf")
documents = loader.load()

text_splitter = SpacyTextSplitter(
    chunk_size=300, 
    pipeline="ko_core_news_sm"
)
splitted_documents = text_splitter.split_documents(documents)

embeddings = OpenAIEmbeddings( #← OpenAIEmbeddings를 초기화
    model="text-embedding-ada-002" #← 모델명을 지정
)

database = Chroma(  #← Chroma를 초기화
    persist_directory="./.data",  #← 영속화 데이터 저장 위치 지정
    embedding_function=embeddings  #← 벡터화할 모델을 지정
)

database.add_documents(  #← 문서를 데이터베이스에 추가
    splitted_documents,  #← 추가할 문서 지정
)

print("데이터베이스 생성이 완료되었습니다.") #← 완료 알림
```

데이터 추출은 loader.load()를 통해 진행됩니다. 이 때 하나의 문장을 Documents라는 클래스로 표현하는데, 이 클래스에서의 page_content 문서의 내용을 담고 있으며, metadata에는 말 그대로 메타 데이터를 담고 있습니다. 여기서 metadata에는 항목이 특별히 정해지지 않아, **사용자가 원하는 커스터마이징**을 통해 원하는 정보를 넣을수도 있습니다.   
<br>
이번에는 데이터 분할입니다. 다양한 spliter방법이 존재하는데 이번에는 spaCy 모듈을 사용했습니다. spaCy는 문장 분할, 품사 판단, 명사구 추출, 구문 분석 등 다양한 언어 분석 기능을 제공합니다. 여기서 pipeline에서의 "ko_core_news_sm"은 분할에 사용할 언어 모델을 의미합니다. 그리고 chunk_size는 분할할 크기를 의미합니다.   
<br>
다음으로는 데이터 벡터화 및 DB에 저장단계입니다. openAI의 embedding 모델을 이용했으며, Chroma DB를 이용하여 저장합니다. embedding_function에는 사용할 Embedding 모델을 정의해주면 되고, add_document를 통해 문서를 데이터베이스 추가해주면 됩니다. persist_directory를 지정해줌으로써 해당 위치에 데이터가 저장되게 됩니다.   
<br>
<br>
<br>
이번에는 RAG의 절차에 대해 알아보겠습니다.   

```python
from langchain.embeddings import OpenAIEmbeddings
from langchain.vectorstores import Chroma

embeddings = OpenAIEmbeddings(
    model="text-embedding-ada-002"
)

database = Chroma(
    persist_directory="./.data", 
    embedding_function=embeddings
)

documents = database.similarity_search("비행 자동차의 최고 속도는?") #← 데이터베이스에서 유사도가 높은 문서를 가져옴
print(f"문서 개수: {len(documents)}") #← 문서 개수 표시

for document in documents:
    print(f"문서 내용: {document.page_content}") #← 문서 내용을 표시
```

상기의 코드는 벡터DB에서 주어진 문장과 유사도가 높은 데이터를 가지고오는 코드입니다. similarity_search를 통해 주어진 입력인 "비행 자동차의 최고 속도는?"과 비슷한 문장들을 Chroma 벡터DB에서 가져오는 작업을 합니다.   
<br>
그럼 이어서 이버엔 검색 결과와 질문을 조합해 질문에 답하는 코드를 알아보겠습니다.   

```python
from langchain.chat_models import ChatOpenAI  #← ChatOpenAI 가져오기
from langchain.embeddings import OpenAIEmbeddings
from langchain.prompts import PromptTemplate  #← PromptTemplate 가져오기
from langchain.schema import HumanMessage  #← HumanMessage 가져오기
from langchain.vectorstores import Chroma

embeddings = OpenAIEmbeddings(
    model="text-embedding-ada-002"
)

database = Chroma(
    persist_directory="./.data", 
    embedding_function=embeddings
)

query = "비행 자동차의 최고 속도는?"

documents = database.similarity_search(query)

documents_string = "" #← 문서 내용을 저장할 변수를 초기화

for document in documents:
    documents_string += f"""
---------------------------
{document.page_content}
""" #← 문서 내용을 추가

prompt = PromptTemplate( #← PromptTemplate를 초기화
    template="""문장을 바탕으로 질문에 답하세요.

문장: 
{document}

질문: {query}
""",
    input_variables=["document","query"] #← 입력 변수를 지정
)

chat = ChatOpenAI( #← ChatOpenAI를 초기화
    model="gpt-3.5-turbo"
)

result = chat([
    HumanMessage(content=prompt.format(document=documents_string, query=query))
])

print(result.content)
```   

이전 코드와 동일하게 similarity_search를 통해 주어진 입력인 "비행 자동차의 최고 속도는?"과 비슷한 문장들을 Chroma 벡터DB에서 가져옵니다. 그리고 가져온 문서리스트에 있는 **문서내용**을 document_string에 추가해줍니다.   
그리고 PromptTemplate을 사용하여 프롬프틑 생성하기 위한 템플릿을 만들어줍니다. {document}에는 벡터DB에서 추출한 문서 내용들을 프롬프트에 넣어주게되고, 사용자 질의는 {query}에 들어가게 되면 언어모델은 알맞은 답변을 뱉어내게됩니다.   

# RetrievalQA로 QA시스템 구축
**RetrievalQA** 모듈을 이용한다면 QA시스템에 보다 쉽게 개발할 수 있습니다.   
보통 RAG를 이용한 QA시스템을 구축할 때, 기본적인 REtrieval를 이용하여 구현하는 경우도 많습니다. 하지만, RetrievalQA는 구현과정에서 간결하게 구현할 수 있습니다.   

```python
from langchain.chains import RetrievalQA  #← RetrievalQA를 가져오기
from langchain.chat_models import ChatOpenAI
from langchain.embeddings import OpenAIEmbeddings
from langchain.vectorstores import Chroma

chat = ChatOpenAI(model="gpt-3.5-turbo")

embeddings = OpenAIEmbeddings(
    model="text-embedding-ada-002"
)

database = Chroma(
    persist_directory="./.data", 
    embedding_function=embeddings
)

retriever = database.as_retriever() #← 데이터베이스를 Retriever로 변환

qa = RetrievalQA.from_llm(  #← RetrievalQA를 초기화
    llm=chat,  #← Chat models를 지정
    retriever=retriever,  #← Retriever를 지정
    return_source_documents=True  #← 응답에 원본 문서를 포함할지를 지정
)

result = qa("비행 자동차의 최고 속도를 알려주세요")

print(result["result"]) #← 응답을 표시

print(result["source_documents"]) #← 원본 문서를 표시
```

database.as_retriever() 메서드를 통해, databse를 Retrieval 형식으로 변환시켜줍니다.   
RetrievalQA를 사용하는데 가장 중요한 점은 **RetrievalQA에는 반드스 Retrieval가 필요**하다는 점입니다.    
RetrievalQA.from_llm()에서 llm에는 언어모델, retrieval에는 변환한 retrieval를 넣어주며, return_source_documents을 통해, 답변시 참고한 문서도 함께 가져오게할 수 있습니다.   
이렇게 이전 PromptTemplate등과 같이 지정해줄 필요없이 간결하게 QA시스템을 구축할 수 있다는 장점이 존재합니다.   


# WikipediaRetriver


