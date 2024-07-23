---
title: Data Analyze (데이터 탐색 및 분석)
create_date: 2024-07-23 10:07 - 2024-07-23 10:07
draft: true
---
## 1) 형태소 분석

- `okt` 형태소 분석기 사용 후기
	- 명사로 분리하는 기준이 명확하지 않아 명사가 아닌 것도 나오게 된다.
	- 동사/형용사는 같은 뜻을 가졌지만 다른 형태를 가진 중복 단어 처리가 안된다.
- `kiwipiepy`형태소 분석기 사용 후기
	- 명사로 분리하는 기준이 명확하다.
	- 동사/형용사는 중복처리를 하여 뜻이 하나인 형태만 나와 분류하기 편하다.

### 1-1 명사 분류 및 시각화

- 명사 형태소 분석기
```python
from konlpy.tag import Okt
from kiwipiepy import Kiwi

okt = Okt()
kiwi = Kiwi()


# okt 명사 형태소 분석기
def okt_noun_extractor(text):
    return okt.nouns(text)


# kiwipiepy 명사 형태소 분석기
def kiwi_noun_extractor(text):
    results = []
    result = kiwi.analyze(text)
    for token, pos, _, _ in result[0][0]:
        # kiwi의 태그 목록 체언:
        # NNG: 일반 명사, NNP: 고유 명사, NNB: 의존 명사, NR: 수사, NP: 대명사
        if len(token) != 1 and pos.startswith('N'): #or pos.startswith('SL'):
            results.append(token)
    return results
```

```python
review_okt_nouns = okt_noun_extractor(review_string)
review_kiwi_nouns = kiwi_noun_extractor(review_string)
```

- 워드 클라우드
```python
# 워드 클라우드를 통해 시각화
from wordcloud import WordCloud
import matplotlib.pyplot as plt
from collections import Counter

def wordcloud_noun(noun_text):
    cnt = len(noun_text)
    counts = Counter(noun_text)
    tags_noun = counts.most_common(cnt)
    wc = WordCloud(font_path='C:/Users/pps/AppData/Local/Microsoft/Windows/Fonts/NanumBarunGothic.ttf', background_color='white', width=800, height=600)
    cloud_noun = wc.generate_from_frequencies(dict(tags_noun))

    return cloud_noun
```

```python
wc_okt = wordcloud_noun(review_okt_nouns)
wc_kiwi = wordcloud_noun(review_kiwi_nouns)


fig, ax = plt.subplots(1,2, figsize=(10, 5))
plt.axis('off')
ax[0].imshow(wc_okt)
ax[0].set_title("Okt 명사 워드 클라우드")
ax[0].axis('off')
ax[1].imshow(wc_kiwi)
ax[1].set_title("kiwipiepy 명사 워드 클라우드")
ax[1].axis('off')
plt.tight_layout()

plt.show()
```

>[!example]- 실행 결과
>![](https://imgur.com/d7ZVTGh.png)

### 1-2 동사/형용사 분류 및 시각화

- 동사/형용사 형태소 분석기
```python
from konlpy.tag import Okt
from kiwipiepy import Kiwi

okt = Okt()
kiwi = Kiwi()


# 동사/형용사 okt 형태소 분석기
def okt_adj_verb_extractor(text):
    results = []
    result = okt.pos(text)
    for token, pos in result:
        if len(token) != 1 and pos.startswith('Adj') or pos.startswith('Verb'):
            results.append(token)

    return results

def kiwi_verb_adj_extractor(text):
    results = []
    result = kiwi.analyze(text)
    for token,pos,_,_ in result[0][0]:
        if len(token) != 1 and pos.startswith('VA') or pos.startswith('VV'):
            results.append(token)

    f_results = list(map(lambda x : x + '다',results))

    return f_results
```

>[!example]- 실행 결과
>![](https://imgur.com/AazsZKG.png)

## 2) 카테고리별 긍/부정 분류

- 카테고리는 4개로 나누어 분류한다.
- 4개의 카테고리로 나누는 이유
	- 리뷰글에서 사람들에게 제일 많이 언급될 카테고리가 `만족도`, `맛`, `서비스`, `가격`이라고 생각했다.
	- 네이버 리뷰를 보면 키워드별 리뷰글을 볼 수 있게 남겨져 있는데 그 중 리뷰글이 많고 포괄적이게 쓰인 키워드들을 선택했다.
- `맛` 카테고리는 음식별 긍/부정 평가를 분석하고 싶어 템플릿을 다르게 적용해보았다.
	- 템플릿이 명확하게 되어있지 않아 출력물에 음식 이름뿐만 아니라 동사/형용사도 같이 나오게 되었다.
	- 템플릿을 고쳐가며 좀 더 발전시켜 할 부분이다.

### Setting
```python
import pandas as pd
import json
from typing import Dict, List
from langchain_openai import ChatOpenAI
from langchain_core.prompts import PromptTemplate
from langchain_core.pydantic_v1 import BaseModel, Field
from langchain_core.output_parsers import JsonOutputParser, StrOutputParser
from langchain_core.runnables import RunnableParallel
from dotenv import load_dotenv
import re

load_dotenv()

review = pd.read_csv("C:\\Users\\pps\\Desktop\\Restaurant_Review\\Review_Analyzer\\Data_Preprocessing\\S_hotel_buffet_review_IQR.csv", index_col= 0, encoding="utf-8")

ten_review = review.copy()
ten_review = ten_review.head(10)
ten_review2 = ten_review.head(10)
```

### 버전 1
>[!Note]- 버전1 코드
>```python
>class ActionModel(BaseModel):
>    positive: List[str] = Field(description="긍정을 나타내는 키워드")
>    negative: List[str] = Field(description="부정을 나타내는 키워드")
>    
>class ReviewModel(BaseModel):
>    category: str = Field(description="CATEGORY 문자열")
>    action: ActionModel
>    
>parser = JsonOutputParser(pydantic_object = ReviewModel)
>output_parser = StrOutputParser()
>model = ChatOpenAI(model_name = "gpt-3.5-turbo")
>
>template1 = """\
># INSTRUCTION
>- 당신은 긍/부정 분류기입니다.
>- 만족도에 대한 평가가 긍정적인지 부정적인지를 분류하세요.
>- 만족도에 대한 평가가 없는 경우 '-'을 표시하세요.
>- 결과를 다음과 같은 딕셔너리 형식으로 출력하세요.
>  "만족도": "긍정/부정/-"
># SENTENCE: {sentence}
>"""
>
>template2 = """\
># INSTRUCTION
>- 당신은 SENTENCE에서 CATEGORY에 따라 긍/부정에 해당하는 키워드를 분류하는 역할입니다.
>- 키워드는 긍/부정의 대상이며, 명사만 추출하세요.
>- FORMAT에 맞춰 답변하세요.
># FORMAT: {format_instructions}
># CATEGORY: {category}
># SENTENCE: {sentence}
>"""
>
>template3 = """\
># INSTRUCTION
>- 당신은 긍/부정 분류기입니다.
>- 서비스에 대한 평가가 긍정적인지 부정적인지를 분류하세요.
>- 서비스에 대한 평가가 없는 경우 '-'을 표시하세요.
>- 결과를 다음과 같은 딕셔너리 형식으로 출력하세요.
>    "서비스": "긍정/부정/-"
># SENTENCE: {sentence}
>"""
>
>template4 = """\
># INSTRUCTION
>- 당신은 긍/부정 분류기입니다.
>- 가격에 대한 평가가 긍정적인지 부정적인지를 분류하세요.
>- 가격에 대한 평가가 없는 경우 '-'을 표시하세요.
>- 결과를 다음과 같은 딕셔너리 형식으로 출력하세요.
>    "가격": "긍정/부정/-"
># SENTENCE: {sentence}
>"""
>
>prompt1 = PromptTemplate.from_template(template1)
>prompt2 = PromptTemplate.from_template(template2).partial(format_instructions = parser.get_format_instructions(), category= "맛")
>prompt3 = PromptTemplate.from_template(template3)
>prompt4 = PromptTemplate.from_template(template4)
>
>chain1 = prompt1 | model | output_parser
>chain2 = prompt2 | model | parser
>chain3 = prompt3 | model | output_parser
>chain4 = prompt4 | model | output_parser
>
>combined = RunnableParallel(
>    satisfy = chain1,
>    taste = chain2,
>    service = chain3,
>    price = chain4,
>)
>
>columns = {"Satisfaction":[], "Taste Positive":[], "Taste Negative": [], "Service":[], "Price":[]}
>
>for i in range(len(ten_review)):
>    sentence = ten_review.Review_Text[i]
>    result = combined.invoke({"sentence":sentence})
>    parsed_result = {}
>    for key, value in result.items():
>        try:
>            if key == "taste":
>                parsed_result[key] = value
>            else:
>                parsed_result[key] = json.loads(value)
>        except json.JSONDecodeError as e:
>            continue
>            
>    combined_result = {
>        **parsed_result.get("satisfy", {}),
>        **parsed_result.get("taste", {}).get("action",{}),
>         **parsed_result.get("service", {}),
>        **parsed_result.get("price", {}),
>    }
>    
>    columns["Satisfaction"].append(combined_result.get("만족도"))
>    columns["Taste Positive"].append(combined_result.get("positive","-"))
>    columns["Taste Negative"].append(combined_result.get("negative","-"))
>    columns["Service"].append(combined_result.get("서비스"))
>    columns["Price"].append(combined_result.get("가격"))
>```

```python
ten_review["Satisfaction"] = columns["Satisfaction"]
ten_review["Taste Positive"] = columns["Taste Positive"]
ten_review["Taste Negative"] = columns["Taste Negative"]
ten_review["Service"] = columns["Service"]
ten_review["Price"] = columns["Price"]

ten_review
```

>[!example]- 실행 결과
>![](https://imgur.com/JfgWJUi.png)

### 버전 2

- `맛` 카테고리에서 긍/부정에 대한 대상이 없다면 `음식 없음`을 출력하게 했다.
- `맛` 카테고리에서 맛에 대한 긍/부정 리뷰글이 없다면 `"-"`을 출력하게 했다.
- 위의 버전 2와 마찬가지로 정확하고 완벽하게 분류되어 출력되지 않았다.

>[!Note]- 버전 2 코드
>```python
>model = ChatOpenAI(model_name = "gpt-3.5-turbo")
> output_parser = StrOutputParser()
>  template1 = """\
>  # INSTRUCTION 
>  - 당신은 긍/부정 분류기입니다. 
>  - 만족도에 대한 평가가 긍정적인지 부정적인지를 분류하세요. 
>  - 만족도에 대한 평가가 없는 경우 '-'을 표시하세요. 
>  - 결과를 다음과 같은 딕셔너리 형식으로 출력하세요.  
> 		 "만족도": "긍정/부정/-" 
>  # SENTENCE: {sentence} 
>  """ 
>  
>  template2 = """\ 
>  # INSTRUCTION 
>  1. You are tasked with classifying the sentiment as positive or negative based on reactions to taste in the SENTENCE. 
>  2. If there is a reaction to taste, categorize the name of the food that caused the reaction as either "Taste Positive" or "Taste Negative". 
>  3. If there is no reaction to taste, output "-" for both "Taste Positive" and "Taste Negative". 
>  4. If there is a reaction to taste but the name of the food is not mentioned, output "음식 없음" for both categories. 
>  5. Differentiate between reactions to the taste of food and evaluations of the restaurant. 
>  6. Be careful not to mistake the name of a restaurant for the name of a food. 
>  7. Output the result in dictionary format. 
>		"Taste Positive" : ["Food Name/음식 없음/-"] 
>		"Taste Negative" : ["Food Name/음식 없음/-"] 
>  8. # SENTENCE: {sentence} 
>  """ 
>  
>  template3 = """\ 
>  # INSTRUCTION 
>  - 당신은 긍/부정 분류기입니다. 
>  - 서비스에 대한 평가가 긍정적인지 부정적인지를 분류하세요. 
>  - 서비스에 대한 평가가 없는 경우 '-'을 표시하세요. 
>  - 결과를 다음과 같은 딕셔너리 형식으로 출력하세요.  
> 		 "서비스": "긍정/부정/-" 
>  # SENTENCE: {sentence} 
>  """ 
>  
>  template4 = """\ 
>  # INSTRUCTION 
>  - 당신은 긍/부정 분류기입니다. 
>  - 가격에 대한 평가가 긍정적인지 부정적인지를 분류하세요. 
>  - 가격에 대한 평가가 없는 경우 '-'을 표시하세요. 
>  - 결과를 다음과 같은 딕셔너리 형식으로 출력하세요. 
> 		 "가격": "긍정/부정/-" 
>  # SENTENCE: {sentence} 
>  """
>   
>  prompt1 = PromptTemplate.from_template(template1) 
>  prompt2 = PromptTemplate.from_template(template2) 
>  prompt3 = PromptTemplate.from_template(template3) 
>  prompt4 = PromptTemplate.from_template(template4) 
>  
>  chain1 = prompt1 | model | output_parser 
>  chain2 = prompt2 | model | output_parser 
>  chain3 = prompt3 | model | output_parser 
>  chain4 = prompt4 | model | output_parser 
>  
>  combined = RunnableParallel( 
>  satisfy = chain1, 
>  taste = chain2, 
>  service = chain3, 
>  price = chain4,
>   ) 
>   columns = {"Satisfaction":[], "Taste Positive":[], "Taste Negative": [], "Service":[], "Price":[]} 
>   
>   for i in range(len(ten_review)): 
> 	  result = combined.batch([{"sentence":ten_review.Review_Text[i]}]) 
> 	  parsed_result = {} 
> 	  for key, value in result[0].items(): 
> 		try: 
> 			parsed_result[key] = json.loads(value) 
> 		except json.JSONDecodeError as e: 
> 			continue 
> 	combined_result = {
> 		 **parsed_result.get("satisfy", {}),
> 		 **parsed_result.get("taste", {}), 
> 		 **parsed_result.get("service", {}), 
> 		 **parsed_result.get("price", {}), 
> 		 } 
> 	 
> 	 columns["Satisfaction"].append(combined_result.get("만족도", "-"))  
> 	 columns["Taste Positive"].append(combined_result.get("Taste Positive", "-"))   
> 	 columns["Taste Negative"].append(combined_result.get("Taste Negative", "-"))  
> 	 columns["Service"].append(combined_result.get("서비스","-")) 
> 	 columns["Price"].append(combined_result.get("가격", "-"))
>```

```python
ten_review2["Satisfaction"] = columns["Satisfaction"]
ten_review2["Taste Positive"] = columns["Taste Positive"]
ten_review2["Taste Negative"] = columns["Taste Negative"]
ten_review2["Service"] = columns["Service"]
ten_review2["Price"] = columns["Price"]

ten_review2
```

>[!example]- 실행 결과
>![](https://imgur.com/u194XDn.png)

