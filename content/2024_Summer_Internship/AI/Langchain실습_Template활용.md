---
title: LangChain_실습_MultiChain
create_date: 2024-07-04 16:07 - 2024-07-04 16:07
draft: false
---
# 각각 Template를 만들어 대상별 긍/부정 분류하기

>[!reference]
>[인턴실습_OpenAPI 이용하기](https://ppsystem.netlify.app/00-pps/%F0%9F%93%95-ai--and--data-lab/%F0%9F%93%9D%EC%9D%B8%ED%84%B4%EC%8B%A4%EC%8A%B5%EA%B4%80%EB%A0%A8)
>[LangChain 공식 문서: Runnable Interface](https://python.langchain.com/v0.1/docs/expression_language/interface/#async-invoke)

![__|450](https://imgur.com/EWAniAn.jpg)  ![__|450](https://imgur.com/0x8IISB.jpg) 

## 실습

[[4주차 과제]]때 못한 각각의 카테고리별 Template를 만들어 **한개의 리뷰에 카테고리 3개 Chain을 동시에 출력**하는 실습을 하도록 하겠다.

실습에 대한 방법은 여러 방법으로 실습해볼것이다.
1. Template안에 `{category}`를 넣어 `category`를 지정하면 지정한 카테고리에 맞게 분류되도록 한다.
2. 여러 개의 Template를 각각의 카테고리별로 만들어 `RunnableParallel`를 이용하여 분류하도록 한다.
3. Template안에 `{category}`,`{sentence}`를 넣어 리뷰별 카테고리별로 분류되도록 한다.

- 기본코드
- 
```python
import pandas as pd
from langchain_openai import ChatOpenAI
from langchain_core.prompts import PromptTemplate
from langchain_core.output_parsers import StrOutputParser
from langchain_core.runnables import RunnablePassthrough, RunnableParallel
from dotenv import load_dotenv

load_dotenv()

temp_review = pd.read_csv("C:/Users/pps/Desktop/Restaurant_Review/Review_Analyzer/Data_Analyze/S_hotel_buffet_review_parse.csv")

review = temp_review.head(10)
sentence = temp_review.Review_Text[0] # '직원 분들 너무 친절하시고 음식 맛은 대한민국 호텔 뷔페 넘버 원인데 말해 뭐 해입니다 조금씩 일찍 입장시켜주시는 융통성도 좋아요 쪼금 아쉬운 건 의 외로 과일류 구색이 약하다는 거'
```
---
### 1번 실습

```python
model = ChatOpenAI(model_name = "gpt-3.5-turbo")
output_parser = StrOutputParser()

template = """\
# INSTRUCTION
- 당신은 긍/부정 분류기입니다.
- {category}에 대한 평가가 긍정적인지 부정적인지를 분류하세요.
- {category}에 대한 평가가 없는 경우 '-'을 표시하세요.
- 결과를 다음과 같은 딕셔너리 형식으로 출력하세요.

    "{category}": "긍정/부정/-"

# SENTENCE: "직원 분들 너무 친절하시고 음식 맛은 대한민국 호텔 뷔페 넘버 원인데 말해 뭐 해입니다 조금씩 일찍 입장시켜주시는 융통성도 좋아요 쪼금 아쉬운 건 의 외로 과일류 구색이 약하다는 거"
"""

prompt = PromptTemplate.from_template(template)
runnable = {"category": RunnablePassthrough()}
chain = runnable | prompt | model

# 동기적 방식
chain.batch([
    {"category":"맛"},
    {"category":"서비스"},
    {"category":"가격"}
])

# 비동기적 방식
await chain.abatch([
    {"category":"맛"},
    {"category":"서비스"},
    {"category":"가격"}
])
```

>[!example]- 실행 결과
>```
>[AIMessage(content="{'category': '맛'}: 긍정", response_metadata={'token_usage': {'completion_tokens': 11, 'prompt_tokens': 245, 'total_tokens': 256}, 'model_name': 'gpt-3.5-turbo', 'system_fingerprint': None, 'finish_reason': 'stop', 'logprobs': None}, id='run-b5ad7523-2d9c-4607-9c2d-7d9783ec5e8a-0', usage_metadata={'input_tokens': 245, 'output_tokens': 11, 'total_tokens': 256}),
> AIMessage(content='{"category": "서비스"}: "긍정"', response_metadata={'token_usage': {'completion_tokens': 14, 'prompt_tokens': 248, 'total_tokens': 262}, 'model_name': 'gpt-3.5-turbo', 'system_fingerprint': None, 'finish_reason': 'stop', 'logprobs': None}, id='run-4bbbae6f-f40b-4cc7-a68f-0b25e95fb045-0', usage_metadata={'input_tokens': 248, 'output_tokens': 14, 'total_tokens': 262}),
 >AIMessage(content='{\'category\': \'가격\'}: "부정"', response_metadata={'token_usage': {'completion_tokens': 13, 'prompt_tokens': 248, 'total_tokens': 261}, 'model_name': 'gpt-3.5-turbo', 'system_fingerprint': None, 'finish_reason': 'stop', 'logprobs': None}, id='run-19cbffc5-abb4-4356-a2d4-0abe6a6c0fb0-0', usage_metadata={'input_tokens': 248, 'output_tokens': 13, 'total_tokens': 261})]
>```


### 2번 실습

```python
model = ChatOpenAI(model_name = "gpt-3.5-turbo")
output_parser = StrOutputParser()

template1 = """\
# INSTRUCTION
- 당신은 긍/부정 분류기입니다.
- 맛에 대한 평가가 긍정적인지 부정적인지를 분류하세요.
- 맛에 대한 평가가 없는 경우 '-'을 표시하세요.
- 결과를 다음과 같은 딕셔너리 형식으로 출력하세요.

    "맛": "긍정/부정/-"

# SENTENCE: {sentence}
"""

template2 = """\
# INSTRUCTION
- 당신은 긍/부정 분류기입니다.
- 서비스에 대한 평가가 긍정적인지 부정적인지를 분류하세요.
- 서비스에 대한 평가가 없는 경우 '-'을 표시하세요.
- 결과를 다음과 같은 딕셔너리 형식으로 출력하세요.

    "서비스": "긍정/부정/-"

# SENTENCE: {sentence}
"""

template3 = """\
# INSTRUCTION
- 당신은 긍/부정 분류기입니다.
- 가격에 대한 평가가 긍정적인지 부정적인지를 분류하세요.
- 가격에 대한 평가가 없는 경우 '-'을 표시하세요.
- 결과를 다음과 같은 딕셔너리 형식으로 출력하세요.

    "가격": "긍정/부정/-"

# SENTENCE: {sentence}
"""

prompt1 = PromptTemplate.from_template(template1)
prompt2 = PromptTemplate.from_template(template2)
prompt3 = PromptTemplate.from_template(template3)

chain1 = prompt1 | model | output_parser
chain2 = prompt2 | model | output_parser
chain3 = prompt3 | model | output_parser

combined = RunnableParallel(
    taste = chain1,
    service = chain2,
    price = chain3,
)

for i in range(len(review)):
    result = combined.batch([{"sentence": temp_review.Review_Text[i]}])
    print(result)
```

>[!example]- 실행 결과
>```
>[{'taste': '{\n    "맛": "긍정"\n}', 'service': '{\n    "서비스": "긍정"\n}', 'price': '{\n    "가격": "-"\n}'}]
>[{'taste': '{\n    "맛": "긍정"\n}', 'service': '{\n    "서비스": "긍정"\n}', 'price': '{"가격": "긍정"}'}]
>[{'taste': '{\n    "맛": "-"\n}', 'service': '{\n    "서비스": "긍정"\n}', 'price': '{\n    "가격": "긍정"\n}'}]
>[{'taste': '{\n    "맛": "부정"\n}', 'service': '{\n    "서비스": "긍정"\n}', 'price': '{\n    "가격": "부정"\n}'}]
>[{'taste': '{\n    "맛": "긍정"\n}', 'service': '{\n    "서비스": "긍정"\n}', 'price': '{\n    "가격": "-"\n}'}]
>[{'taste': '{\n    "맛": "긍정"\n}', 'service': '{\n    "서비스": "긍정"\n}', 'price': '{\n    "가격": "긍정"\n}'}]
>[{'taste': '{\n    "맛": "긍정"\n}', 'service': '{\n    "서비스": "긍정"\n}', 'price': '{\n    "가격": "-"\n}'}]
>[{'taste': '{\n    "맛": "긍정"\n}', 'service': '{\n    "서비스": "긍정"\n}', 'price': '{\n    "가격": "긍정"\n}'}]
>[{'taste': '{\n    "맛": "긍정"\n}', 'service': '{\n    "서비스": "긍정"\n}', 'price': '{\n    "가격": "긍정"\n}'}]
>[{'taste': '{\n    "맛": "긍정"\n}', 'service': '{\n    "서비스": "긍정"\n}', 'price': '{"가격": "긍정"}'}]
>```


### 3번 실습

```python
from langchain_openai import ChatOpenAI
from langchain_core.prompts import PromptTemplate
from langchain_core.output_parsers import JsonOutputParser
from langchain_core.runnables import RunnablePassthrough, RunnableParallel
from operator import itemgetter

template = """\
# INSTRUCTION
- 당신은 {sentence}에서 {category}에 따라 긍/부정을 분류하는 역할입니다.
- {category}에 대한 반응을 [긍정, 부정, -] 중 하나로 분류하세요
- JSON 형식으로 출력하세요
"""

prompt = PromptTemplate.from_template(template)
model = ChatOpenAI(model_name = "gpt-3.5-turbo")
output_parser = JsonOutputParser()
chain = prompt | model | output_parser

def make_runnable(text):
    return RunnablePassthrough.assign(category = lambda _: text)

combined = RunnableParallel(
    taste = make_runnable("맛") | chain,
    service = make_runnable("서비스") | chain,
    price = make_runnable("가격") | chain,
)

review = "직원분들 너무 친절하시고 음식맛은 대한민국 호텔부페 넘버원인데 말해뭐해 입니다 조금씩 일찍 입장 시켜주시는 융통성도 좋아요 쬐끔 아쉬운건 의외로 과일류 구색이 약하다는 거"

combined.invoke({"sentence": review})
```

>[!example]- 실행 결과
>```
>{'taste': {'맛반응': '부정'},
 >'service': {'서비스반응': '긍정'},
 >'price': {'음식맛': '긍정', '서비스': '긍정', '가격': '부정', '분위기': '긍정'}}
>```

- `make_runnable(text)`를 통해 `category`에 `review`를 추가할 수 있다.
- `RunnablePassthrough.assign()`는 딕셔너리를 추가할 때 용이한데 그래서 딕셔너리 형태로만 받는다.

- 예제
```python
add_runnable = RunnablePassthrough.assign(taste = lambda x: "안뇽")
add_runnable.invoke({"input": "다람쥐"})
```

```python
# 출력값
# {'input': '다람쥐', 'taste': '안뇽'}
```

`{"input":"다람쥐"}`에 딕셔너리 형태로 `{"taste":"안뇽"}`이 들어간 것이다.

```python
runnable = {"input": RunnablePassthrough()}
add_runnable = RunnablePassthrough.assign(taste = lambda x: "안뇽")
(runnable | add_runnable).invoke("다람쥐")
```

```python
# 출력값
# {'input': '다람쥐', 'taste': '안뇽'}
```

`(runnable | add_runnable).invoke("다람쥐")`에서 `"다람쥐"`가 맨앞의 `runnable`에 들어가며 `{"input":"다람쥐"}` 딕셔너리가 만들어지고 `add_runnable`을 통해 `{"taste":"안뇽"}`이 추가 되었다.