---
title:  "Relevance Check(LangChain)"
categories: LangChain
tag: [theory, AI, LLM, LangChain, Retrieval]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: Prompt
comments: true
date: 2024-12-01
toc_sticky: true
---
하기의 내용은 <a href="https://github.com/teddylee777/langchain-teddynote" target="_blank">langchain-teddynote Github</a>기반으로 작성했습니다.

# Evaluator
이번 절에서는 Custom Relevance Check를 할 수 있도록 Class를 직접 구현해보겠습니다. 문서기반 QA시스템에서 Retrieval의 점수와 LLM의 답변의 점수 등을 사용자의 질의와 얼마나 관련이 있는지를 확인해볼 수 있습니다.   

<br>
Retrieval에서 정말 잘 답변을 할 수 있는지를 검사하기 위해 크게 3가지에 대한 관련성을 검사하는 Class를 만들어보겠습니다.   
① 질문과 답변간의 관련성   
② 질문과 Retrieval간의 관련성  
③ 답변과 Retrieval간의 관련성   
이 3가지에 대해 관련성이 높다면, RAG가 정말 잘 작동했다고 신뢰를 할 수 있을 겁니다.   
그럼 구현을 해보겠습니다.   
우선 상기의 3가지에 대한 **구조화된 답변**을 내뱉을 수 있도록 LLM을 만들어 보겠습니다.   

```python
"""질문과 답변간의 관련성"""
class QuestionAnswerScore(BaseModel):
    score : str = Field(
        description="relevant or not relevant. Answer 'yes' if the answer is relevant to the question else answer 'no'"
    )

"""질문과 Retrieval간의 관련성"""    
class QuestionRetrievalScore(BaseModel):
    score : str = Field(
        description="relevant or not relevant. Answer 'yes' if the question is relevant to the retrieved document else answer 'no'"
    )

"""답변과 Retrieval간의 관련성"""
class AnswerRetrievalScore(BaseModel):
    score : str = Field(
        description="relevant or not relevant. Answer 'yes' if the answer is relevant to the retrieved document else answer 'no'"
    )
```


<br>
그럼 이어서 상기의 구조화된 답변을 이용하여 Relevance를 검사하는 Class를 구현해보겠습니다.   

```python
class RelevanceChecker:
    """
    해당 class는 input에 대한 타입에 따라 관련성을 평가하는 클래스입니다.
    관련이 있다면 "yes", 없다면 "no"를 반환합니다.
    
    크게 3가지의 타입에 관련한 관련성 평가합니다.
    (Question-Answer, Question-Retrieval, Answer-Retrieval)
    """
    
    def __init__(self, llm: object, types: str = "question-answer"):
        self.llm = llm
        self.types = types
        
    
    def create(self):
        """
        구조화된 LLM
        prompt 생성
        """            
        if self.types == "question-answer":
            model = self.llm.with_structured_output(QuestionAnswerScore)
            template = """You are a grader assessing whether an answer appropriately addresses the given question. \n
                Here is the question: \n\n {question} \n\n
                Here is the answer: {answer} \n
                If the answer directly addresses the question and provides relevant information, grade it as relevant. \n
                Consider both semantic meaning and factual accuracy in your assessment. \n
                
                Give a binary score 'yes' or 'no' score to indicate whether the answer is relevant to the question."""
            input_vars = ["question", "answer"]
            
        elif self.types == "question-retrieval":
            model = self.llm.with_structured_output(QuestionRetrievalScore)
            template = """You are a grader assessing whether a retrieved document is relevant to the given question. \n
                Here is the question: \n\n {question} \n\n
                Here is the retrieved document: \n\n {context} \n
                If the document contains information that could help answer the question, grade it as relevant. \n
                Consider both semantic meaning and potential usefulness for answering the question. \n
                
                Give a binary score 'yes' or 'no' score to indicate whether the retrieved document is relevant to the question."""
            input_vars = ["question", "context"]
            
        elif self.types == "answer-retrieval":
            model = self.llm.with_structured_output(AnswerRetrievalScore)
            template = """You are a grader assessing relevance of a retrieved document to a user question. \n 
                Here is the retrieved document: \n\n {context} \n\n
                Here is the answer: {answer} \n
                If the document contains keyword(s) or semantic meaning related to the user answer, grade it as relevant. \n
                
                Give a binary score 'yes' or 'no' score to indicate whether the retrieved document is relevant to the answer."""
            input_vars = ["context", "answer"]
        else:
            raise ValueError(f"Invalid Types : {self.types}")
        
        prompt = PromptTemplate(template=template, input_variables=input_vars)
        
        chain = prompt | model
        return chain 
```

상기의 코드에 대해 살펴보겠습니다. types는 3가지로 고정을 시켜놓았고, 3가지 types에 대해 서로 다른 프롬프트를 생성해줍니다. Question-Answer, Question-Retrieval, Answer-Retrieval 이렇게 3가지에 대해 관련성 점검을 하며, 관련성이 있다면 "yes", 없다면 "no"를 내뱉어냅니다. 그럼 이 chain을 이용하여 한 번 관련성 검사를 해보겠습니다. 그럼 우선 Retrieval를 만들어 보겠습니다.   

```python
class RAGChain:
    def __init__(self, pdf, llm):
        self.llm = llm
        self.pdf = pdf
    
    def create_retriever(self, 
                         embeddings: object = OpenAIEmbeddings(model="text-embedding-ada-002")):
        loader = PyPDFLoader(self.pdf)

        text_splitter = RecursiveCharacterTextSplitter(chunk_size=1000, chunk_overlap=100)
        split_docs = loader.load_and_split(text_splitter)
        vector = FAISS.from_documents(split_docs, embeddings)
        retriever = vector.as_retriever()
        return retriever

        
    def format_docs(self, docs):
        """검색된 문서들을 하나의 문자열로 포맷팅"""
        context = ""
        for doc in docs:
            context += doc.page_content
            context += '\n'
        return context
            

    def invoke(self, inputs):
        question = inputs.get("question", "")
        context = inputs.get("context", [])
        if isinstance(context, list):
            context = self.format_docs(context)
        history = inputs.get("chat_history", [])
        history = " ".join(history)
        
        template = """
        다음 정보는 이전 대화에 대한 내용입니다.
        {chat_history}
        
        다음 정보를 바탕으로 질문에 답하세요:
        {context}

        질문: {question}
        
        주어진 질문에만 답변하세요. 문장으로 답변해주세요.
        답변:
        """
        prompt = PromptTemplate.from_template(template)
        
        rag_chain = (
            prompt
            | self.llm
            | StrOutputParser()
        )
        answer = rag_chain.invoke({"chat_history": history, "context":context, "question":question })
        
        return answer
rag_chain = RAGChain(llm=llm, pdf="./data/AI_brief_2023년_2월호.pdf")
retriever = rag_chain.create_retriever()
```

상기의 Retireval 클래스를 통해 Question-Retrieval과 Question-Answer간의 관련성 검사를 한 번 해보겠습니다.   
```python
# Q-R
question_retrieval_relevant = RelevanceChecker(  
        llm=ChatOpenAI(model="gpt-4o-mini", temperature=0), types="question-retrieval"  
    ).create()
retriever_result = retriever.invoke("비정형 데이터에대해 알려줘")
response  = question_retrieval_relevant.invoke({"question": "비정형 데이터에 대해 알려줘.", "context":retriever_result})
print(response)

# Q-A
question_answer_relevant = RelevanceChecker(  
        llm=ChatOpenAI(model="gpt-4o-mini", temperature=0), types="question-answer"  
    ).create()
answer = rag_chain.invoke(
    {
        "question": "비정형 데이터에 대해 알려줘.",
        # "context": retriever_result,
        "chat_history": [],
    }
)
response  = question_answer_relevant.invoke({"question": "비정형 데이터에 대해 알려줘.", "answer":answer})
print(response)
print(answer)
```

