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
>2. LLM의 학습 방법  
>	- Fine Tuning  
>	- In Context Learning  
>3. LLM의 서비스 과정  

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


>[!reference]
>[PPS 연구원님의 LLM_전체적인 내용 정리](https://ppsystem.netlify.app/01-AI/2\)-Note/LLM-Note#1%EF%B8%8F%E2%83%A3-%EC%9A%A9%EC%96%B4-%EC%A0%95%EB%A6%AC)
>[NVIDIA 트랜스포머 모델이란 무엇인가? (1)](https://blogs.nvidia.co.kr/blog/what-is-a-transformer-model/)
>[WikiDocs_Attention Mechanism](https://wikidocs.net/22893)