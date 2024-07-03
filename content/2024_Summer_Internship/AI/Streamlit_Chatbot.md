---
title: Streamlit_Chatbot 만들기(클론)
create_date: 2024-07-02 15:07 - 2024-07-02 15:07
draft: true
---
# Streamlit

>[!reference]
>[연구원님 챗봇 프로젝트](https://ppsystem.netlify.app/02-Python/2\)-Python/Streamlit-Chatbot)
>[Streamlit 공식문서 chatbot 예제](https://docs.streamlit.io/develop/tutorials/llms/build-conversational-apps)

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

