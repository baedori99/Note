---
title: LLM
create_date: 2024-07-17 17:07 - 2024-07-17 17:07
draft: true
---
>[!example]- 
>![](https://imgur.com/mpTwXPI.jpg)

>[!Summary]
>1. 용어 정리  
>	- 인공지능  
>	- 자연어 처리(NLP)  
>	- LLM  
>	- Multi Modal  
>1. LLM의 학습 방법  
>	- Fine Tuning  
>	- In Context Learning  
>2. LLM의 서비스 과정  

이 노트는 원래 데이터 분석과정중 Embedding에 관련하여 노트할려고 했지만 LLM의 정리를 하며 찬찬히 나아가도록 하겠다.

## 1. 용어 정리
### 1) 인공지능

![](https://imgur.com/cMkbrXc.png)

- 학습 데이터의 양이 많아짐에 따라 연산량이 기하급수적으로 커지기 때문에 이를 감당할 수 있는 딥러닝 기반 모델들이 많이 쓰이고 있다.
- 언어 분야는 구글이 만든 Transformer 기반 모델들을 활용한다.

>[!info]
>Transformer란?
>- [Attention Is All You Need](https://arxiv.org/abs/1706.03762) 논문에서 발표된 딥러닝 모델의 일종이다.  
>- 문장 속 단어와 같은 순차 데이터 내의 관계를 추적해 맥락과 의미를 학습하는 신경망이다.  
>- Attention Mechanism은 입력 시퀀스가 길어지면 출력 시퀀스의 정확도가 떨어지는 것을 보정해주기 위한 기법이다.

- Transformer 모델의 특징
	1. **Attention Mechanism**으로 모든 단어간의 상호작용을 동시에 고려하여 문맥을 잘 파악함
	2. **병렬 처리 연산**으로 연산 속도가 빠름
	3. **입출력 길이를 자유롭게 설정**할 수 있어 다양한 과제(번역, 요약, Q&A등)에 활용이 용이함
	4. 사전 훈련된 모델에 새로운 학습 데이터로 재학습하는 **Transfer Learning(전이 학습)** 이 가능함

### 2) 자연어 처리(Natural Langauge Model, NLP)

- 자연어란 한국어, 영어등의 사람들이 일상적으로 쓰는 언어를 의미한다.
- 사람들이 대화하거나 기록하는 데 쓰이는 텍스트들을 컴퓨터가 이해하게 만들기 위해 각 언어의 특징에 따라 수치 데이터로 변환하는 과정을 연구하는 분야이다.
- 한국어의 경우 영어보다 특별한 규칙(조사, 어미, 말투 등)이 많고, '한국어만 읽을 수 있는 리뷰'처럼 이상하게 써도 이해가 가능하다는 특징이 있어 매우 까다로운 언어이다.
>[!example]- K-언어의 위대함
>![](https://imgur.com/5lhVlw5.png)

### 3) LLM(Large Language Model)

![](https://imgur.com/MjLsmXQ.png)

- 억 단위 이상의 데이터를 통해 학습된 거대 언어 모델로, 텍스트를 입력하면 그에 맞는 대답을 하는 텍스트 생성 AI 분야이다.
- 다양하게 활용할 수 있고 직관적으로 편리함을 느낄 수 있다는 장점이 있다.
- 하지만, 대용량의 학습 데이터인 모델의 크기가 매우 커서 많은 자원과 시간이 필요한 분야이다.

### 4) Multi Modal(멀티 모달)

![](https://imgur.com/KVnG33g.png)

- 모달리티(Modality)는 '양식', '양상'이라는 뜻인데, 보통 어떤 형태로 나타나는 현상이나 그것을 받아들이는 방식 또는 정보로 표현되거나 인식되는 특정한 형태 또는 방식을 말한다. 데이터의 형태로 말하면 텍스트, 오디오, 이미지 등이 있다. 
- Multi Modal은 시각, 청각을 비롯한 여러 인터페이스를 통해서 정보를 주고받는 것을 말하는 개념 또는 다양한 형태의 데이터를 입력을 받으면 종합적으로 처리해서 다양한 형태로 출력이 가능한 기술을 의미한다.
- Multi Modal AI는 다양한 채널의 모달리티를 동시에 받아들여서 학습하고 사고하는 AI 또는 인간이 사물을 받아들이는 다양한 방식과 동일하게 학습하는 AI
- Multi Modal 예시
	- 텍스트, 음성, 얼굴 표정을 통해 종합적으로 사람의 감정을 인식
	- 심박수, 행동 및 음성을 통해 현재의 건강상태를 파악
	- 수업 내용을 영상으로 만들거나 음성으로 Q&A를 하고, 퀴즈를 생성하는 학습 플랫폼폼

## 2. LLM의 학습 방법

LLM을 학습시키는 방법에는 2가지 있다.
- Fine Tuning
- In Context Learning(ICL)

|       | Fine Tuning                              | In Context Learning(ICL)                           |
| :---: | ---------------------------------------- | -------------------------------------------------- |
| 학습 방식 | 파운데이션 모델에 추가 데이터를 학습<br>-> 모델 파라미터가 바뀐다. | 프롬프트 내에 정보 또는 예시를 제공하여 학습<br> -> 모델 파라미터가 바뀌지 않는다. |
|  장점   | 특정 도메인에 대한 답변 선능 향상                      | 시간과 비용이 덜 하다.                                      |
|  단점   | 시간과 비용이 많이 필요하다.                         | 잘 설계된 Prompt Engineering이 필요하다.                    |

### 1) Fine Tuning

![](https://imgur.com/bDXsHqx.png)

- Fine Tuning은 새로운 학습 데이터를 통해 사전에 학습된 LLM을 활용하여 재학습하는 것을 말한다.
- 모델이 매우 크기 때문에 많은 GPU가 필요하며, 이는 큰 비용을 초래한다.
- 특정 도메인에 대한 LLM 서비스를 개발할 때 도메인 최적화를 위해 많이 쓰인다.(ex. 금융 등)
- 제공하려는 서비스에 따라 만드는 학습 데이터 구조가 다르며, 데이터의 Bias(편향성), 윤리성, 관리 체계 등이 잘 고려되어야 한다.
>[!reference]
>[PPS 연구원님의 LLM_전체적인 내용 정리](https://ppsystem.netlify.app/01-AI/2\)-Note/LLM-Note#1%EF%B8%8F%E2%83%A3-%EC%9A%A9%EC%96%B4-%EC%A0%95%EB%A6%AC)
>[NVIDIA 트랜스포머 모델이란 무엇인가? (1)](https://blogs.nvidia.co.kr/blog/what-is-a-transformer-model/)
>[WikiDocs_Attention Mechanism](https://wikidocs.net/22893)
>[삼성 SDS Multi Modal](https://www.samsungsds.com/kr/insights/multi-modal-ai.html)

