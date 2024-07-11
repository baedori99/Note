---
title: Streamlit_Chatbot 만들기(클론)
create_date: 2024-07-02 15:07 - 2024-07-02 15:07
draft: false
---
# Streamlit

>[!reference]
>[연구원님 챗봇 프로젝트](https://ppsystem.netlify.app/02-Python/2\)-Python/Streamlit-Chatbot)  
>[Streamlit 공식문서 chatbot 예제](https://docs.streamlit.io/develop/tutorials/llms/build-conversational-apps)  
>[langchain_StrOutputParser()](https://data-science-hi.tistory.com/233)  

---
## 설치
- `pip install streamlit`

---
## Streamlit ChatBot 만들기

>[!summary]
>1. 채팅창 만들기
>2. 채팅 저장하기
>3. LLM과 연결하기

---
### 쓰이는 주요 함수

- `st.chat_message(name)` : 메시지 작성자의 이름을 적는 함수
- `st.markdown(body)` :  문자열을 마크다운 형식으로 작성되게 하는 함수
- `st.chat_input(placeholder="")` : 채팅 입력창이 비어있을 경우 써져있는 글을 적용하는 함수
- `st.session_state` : 각 사용자 세션 재실행 간에 변수를 공유하는 함수(값을 유지시킨다.)

---
### 1. 채팅창 만들기

```python
import streamlit as st

# 인사말
greeting = "안녕하세요. 챗봇입니다."
st.chat_message("assistant").markdown(greeting)

# 입력창
question = st.chat_input(placeholder="메세지 입력")

# 메시지가 입력되면 user의 메세지 출력
if question:
    st.chat_message("user").markdown(question)
```

- 출력값 (안녕하세요는 내가 작성한 말)
![](https://imgur.com/8cmkVuG.jpg)

>[!error] 메세지가 저장 안되는 문제 발생 -> 전송 버튼을 누르면 초기화된다.

---
### 2. 채팅 저장하기

```python
import streamlit as st

# 빈 리스트 만들기
if "messages" not in st.session_state:
    st.session_state["messages"] = []

# 첫 채팅을 시작할 때 첫 인사 출력
if len(st.session_state["messages"]) == 0:
    greeting = "안녕하세요. 챗봇입니다."
    st.chat_message("assistant").markdown(greeting)
    st.session_state["messages"].append({"role":"assistant","content":greeting})

# 채팅 기록이 있을 때 기록된 채팅 출력
else:
    for chat in st.session_state["messages"]:
        st.chat_message(chat["role"]).markdown(chat["content"])

# 입력창
question = st.chat_input(placeholder="메세지 입력")

# 채팅이 입력되었을 때
if question:
    # 입력된 채팅 출력
    st.chat_message("user").markdown(question)
    st.session_state["messages"].append({"role":"user", "content":question})

    # 답변 출력
    answer = "즐건 저녁되세요!"
    st.chat_message("assistant").markdown(answer)
    st.session_state["messages"].append({"role":"assistant","content":answer})
```

- 출력값
![](https://imgur.com/4NoJZRl.jpg)

---
### 3. LLM 연결하기

위에서는 `answer`을 고정했다. 이번에는 LLM과 연결하여 대화가 가능하도록 하려고 한다.

- 사용모델: `gpt-3.5-turbo`
- 활용 라이브러리: `langchain`

위의 언급처럼 `user`채팅이 입력되면 초기화되는 문제로 인해 `st.session_state`를 사용하여 재사용할 수 있도록 하는 것이 좋다.

`answer`를 아래 코드로 대체한다.

```python
from dotenv import load_dotenv
load_dotenv()

from langchain_openai import ChatOpenAI
model = ChatOpenAI(model_name="gpt-3.5-turbo")
answer = model.invoke(question).content
```

- 출력값
![](https://imgur.com/OAFPPYz.jpg)

---
### 4. 추가 기능
#### 1. Template를 작성하여 만들기

Template를 작성하여 MBTI의 특징에 대해서 알려주는 챗봇을 만들어보려고 한다.

- Template 작성및 Chain 연결
```python
from langchain_core.output_parsers import StrOutputParser
from langchain_core.prompts import ChatPromptTemplate
from langchain_openai import ChatOpenAI

template = """\
당신은 상대방의 MBTI를 듣고 그 MBTI에 관한 정보를 알려주는 로봇입니다.
MBTI에 관한 질문에만 답변해주세요.
500자 이내로 상대방의 CHAT에 존댓말로 답변해주세요
"""

prompt = ChatPromptTemplate.from_messages(
        [("system", template), ("human", "{input}")]
    )
model = ChatOpenAI(model_name="gpt-3.5-turbo")
chain = prompt | model | StrOutputParser()

answer = chain.invoke({"input": question})
```

-  `StrOutputParser()` :  출력값을 기본 `str`형태로 받는다.

---
##### 최종 코드

![](https://imgur.com/Rhrhg3s.jpg)


```python
import streamlit as st
from dotenv import load_dotenv
from langchain_core.output_parsers import StrOutputParser
from langchain_core.prompts import ChatPromptTemplate
from langchain_openai import ChatOpenAI

load_dotenv()

from langchain_openai import ChatOpenAI

template = """\
당신은 상대방의 MBTI를 듣고 그 MBTI에 관한 정보를 알려주는 로봇입니다.
MBTI에 관한 질문에만 답변해주세요.
500자 이내로 상대방의 CHAT에 존댓말로 답변해주세요
"""

session_key = "chat_history"

st.header("MBTI에 대해 알려주는 챗봇")

# 빈 리스트 만들기
if session_key not in st.session_state:
    st.session_state[session_key] = []

# 저장된 체인 불러오기
if "chain" in st.session_state:
    chain = st.session_state["chain"]

# 처음에 체인 만들기
else:
    prompt = ChatPromptTemplate.from_messages(
        [("system", template), ("human", "{input}")]
    )
    model = ChatOpenAI(model_name="gpt-3.5-turbo")
    chain = prompt | model | StrOutputParser()

# 첫 채팅을 시작할 때 첫 인사 출력
if len(st.session_state[session_key]) == 0:
    greeting = "안녕하세요. 저는 MBTI에 진심인 로봇입니다. 당신의 MBTI는 무엇인가요?"
    st.chat_message("assistant").markdown(greeting)
    st.session_state[session_key].append(
        {"role": "assistant", "content": greeting}
    )

# 채팅 기록이 있을 때 기록된 채팅 출력
else:
    for chat in st.session_state[session_key]:
        st.chat_message(chat["role"]).markdown(chat["content"])

# 입력창
question = st.chat_input(placeholder="메세지 입력")

# 채팅이 입력되었을 때
if question:

    # 입력된 채팅 출력
    st.chat_message("user").markdown(question)
    st.session_state[session_key].append(
        {"role": "user", "content": question}
    )
    answer = chain.invoke({"input": question})
    st.chat_message("assistant").markdown(answer)
    st.session_state[session_key].append(
        {"role": "assistant", "content": answer}
    )
```

---
#### 2. 스트리밍 기능을 추가

>[!reference] 스트리밍관련 출처
>[[LangChain#스트리밍(Streaming)|스트리밍에 관한 노트]]  
>[Callback 관련 LangChain 공식 문서](https://python.langchain.com/v0.1/docs/modules/callbacks/)  
>[[과제 공부용#Callback 함수|Callback함수에 관한 노트]]  

- `class BaseCallbackHandler:`
	- LangChain에서 Callback 함수들을 쓸 수 있게 하는 클래스 
- `def on_llm_new_token(self, token: str, **kwargs: Any) -> Any:`
	- 본문: "Run on new LLM token. Only available when streaming is enabled."
	- 오직 스트리밍 기능이 가능한 경우, 새로운 LLM Token으로 실행한다.

```python
from dotenv import load_dotenv
load_dotenv()

from langchain_openai import ChatOpenAI
from langchain_core.callbacks import BaseCallbackHandler

class CustomHandler(BaseCallbackHandler):
	def __init__(self, container):
		self.container = container
		self.text = ""
	def on_llm_new_token(self, token: str, **kwargs) -> None:
		self.text += token # 토큰 하나씩 추가
		self.container.markdown(self.text) # 하나씩 추가된 토큰 출력

model = ChatOpenAI(model_name="gpt-3.5-turbo",streaming = True) # 스트리밍기능 가능
container = st.empty()
model.callbacks = [CustomHandler(container)]
answer = model.invoke(question).content
```

---
#### 3. 메모리 기능을 추가

>[!reference] 메모리 관련 출처
>[[LangChain#Memory|Memory  관련 노트]]

```python
from langchain_core.prompts import ChatPromptTemplate, MessagesPlaceholder
from langchain_openai import ChatOpenAI
from langchain_core.output_parsers import StrOutputParser
from operator import itemgetter
from langchain.memory import ConversationBufferMemory
from langchain_core.runnables import RunnableLambda, RunnablePassthrough

memory = ConversationBufferMemory(return_messages=True, memory_key="chat_history")
runnable = RunnablePassthrough.assign(
		chat_history = RunnableLambda(memory.load_memory_variables)
		| itemgetter("chat_history")
)
prompt = ChatPromptTemplate.from_messages(
	[
		("system", template),
		MessagesPlaceholder(variable_name="chat_history"),
		("human", "{input}")
	]
)
model = ChatOpenAI(model_name = "gpt-3.5-turbo", streaming = True)
chain = runnable | prompt | model | StrOutputParser()

# 저장되는지 확인
print(memory.load_memory_variables({}))
```

---
## 최종 코드

```python
import streamlit as st
from dotenv import load_dotenv
from langchain_core.output_parsers import StrOutputParser
from langchain_core.prompts import ChatPromptTemplate, MessagesPlaceholder
from langchain_openai import ChatOpenAI

load_dotenv()

from langchain.memory import ConversationBufferMemory
from langchain_core.callbacks import BaseCallbackHandler
from operator import itemgetter
from langchain_core.runnables import RunnableLambda, RunnablePassthrough


template = """\
당신은 상대방의 MBTI를 듣고 그 MBTI에 관한 정보를 알려주는 로봇입니다.
MBTI에 관한 질문에만 답변해주세요.
500자 이내로 상대방의 CHAT에 존댓말로 답변해주세요
"""

session_key = "chat_history"

class CustomHandler(BaseCallbackHandler):
    def __init__(self, container):
        self.container = container
        self.text = ""
    def on_llm_new_token(self, token: str, **kwargs) -> None:
        self.text += token # 토큰 하나씩 추가
        self.container.markdown(self.text) # 하나씩 추가된 토큰 출력

st.header("MBTI에 대해 알려주는 챗봇")

# 빈 리스트 만들기
if session_key not in st.session_state:
    st.session_state[session_key] = []

# 저장된 체인 불러오기
if "chain" in st.session_state:
    chain = st.session_state["chain"]
    memory = st.session_state["memory"]

# 처음에 체인, 메모리 만들기
else:
    # 대화 버퍼 메모리를 생성하고, 메시지 반환 기능을 활성화
    memory = ConversationBufferMemory(return_messages=True, memory_key = "chat_history")
    runnable = RunnablePassthrough.assign(
        chat_history = RunnableLambda(memory.load_memory_variables)
        | itemgetter("chat_history")
    )
    prompt = ChatPromptTemplate.from_messages(
        [("system", template), MessagesPlaceholder(variable_name = "chat_history"), ("human", "{input}")]
    )
    model = ChatOpenAI(model_name="gpt-3.5-turbo", streaming = True)
    chain = runnable | prompt | model | StrOutputParser()

    # 체인 및 메모리 저장
    st.session_state["chain"] = chain
    st.session_state["memory"] = memory

# 첫 채팅을 시작할 때 첫 인사 출력
if len(st.session_state[session_key]) == 0:
    greeting = "안녕하세요. 저는 MBTI에 진심인 로봇입니다. 당신의 MBTI는 무엇인가요?"
    st.chat_message("assistant").markdown(greeting)
    st.session_state[session_key].append(
        {"role": "assistant", "content": greeting}
    )

# 채팅 기록이 있을 때 기록된 채팅 출력
else:
    for chat in st.session_state[session_key]:
        st.chat_message(chat["role"]).markdown(chat["content"])

# 입력창
question = st.chat_input(placeholder="메세지 입력")

# 채팅이 입력되었을 때
if question:

    # 입력된 채팅 출력
    st.chat_message("user").markdown(question)
    st.session_state[session_key].append(
        {"role": "user", "content": question}
    )

    # 답변 출력
    with st.chat_message("assistant"):
        container = st.empty()
        handler = CustomHandler(container)
        answer = chain.invoke(
            {"input": question},
            {"callbacks": [handler]}
            )

    st.session_state[session_key].append({"role": "assistant", "content": answer})
    # 메모리 저장
    memory.save_context(
        {"inputs": question},
        {"output": answer}
    )

    # 확인용 메모리 출력
    print(memory.load_memory_variables({}))
```

- 출력 예시
![](https://imgur.com/yKnlRPw.jpg)
