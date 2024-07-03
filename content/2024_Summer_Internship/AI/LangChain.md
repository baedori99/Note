---
title: LangChain
create_date: 2024-06-19 15:06 - 2024-06-19 15:06
draft: false
tags:
  - "#AI"
---
>[!reference] 참고자료
>[Langchain 위키독스](https://wikidocs.net/book/14473)  
>[Python Langchain Documentation](https://python.langchain.com/v0.1/docs/get_started/quickstart/)  
>[LangChain정의 관련 블로그](https://www.magicaiprompts.com/docs/langchain/what-is-langchain-innovative-framework-for-llm/)  
>[LangChain모듈 관련 블로그](https://m.post.naver.com/viewer/postView.naver?volumeNo=37460860&memberNo=36733075)  
>[LangChain 관련 IDG Article](https://www.ciokorea.com/column/305341#csidx973f1264e8a2e758d10e50c3f1541b5)  
>[SDK와 API의 차이점](https://doozi0316.tistory.com/entry/SDK-API%EC%9D%98-%EA%B0%9C%EB%85%90%EA%B3%BC-%EC%B0%A8%EC%9D%B4%EC%A0%90)  
>[데이터 파이프라인(ETL과 ELT)](https://seaforest76.tistory.com/27)  
>[상태 저장형 과 비저장형](https://change-words.tistory.com/entry/stateful-stateless)  

## LangChain이란?

- **대규모 언어 모델(LLMs)을 이용한 애플리케이션 개발을 위한 종합적인 프레임워크** 
- 이 프레임워크는 LLM 애플리케이션의 전체 생명주기를 간소화하고 최적화하는 것을 목표로 함.
	- 개발에서부터 프로덕션 단계, 그리고 최종 배포까지 LLM 기반 서비스 구축의 모든 단계를 지원
- 대규모 언어 모델과 애플리케이션의 통합을 간소화하는 SDK이다.
	- SDK(Software Development Kit) : ==소프트웨어 개발 도구 모음==
		- API, IDE, 문서, 라이브러리, 코드 샘플 및 기타 유틸리티 포함
		- 프로그램 및 응용 프로그램 개발의 복잡성을 줄이는 강력한 기능 집합
	- API(Application Programming Interface)
		- ==모듈화하여 만들어진, 어떤 기능을 제어/제공하는 인터페이스==
	- **API는 SDK의 일부가 될 수 있다는점에서 SDK가 API보다 더 큰 개념이다.**

## LangChain 프레임워크의 구성

- 일단 이런 구성이 있구나 정도로 알고 있자...

1. 랭체인 라이브러리(LangChain Libraries):
	- 파이썬과 자바스크립트 라이브러리를 포함
	- 다양한 컴포넌트의 인터페이스와 통합
	- 이 컴포넌트들을 체인과 에이전트로 결합할수 있는 기본 런타임
	- 체인과 에이전트의 사용 가능한 구현이 가능

2. 랭체인 템플릿(LangChain Templates):
	-  다양한 작업을 위한 쉽게 배포할 수 있는 참조 아키텍쳐 모음집
	- 개발자들이 특정 작업에 맞춰 빠르게 애플리케이션을 구축할 수 있도록 도움

3. 랭서브(LangServe):
	- 랭체인 체인을 REST API로 배포할 수 있게 하는 라이브러리
	- 개발자들은 자신의 애플리케이션을 외부 시스템과 쉽게 통합 가능

4. 랭스미스(LangSmith):
	- 개발자 플랫폼
	- LLM 프레임워크에서 구축된 체인을 디버깅, 테스트, 평가, 모니터링
	- 랭체인과의 원활한 통합을 지원

---
## LangChain 흐름도

![랭체인 흐름도](https://imgur.com/fLJsz2B.jpg)

- 랭체인이 어떻게 LLM에서 원하는 결과를 얻기 위한 흐름을 조율하는지 나타낸 그림이다.

#### 데이터 소스(Data Sources)
- 애플리케이션이 LLM에 대한 컨텍스트를 구축하기 위해 PDF, 웹페이지, CSV, 관계형 데이터베이스와 같은 외부 소스에서 데이터를 검색해야하는 경우
	- 랭체인은 서로 다른 소스에서 데이터에 액세스하고 검색할 수 있는 모듈과 원활하게 통합한다.
#### 단어 임베딩(Word Embeddings)
- 일부 외부 소스에서 검색된 데이터는 벡터로 변환되어야한다.
	- 텍스트를 LLM과 관련된 단어 임베딩 모델에 전달하게 된다.
- 랭체인은 선택한 LLM을 기반으로 최적의 임베딩 모델을 선택한다.

#### 벡터 데이터베이스(Vector Database)
- 생성된 임베딩은 유사성 검색을 위해 벡터 데이터베이스에 저장된다.
- 랭체인은 매모리 내 배열부터 파인콘(Pinecone)과 같은 호스팅 벡터 데이터베스에 이르기까지 다양한 소스에서 벡터를 쉽게 저장하고 검색할 수 있도록 지원한다.

#### 대규모 언어모델(LLM)
- 랭체인은 OpenAI, 코히어(Cohere), AI21에서 제공하는 주류 LLM과 허깅페이스(Hugging Face)에서 제공되는 오픈소스 LLM을 지원한다.
- 지원되는 모델과 API 엔드포인트 목록은 빠르게 증가하고 있다.

---
##  LangChain 프레임워크

![랭체인 프레임워크](https://imgur.com/EtH9RMb.jpg)

- 위의 이미지는 랭체인 프레임워크의 핵심이다.
- 스택 상단의 애플리케이션은 파이썬 또는 자바스크립트 SDK를 통해 여러 랭체인 모듈중 하나와 상호 작용한다.

---
### LangChain 모듈

#### 모델 I/O
- 대규모 언어 모델과의 인터페이스
	- 효과적인 프롬프트 생성
	- 모델 API를 호출
	- 결과 해석을 돕는다.
- 생성형 AI의 핵심인 프롬프트 엔지니어링이 랭체인에서 잘처리된다.
	- LLM 제공자가 노출하는 인증, API 매개변수, 엔드포인트를 요약한다.
- 모델에서 보낸 응답을 애플리케이션에서 사용할 수 있는 원하는 방식으로 해석하는 작업을 돕는다.

![](https://imgur.com/81stHc7.jpg)

- **프롬프트를 관리하고 공통 인터페이스를 통해 언어 모델을 호출하고 모델 출력에서 정보를 추출 가능**

#### 데이터 연결
- <U>랭체인의 가장 중요한 구성 요소</U>
- LLM 애플리케이션의 <U>ETL 파이프라인</U>이다.
	- 데이터 파이프라인 :
		- 한 데이터 처리 단계의 출력이 다음 단계의 입력으로 이어지는 형태로 연결된 구조
		- 데이터를 한 장소에서 다른 장소로 옮기는 것
		- 데이터를 이동시킬 수 있는 통로를 만드는 것
		- 데이터를 생성해서 저장하기까지의 일련의 과정
		- ETL
			- 추출(Extract) : 다양한 소스에서 데이터를 수집, 원본 데이터 소스에서 데이터를 뽑아냄
			- 변환(Transformation) : 분석가, 시각화 도구 또는 파이프라인이 제공하는 모든 사용 사례에 유용하게 쓸 수 있게 각 소스 시스템의 원본 데이터를 결합하고 형식을 지정하는 단계
			- 로드(Load) : 원본 데이터 또는 완전히 변환된 데이터를 최종 대상으로 가져옴
				- 데이터를 데이터 저장소에 저장
1. PDF 또는 엑셀 파일과 같은 외부 문서를 로드
2. 이를 처리하기 위해 일괄적으로 단어 임베딩으로 변환
3. 임베딩을 벡터 데이터베이스에 저장
4. 쿼리를 통해 검색

![](https://imgur.com/Nujbhzj.jpg)

- **데이터를 로드, 변환, 저장 및 쿼리하기 위한 빌딩 블록을 제공**

#### 체인
- 한 모듈의 출력이 다른 모듈에 입력으로 전송
- 개발자는 종종 원하는 결과를 얻을 때까지 LLM을 사용해 응답을 명확하게 하고 요약해야 한다.
- 구성 요소와 LLM을 활용하여 예상되는 응답을 얻는 효율적인 파이프 라인을 구축하도록 설계되었다.
- 간단한 체인에는 프롬프트와 LLM이 포함될수 있지만
- 복잡한 체인에는 재귀와 같이 LLM을 여러번 호출하여 결과를 얻도록 구축할 수 있다.
- **복잡한 애플리케이션은 LLM을 상호, 또는 다른 구성요소와 체인으로 연결해야한다.**
	- 랭체인은 `체인으로 연결된` 애플리케이션을 위한 체인 인터페이스 제공

#### 메모리
- LLM은 상태 비저장형이지만 정확한 응답을 위해 컨텍스트가 필요하다.
	- 상태 저장형: 이전 요청을 기억(저장)
	- 상태 비저장형: 이전 요청을 기억❌
- 메모리 모듈은 단기 및 장기 메모리를 쉽게 추가할 수 있도록 도와준다.
- 단기 메모리는 간단한 메커니즘을 통해 대화의 기록을 유지
- 메시지 기록은 레디스(Redis)와 같은 외부 소스에 저장되어 장기 메모리를 유지 가능하다.
- **대화형 시스템은 어느 정도 기간의 과거 메시지에 직접 액세스할 수 있게 하는것이 메모리 기능이다.**

#### 콜백
- 개발자에게 LLM 애플리케이션의 다양한 단계에 연결할 수 있는 콜백 시스템을 제공
- 파이프라인 내에서 특정 상황이 발생할 때 호출되는 사용자 지정 콜백 핸들러를 작성할 수 있게 해준다.
- 랭체인의 기본 콜백은 모든 단계의 출력을 콘솔에 간단히 인쇄하는 stdout을 가리킨다.

#### 에이전트
- 일종의 동적 체인
- 에이전트의 기본 일련의 동작을 선택하기 위해 LLM을 사용하는 것이다.
- 동작의 순서는 체인(코드)으로 하드 코딩된다.
- 언어 모델은 에이전트 내에서 추론 엔진으로 사용되어 어떤 순서로 어떤 동작을 취할지 결정한다.
- **시퀀스를 하드 코딩하는 체인과 달리, 에이전트는 언어 모델을 추론 엔진으로 사용해 어떤 작업을 어느 순서에 따라 수행할지를 결정**

---
## 기본 LLM 체인(Prompt + LLM)

### 기본 LLM 체인
- 사용자의 입력(Prompt)을 받아 LLM을 통해 적절한 응답이나 결과를 생성하는 구조
- 대화형 AI, 자동 문서 생성, 데이터 분석 및 요약 등 다양한 용도로 활용 가능

### 1. 기본 LLM 체인의 구성 요소

1. 프롬프트(Prompt): 
	- 사용자 또는 시스템에서 제공하는 입력
	- LLM에게 특정 작업을 수행하도록 요청하는 지시문
	- 질문, 명령, 문장 시작 부분 등 다양한 형태를 취할 수 있다.
	- <U>LLM의 응답을 유도하는 데 중요한 역할</U>을 한다.

2. LLM(Large Language Model):
	- 대량의 텍스트 데이터에서 학습하여 언어를 이해하고 생성할 수 있는 인공지능 시스템
	- 프롬프트를 바탕으로 적절한 응답을 생성하거나, 주어진 작업을 수행하는데 사용된다.

### 2. 일반적인 작동 방식

1. 프롬프트 생성:
	- 사용자의 요구 사항이나 특정 작업을 정의하는 프롬프트를 생성
	- 이 프롬프트는 LLM에게 전달되기 전
		- 작업의 목적과 맥락을 명확히 전달하기 위해 최적화될 수 있음

2. LLM 처리:
	- LLM은 제공된 프롬프트를 분석하고, 학습된 지식을 바탕으로 적절한 응답을 생성
		- 이 과정에서
			- LLM은 내부적으로 다양한 언어 패턴과 내외부 지식을 활용하여, 요청된 작업을 수행하거나 정보를 제공

3. 응답 반환:
	- LLM에 의해 생성된 응답은 최종 사용자에게 필요한 형태로 변환되어 제공
	- 이 응답은
		- 직접적인 답변
		- 생성된 텍스트
		- 요약된 정보 등
		- 다양한 형태로 가능

---
## LangChain x OpenAI 통합 패키지 설치

- 패키지 설치
```
pip install langchain-openai
```

- API에 엑세스하기 위해선 API 키가 필요하다.
	- API키를 발급받아 `.env`파일에 저장해서 사용할 수 있다.

- 시작 Setting
```python
import os
from dotenv import load_dotenv
from langchain_openai import ChatOpenAI

# API 인증키 받기
load_dotenv()

# 언어 모델 초기화
llm = ChatOpenAI()
```

---
## Prompt

### 예시

```python
sentence = "이거 개재밌음"

prompt = f"""\
{sentence}를 '긍정', '부정'으로 판단해서 이를 '긍정','부정'으로 답변해줘    
"""

llm.invoke(prompt)
```

- 출력값
```
AIMessage(content='긍정', response_metadata={'token_usage': {'completion_tokens': 3, 'prompt_tokens': 52, 'total_tokens': 55}, 'model_name': 'gpt-3.5-turbo', 'system_fingerprint': None, 'finish_reason': 'stop', 'logprobs': None}, id='run-59d3bd3d-8771-4cae-92cd-6cb69e4a9e6d-0', usage_metadata={'input_tokens': 52, 'output_tokens': 3, 'total_tokens': 55})
```

- `prompt = f"""\`
	- `f`는 동적인 데이터를 문자열로 표현할 때 쓰는 방법
	- `\`는 줄이 나눠지는 것이 아닌 한줄로 이을 때 쓰는 방법
- 출력값의 `content`는 결과값
	- 작성자가 원하는 결과값을 가져오는 곳이 `content`

### 프롬프트 작성 원칙

>[!tip] 출처
>[프롬프트 작성 원칙](https://wikidocs.net/231229)

#### 1. 명확성과 구체성
- 질문은 명확하고 구체적이어야 한다.
- 모호한 질문은 LLM 모델의 혼란을 초래할 수 있다.
- 예시:
	- "다음주 주식 시장에 영향을 줄 수 있는 예정된 이벤트들은 무엇일까요?"는
	- "주식 시장에 대해 알려주세요" 보다 더 구체적이고 명확한 질문이다.

#### 2. 배경 정보를 포함
- 모델이 문맥을 이해할 수 있도록 필요한 배경 정보를 제공하는 것이 좋다.
	- 이는 환각 현상(hallucination)이 발생할 위험을 낮추고, 관련성 높은 응답을 생성하는데 도움을 준다.
- 예시:
	- "2020년 미국 대선의 결과를 바탕으로 현재 정치 상황에 대한 분석을 해주세요."

#### 3. 간결함
- 핵심 정보에 초점을 맞추고, 불필요한 정보는 배제한다.
- 프롬프트가 길어지면 모델이 덜 중요한 부분에 집중하거나 상당한 영향을 받는 문제가 발생 가능
- 예시:
	- "2021년에 발표된 삼성전자의 ESG 보고서를 요약해주세요"

#### 4. 열린 질문 사용
- 열린 질문을 통해 모델이 자세하고 풍부한 답변을 제공하도록 유도한다.
- 단순한 "예" 또는 "아니오"로 대답할 수 있는 질문보다는 더 많은 정보를 제공하는 질문이 좋다.
- 예시:
	- "신재생에너지에 대한 최신 연구 동향은 무엇인가요?"

#### 5. 명확한 목표 설정
- 얻고자 하는 정보나 결과의 유형을 정확하게 정의한다.
	- 모델이 명확한 지침에 따라 응답을 생성하도록 돕는다.
- 예시:
	- "AI 윤리에 대한 문제점과 해결 방안을 요약하여 설명해주세요."

#### 6. 언어와 문체
- 대화의 맥락에 적합한 언어와 문제를 선택한다.
	- 모델이 상황에 맞는 표현을 선택하는데 도움이 됩니다.
- 예시:
	- 공식적인 보고서를 요청하는 경우
		- "XX보고서에 대한 전문적인 요약을 부탁드립니다." -> 정중한 문체 사용

### 프롬프트 템플릿

#### PromptTemplate
- 단일 문장 또는 간단한 명령을 입력하여 단일 문장 또는 간단한 응답을 생성하는 데 사용되는 프롬프트를 구성할 수 있는 문자열 템플릿
- Python의 문자열 포멧팅을 사용하여 동적으로 특정한 위치에 입력 값을 포함시킬 수 잇다.

#### 1. 구성요소
- LLM 모델에 입력할 포름프트를 구성할 때 `지시`,`예시`,`맥락`,`질문`과 같은 다양한 구성요소를 조합 가능하다.

| 구분  | 내용                                 |
| --- | ---------------------------------- |
| 지시  | 언어 모델에게 어떤 작업을 수행하도록 요청하는 구체적인 지시. |
| 예시  | 요청된 작업을 수행하는 방법에 대한 하나 이상의 예시.     |
| 맥락  | 특정 작업을 수행하기 위한 추가적인 맥락             |
| 질문  | 어떤 답변을 요구하는 구체적인 질문                |
- 예시
	- 지시: "아래 제공된 제품 리뷰를 요약해주세요."
	- 예시: "예를 들어, '이 제품은 매우 사용하기 편리하며 배터리 수명이 길다.'라는 리뷰는 '사용 편리성과 긴 배터리 수명이 특징'으로 요약할 수 있습니다."
	- 맥락: "리뷰는 스마트 워치에 대한 것이며, 사용자 경험에 초점을 맞추고 있습니다."
	- 질문: "이 리뷰를 바탕으로 스마트 워치의 주요 장점을 두세 문장으로 요약해주세요."

#### 2. 문자열 템플릿
- `langchain_core.prompts`모듈의 `PromptTemplate`클래스를 사용한다.
- 이 예제는 내가 실습했던 유튜브 크롤링 과제를 할 때 긍정/부정 분류기로 쓴 문자열 템플릿이다.

```python
from langchain_core.prompts import PromptTemplate

template = """\
# INSTRUCTION
- 당신은 긍/부정 분류기입니다.
- SENTENCE를 ["긍정", "부정"] 중 하나로 하나의 문자열로 분류하세요.

# SENTENCE: {sentence}
"""

prompt = PromptTemplate.from_template(template)
chain = prompt | llm
```

- `PromptTemplate.from_template` 메서드를 사용하여 문자열 템플릿으로부터 `PromptTemplate`인스턴스를 생성한다.
	- 이때, `template` 변수에 정의된 템플릿 문자열이 사용된다.
- chain은 정의된 프롬프트 템플릿을 LLM과 연결하여 입력프롬프트를 생성하고 결과를 얻는 일련의 작업을 수행한다.
	- `|`연산자는 파이프 연산자로, 프롬프트를 LLM에 입력하고 그 결과를 얻는 과정을 나타낸다.

### 프롬프트 템플릿 활용

>[!reference]
>[랭체인LangChain 노트](https://wikidocs.net/233795)

#### LLMChain 객체

- LLMChain은 특정 PromptTemplate와 연결된 체인 객체를 생성한다.
- 사용법
	- `chain = prompt | llm`
##### 스트리밍(Streaming)

- 대규모 데이터를 한 번에 전송하는 대신, 일정한 속도로  연속적으로 전송하여 처리하는 방식
- 토큰별(글자 하나씩) 출력한다는 뜻
- 스트리밍 옵션은 질의에 대한 답변을 실시간으로 받을 때 유용하다.
- 장점
	1. 실시간 데이터 처리: 데이터를 수신하자마자 즉시 처리할 수 있어 빠른 반응이 요구되는 애플리케인션에 적합하다.
	2. 리소스 효율성: 대규모 데이터를 한 번에 처리하지 않고 나누어 처리하기 때문에 메모리와 CPU 리소스를 효율적으로 사용할 수 있다.
	3. 연속적 분석: 지속적으로 들어오는 데이터를 분석할 수 있어, 실시간 모니터링 및 대응이 가능하다.

---
## Memory

>[!reference]
>[랭체인LangChain 노트 메모리(Memory)](https://wikidocs.net/233773)

### 1. 대화 버퍼 메모리 (ConversationBufferMemory)

- 이 메모리는 메시지를 저장한 다음 변수에 메시지를 추출할 수 있게 한다.
#### 예제

```python
from langchain.memory import ConversationBufferMemory
```

```python
memory = ConversationBufferMemory()
memory.save_context(
    inputs={
        "human": "안녕하세요, 비대면으로 은행 계좌를 개설하고 싶습니다. 어떻게 시작해야 하나요?"
    },
    outputs={
        "ai": "안녕하세요! 계좌 개설을 원하신다니 기쁩니다. 먼저, 본인 인증을 위해 신분증을 준비해 주시겠어요?"
    },
)
```

- memory의 `load_memory_variables({})` 함수는 메시지 히스토리를 반환한다.
- 
```python
# 'history' 키에 저장된 대화 기록을 확인합니다.
memory.load_memory_variables({})
```

```
# 출력값
{'history': 'Human: 안녕하세요, 비대면으로 은행 계좌를 개설하고 싶습니다. 어떻게 시작해야 하나요?\nAI: 안녕하세요! 계좌 개설을 원하신다니 기쁩니다. 먼저, 본인 인증을 위해 신분증을 준비해 주시겠어요?'}
```

- `save_context(inputs, outputs)` 메서드를 사용하여 대화 기록을 저장할 수 있다.
	- 이 메서드는 `inputs`와 `outputs` 두 개의 인자를 받는다.
	- `inputs`은 사용자의 입력을, `outputs` 는 AI의 출력을 저장한다.
	- 이 메서드를 사용하면 대화 기록이 `history` 키에 저장된다.
	- 이후 `load_memory_variables` 메서드를 사용하여 저장된 대화 기록을 확인할 수 있다.

```python
# inputs: dictionary(key: "human" or "ai", value: 질문)
# outputs: dictionary(key: "ai" or "human", value: 답변)
memory.save_context(
    inputs={"human": "네, 신분증을 준비했습니다. 이제 무엇을 해야 하나요?"},
    outputs={
        "ai": "감사합니다. 신분증 앞뒤를 명확하게 촬영하여 업로드해 주세요. 이후 본인 인증 절차를 진행하겠습니다."
    },
)
```

```python
# 2개의 대화를 저장합니다.
memory.save_context(
    inputs={"human": "사진을 업로드했습니다. 본인 인증은 어떻게 진행되나요?"},
    outputs={
        "ai": "업로드해 주신 사진을 확인했습니다. 이제 휴대폰을 통한 본인 인증을 진행해 주세요. 문자로 발송된 인증번호를 입력해 주시면 됩니다."
    },
)
memory.save_context(
    inputs={"human": "인증번호를 입력했습니다. 계좌 개설은 이제 어떻게 하나요?"},
    outputs={
        "ai": "본인 인증이 완료되었습니다. 이제 원하시는 계좌 종류를 선택하고 필요한 정보를 입력해 주세요. 예금 종류, 통화 종류 등을 선택할 수 있습니다."
    },
)
```

```python
# history에 저장된 대화 기록을 확인합니다.
print(memory.load_memory_variables({})["history"])
```

```
# 출력값
Human: 안녕하세요, 비대면으로 은행 계좌를 개설하고 싶습니다. 어떻게 시작해야 하나요?
AI: 안녕하세요! 계좌 개설을 원하신다니 기쁩니다. 먼저, 본인 인증을 위해 신분증을 준비해 주시겠어요?
Human: 사진을 업로드했습니다. 본인 인증은 어떻게 진행되나요?
AI: 업로드해 주신 사진을 확인했습니다. 이제 휴대폰을 통한 본인 인증을 진행해 주세요. 문자로 발송된 인증번호를 입력해 주시면 됩니다.
Human: 인증번호를 입력했습니다. 계좌 개설은 이제 어떻게 하나요?
AI: 본인 인증이 완료되었습니다. 이제 원하시는 계좌 종류를 선택하고 필요한 정보를 입력해 주세요. 예금 종류, 통화 종류 등을 선택할 수 있습니다.
```

```python
# 추가로 2개의 대화를 저장합니다.
memory.save_context(
    inputs={"human": "정보를 모두 입력했습니다. 다음 단계는 무엇인가요?"},
    outputs={
        "ai": "입력해 주신 정보를 확인했습니다. 계좌 개설 절차가 거의 끝났습니다. 마지막으로 이용 약관에 동의해 주시고, 계좌 개설을 최종 확인해 주세요."
    },
)
memory.save_context(
    inputs={"human": "모든 절차를 완료했습니다. 계좌가 개설된 건가요?"},
    outputs={
        "ai": "네, 계좌 개설이 완료되었습니다. 고객님의 계좌 번호와 관련 정보는 등록하신 이메일로 발송되었습니다. 추가적인 도움이 필요하시면 언제든지 문의해 주세요. 감사합니다!"
    },
)
```

```python
# history에 저장된 대화 기록을 확인합니다.
print(memory.load_memory_variables({})["history"])
```

```
# 출력값
Human: 안녕하세요, 비대면으로 은행 계좌를 개설하고 싶습니다. 어떻게 시작해야 하나요?
AI: 안녕하세요! 계좌 개설을 원하신다니 기쁩니다. 먼저, 본인 인증을 위해 신분증을 준비해 주시겠어요?
Human: 사진을 업로드했습니다. 본인 인증은 어떻게 진행되나요?
AI: 업로드해 주신 사진을 확인했습니다. 이제 휴대폰을 통한 본인 인증을 진행해 주세요. 문자로 발송된 인증번호를 입력해 주시면 됩니다.
Human: 인증번호를 입력했습니다. 계좌 개설은 이제 어떻게 하나요?
AI: 본인 인증이 완료되었습니다. 이제 원하시는 계좌 종류를 선택하고 필요한 정보를 입력해 주세요. 예금 종류, 통화 종류 등을 선택할 수 있습니다.
Human: 정보를 모두 입력했습니다. 다음 단계는 무엇인가요?
AI: 입력해 주신 정보를 확인했습니다. 계좌 개설 절차가 거의 끝났습니다. 마지막으로 이용 약관에 동의해 주시고, 계좌 개설을 최종 확인해 주세요.
Human: 모든 절차를 완료했습니다. 계좌가 개설된 건가요?
AI: 네, 계좌 개설이 완료되었습니다. 고객님의 계좌 번호와 관련 정보는 등록하신 이메일로 발송되었습니다. 추가적인 도움이 필요하시면 언제든지 문의해 주세요. 감사합니다!
```

- `return_messages=True` 로 설정하면 `HumanMessage`와 `AIMessage` 객체를 반환한다.

```python
memory = ConversationBufferMemory(return_messages=True)

memory.save_context(
    inputs={
        "human": "안녕하세요, 비대면으로 은행 계좌를 개설하고 싶습니다. 어떻게 시작해야 하나요?"
    },
    outputs={
        "ai": "안녕하세요! 계좌 개설을 원하신다니 기쁩니다. 먼저, 본인 인증을 위해 신분증을 준비해 주시겠어요?"
    },
)

memory.save_context(
    inputs={"human": "네, 신분증을 준비했습니다. 이제 무엇을 해야 하나요?"},
    outputs={
        "ai": "감사합니다. 신분증 앞뒤를 명확하게 촬영하여 업로드해 주세요. 이후 본인 인증 절차를 진행하겠습니다."
    },
)

memory.save_context(
    inputs={"human": "사진을 업로드했습니다. 본인 인증은 어떻게 진행되나요?"},
    outputs={
        "ai": "업로드해 주신 사진을 확인했습니다. 이제 휴대폰을 통한 본인 인증을 진행해 주세요. 문자로 발송된 인증번호를 입력해 주시면 됩니다."
    },
)
```

```python
# history에 저장된 대화 기록을 확인합니다.
memory.load_memory_variables({})["history"]
```

```
# 출력값
[HumanMessage(content='안녕하세요, 비대면으로 은행 계좌를 개설하고 싶습니다. 어떻게 시작해야 하나요?'), AIMessage(content='안녕하세요! 계좌 개설을 원하신다니 기쁩니다. 먼저, 본인 인증을 위해 신분증을 준비해 주시겠어요?'), HumanMessage(content='네, 신분증을 준비했습니다. 이제 무엇을 해야 하나요?'), AIMessage(content='감사합니다. 신분증 앞뒤를 명확하게 촬영하여 업로드해 주세요. 이후 본인 인증 절차를 진행하겠습니다.'), HumanMessage(content='사진을 업로드했습니다. 본인 인증은 어떻게 진행되나요?'), AIMessage(content='업로드해 주신 사진을 확인했습니다. 이제 휴대폰을 통한 본인 인증을 진행해 주세요. 문자로 발송된 인증번호를 입력해 주시면 됩니다.')]
```

#####  Chain에 적용
```python
from langchain_openai import ChatOpenAI
from langchain.chains import ConversationChain

# LLM 모델을 생성합니다.
llm = ChatOpenAI(temperature=0)
# ConversationChain을 생성합니다.
conversation = ConversationChain(
    # ConversationBufferMemory를 사용합니다.
    llm=llm,
    memory=ConversationBufferMemory(),
)
```

- `ConversationChain`을 사용하여 대화를 진행한다.

```python
# 대화를 시작합니다.
response = conversation.predict(
    input="안녕하세요, 비대면으로 은행 계좌를 개설하고 싶습니다. 어떻게 시작해야 하나요?"
)
print(response)
```

```
# 출력값
안녕하세요! 은행 계좌를 개설하려면 먼저 해당 은행의 공식 웹사이트에 접속하셔서 온라인 개설 절차를 따라야 합니다. 보통 개인 정보, 신분증 사본, 주소증명서 등의 문서를 제출해야 하며, 온라인 양식을 작성하고 전자 서명을 해야 합니다. 그 후에 은행에서 제공하는 안내에 따라 추가 단계를 진행하시면 됩니다. 혹시 어떤 은행을 고려하고 계신가요?
```

- 이전의 대화 기록을 기억하고 있는지 확인한다.
```python
# 이전 대화내용을 불렛포인트로 정리해 달라는 요청을 보냅니다.
response = conversation.predict(
    input="이전 답변을 불렛포인트 형식으로 정리하여 알려주세요."
)
print(response)
```

```
# 출력값
1. 해당 은행의 공식 웹사이트에 접속
2. 온라인 개설 절차 따르기
3. 개인 정보, 신분증 사본, 주소증명서 등 제출
4. 온라인 양식 작성 및 전자 서명
5. 은행 안내에 따라 추가 단계 진행
```

### LCEL (대화내용 기억하기): 메모리 추가

- 임의의 체인에 메모리를 추가하는 방법을 보여준다. 현재 메모리 클래스를 사용할 수 있지만 수동으로 연결 해야한다.

```python
from operator import itemgetter
from langchain.memory import ConversationBufferMemory
from langchain_core.prompts import ChatPromptTemplate, MessagesPlaceholder
from langchain_core.runnables import RunnableLambda, RunnablePassthrough
from langchain_openai import ChatOpenAI

# ChatOpenAI 모델을 초기화합니다.
model = ChatOpenAI()
# 대화형 프롬프트를 생성합니다. 이 프롬프트는 시스템 메시지, 이전 대화 내역, 그리고 사용자 입력을 포함합니다.
prompt = ChatPromptTemplate.from_messages(
    [
        ("system", "You are a helpful chatbot"),
        MessagesPlaceholder(variable_name="chat_history"),
        ("human", "{input}"),
    ]
)
```

- 대화내용을 저장할 메모리인 `ConversationBufferMemory` 생성하고 `return_messages` 매개변수를 `True`로 설정하여, 생성된 인스턴스가 메시지를 반환하도록 한다.
- `memory_key` 설정: 추후 Chain의 `prompt` 안에 대입될 key다. 변경하여 사용 가능하다.

```python
# 대화 버퍼 메모리를 생성하고, 메시지 반환 기능을 활성화합니다.
memory = ConversationBufferMemory(
    return_messages=True, memory_key="chat_history")
```

- `RunnablePassthrough.assign`을 사용하여 `chat_history`변수에 `memory.load_memory_variables`함수의 결과를 할당하고, 이 결과에서 `chat_history` 키에 해당하는 값을 추출한다.

```python
runnable = RunnablePassthrough.assign(
    chat_history=RunnableLambda(memory.load_memory_variables)
    | itemgetter("chat_history")  # memory_key 와 동일하게 입력합니다.
)
```

- `runnable` 에 첫 번째 대화를 시작한다.
	- `input` : 사용자 입력 대화가 전달된다.
	- `chat_history`: 대화 기록이 전달된다.

```python
runnable.invoke({"input": "hi!"})
```

```
# 출력값
{'input': 'hi!', 'chat_history': []}
```

```python
chain = runnable | prompt | model
```

첫 번째 대화를 진행한다.

```python
# chain 객체의 invoke 메서드를 사용하여 입력에 대한 응답을 생성합니다.
response = chain.invoke({"input": "만나서 반갑습니다. 제 이름은 테디입니다."})
print(response)  # 생성된 응답을 출력합니다.
```

```
content='만나서 반가워요, 테디님! 무엇을 도와드릴까요?' response_metadata={'finish_reason': 'stop', 'logprobs': None}
```

- `memory.save_context` 함수는 입력 데이터(`inputs`)와 응답 내용(`response.content`)을 메모리에 저장하는 역할
- 이는 AI 모델의 학습 과정에서 현재 상태를 기록하거나, 사용자의 요청과 시스템의 응답을 추적하는데 사용될 수 있다.

```python
# 입력된 데이터와 응답 내용을 메모리에 저장합니다.
memory.save_context(
    {"inputs": "만나서 반갑습니다. 제 이름은 테디입니다."}, {"output": response.content}
)
# 저장된 대화기록을 출력합니다.
memory.load_memory_variables({})
```

```
# 출력값
{'chat_history': [HumanMessage(content='만나서 반갑습니다. 제 이름은 테디입니다.'),  AIMessage(content='만나서 반가워요, 테디님! 무엇을 도와드릴까요?')]}
```

이름을 기억하고 있는지 추가 질의한다.

```python
# 이름을 기억하고 있는지 추가 질의합니다.
response = chain.invoke({"input": "제 이름이 무엇이었는지 기억하세요?"})
# 답변을 출력합니다.
print(response.content)
```

```
# 출력값
네, 테디님이세요. 어떻게 도와드릴까요?
```

---
## LCEL(LangChain Expression Language)

>[!reference] 참고 자료
>[연구원님 LLM 연구노트](https://ppsystem.netlify.app/02-Python/1\)\-Langchain/Langchain-LCEL#-runnablelambda)  
>[LangChain 공식 문서](https://python.langchain.com/v0.1/docs/expression_language/interface/#input-schema)  
>[랭체인LangChain 노트](https://wikidocs.net/233781)  

LCEL(LangChain Expression Language)은 프롬프트 구성, 모델 인스턴스 생성, 출력 생성의 과정을 **==Chain==** 으로 묶어 복잡한 워크플로우를 쉽고 직관적으로 구축할 수 있도록 돕는 인터페이스이다.

특수문자(`|`)를 활용하여 본인만의 Chain을 구축할 수 있다.

### 1. Methods

| Sync/Async         | Description              |
| ------------------ | ------------------------ |
| `invoke`/`ainvoke` | 입력에 대한 결과를 출력한다.         |
| `batch`/`abatch`   | 반복되는 입력을 리스트로 입력하여 처리한다. |
| `stream`/`astream` | chunk마다 출력되게 한다.         |
| `astream_log`      | 중간 단계를 스트리밍한다.           |

```python
# 기본 코드
from langchain_openai import ChatOpenAI
from langchain_core.prompts import PromptTemplate
from langchain_core.output_parsers import StrOutputParser

prompt = PromptTemplate.from_template("{input}에 대해 한국어로 한 줄로 설명해줘")
model = ChatOpenAI(model_name = "gpt-3.5-turbo")
output_parser = StrOutputParser()
chain = prompt | model | output_parser
```

#### `invoke`/ `ainvoke`

```python
import time 
import asyncio
 
def run_sync(input_list):
    """invoke 실행 함수"""
    start_time = time.time()
    for input in input_list:
        result = chain.invoke(input)
        print(result)
    end_time = time.time()
    print("="*100)
    print(f"Sync execution time: {end_time - start_time:.2f} seconds")
 
async def run_async(input_list):
    """ainvoke 실행 함수"""
    start_time = time.time()
    tasks = [chain.ainvoke(input) for input in input_list]
    results = await asyncio.gather(*tasks)
    end_time = time.time()
    print(f"Async execution time: {end_time - start_time:.2f} seconds")
    print("="*100)
 
    for result in results:
        print(result)
 
run_sync(input_list)
await run_async(input_list)
```

```
# 출력값
# Sync execution time: 7.13 seconds
# Async execution time: 1.58 seconds
```

#### `batch` / `abatch`
`batch`와 `abatch`의 속도 차이가 크게 나지 않는 것처럼 보이지만, 보다 더 복잡한 코드에서는 차이가 날 것이다.

```python
import time
import asyncio
 
def run_sync(input_list):
    """batch 실행 함수"""
    start_time = time.time()
    result = chain.batch(input_list)
    end_time = time.time()
    print(f"Sync execution time: {end_time - start_time:.2f} seconds")
    print("="*100)
    print("\n".join(result))
 
async def run_async():
    """abatch 실행 함수"""
    start_time = time.time()
    tasks = chain.abatch(input_list)
    result = await tasks
    end_time = time.time()
    print(f"Async execution time: {end_time - start_time:.2f} seconds")
    print("="*100)
    print("\n".join(result))
 
run_sync(input_list)
await run_async(input_list)
```

```
# 출력값
# Sync execution time: 1.78 seconds
# Async execution time: 1.65 seconds
```

#### `stream` / `astream`

generator로 출력되어 `for`문으로 `print`하면 chunk별로 `streaming`된다.

```python
# generator로 출력되는 것을 확인
chain.stream({"input":"파이썬"}) 
 
# 출력값
# <generator object RunnableSequence.stream at 0x0000014E37FB6650>
```

```python
# stream
for chunk in chain.stream({"input":"파이썬"}):
    print(chunk, end="", flush=True)
 
# astream
for chunk in chain.stream({"input":"파이썬"}):
    print(chunk, end="", flush=True)
```

#### `stream_log`

chain 실행과정을 로깅하는 함수로 디버깅할때 용이하다.

```python
stream = chain.astream_log({"input":"파이썬"})
async for chunk in stream:
    print(chunk)
    print("="*100)
```





