---
title: Data Analyze (데이터 탐색 및 분석)
create_date: 2024-07-23 10:07 - 2024-07-23 10:07
draft: false
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

- 카테고리 4개인 `만족도`,`맛`,`서비스`,`가격`으로 나누어 분류를 시도했다.
- 각 카테고리별로 나누어 긍/부정 분류를 한다.
- 처음 시도한 것으로 클래스로 만들었다.
>[!Note]- 버전1 코드
>```python
>class MyChain:
>    """chain을 만들어 프롬프트와 연결하는 클래스"""
>    
>    def __init__(self, template):
>        self.llm = ChatOpenAI()
>        self.prompt = PromptTemplate.from_template(template)
>        
>    def invoke(self, review_text):
>        input_data = {"sentence": review_text}
>        result = (self.prompt | self.llm).invoke(input_data)
>        return result
>        
>def parsing(output):
>    """분류된 json형식을 딕셔너리로 바꾸는 함수"""
>    try:
>        result_dict = json.loads(output)
>    except json.JSONDecodeError:
>        result_dict = {}
>    return result_dict
>    
>def save_parse_reviews(df, chain):
>    """분류된 데이터들을 데이터프레임에 저장하는 함수"""
>    temp = {"만족도": [], "맛":[], "서비스":[], "가격":[]}
>    
>    for sentence in df["Review_Text"]:
>        emo_eval = chain.invoke(sentence)
>        test_result = parsing(emo_eval.content)
>        temp["만족도"].append(test_result["만족도"])
>        temp["맛"].append(test_result["맛"])
>        temp["서비스"].append(test_result["서비스"])
>        temp["가격"].append(test_result["가격"])
>        
>    df["만족도"] = temp["만족도"]
>    df["맛"] = temp["맛"]
>    df["서비스"] = temp["서비스"]
>    df["가격"] = temp["가격"]
>    df.to_csv("./S_hotel_buffet_review_parse.csv")
>    return df
>    
>load_dotenv()
>
>template = """\
># INSTRUCTION
>- 당신은 긍/부정 분류기입니다.
>- 각 대상 '만족도', '맛', '서비스', '가격'에 대한 평가가 긍정적인지 부정적인지를 분류하세요.
>- 대상에 대한 평가가 없는 경우 '-'을 표시하세요.
>- 예시를 보고 결과를 다음과 같은 딕셔너리 형식으로 출력하세요:
>    "만족도": "긍정/부정/-",
>    "맛": "긍정/부정/-",
>    "서비스": "긍정/부정/-",
>    "가격": "긍정/부정/-"
># SENTENCE: {sentence}
>"""
>```

```python
# 실행
s_hotel_buffet_review_outlier = pd.read_csv("C:/Users/pps/Desktop/Restaurant_Review/Data_Preprocessing/S_hotel_buffet_review_IQR.csv")
s_hotel_buffet_review_outlier = s_hotel_buffet_review_outlier.drop('Unnamed: 0', axis = 1)

chain = MyChain(template=template)

s_hotel_buffet_parse_review = save_parse_reviews(s_hotel_buffet_review_outlier, chain)
```

>[!example]- 실행 결과
>![](https://imgur.com/XjlpSDi.png)


### 버전 2

- `pydantic` 라이브러리를 사용해 템플릿을 명확한 제시 방향으로 만든다.
- `맛` 카테고리는 음식에 따라 긍/부정이 나누어질 수 있어 `맛` 카테고리 자체로 긍/부정 판단이 어렵다고 생각했다.
- `맛` 카테고리에 `맛 긍정`과 `맛 부정` 항목에 음식 이름을 넣어 분류하겠다.
>[!Note]- 버전2 코드
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


### 버전 3

- `맛` 카테고리에서 긍/부정에 대한 대상이 없다면 `음식 없음`을 출력하게 했다.
- `맛` 카테고리에서 맛에 대한 긍/부정 리뷰글이 없다면 `"-"`을 출력하게 했다.
- 위의 버전 2와 마찬가지로 정확하고 완벽하게 분류되어 출력되지 않았다.

>[!Note]- 버전3 코드
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


### 버전 4

- 카테고리별로 나누어 긍/부정을 확인하는 방법도 있지만 리뷰글 자체의 긍/부정도 확인하려고 한다.
- 카테고리로 안나누고 긍/부정으로 분류하겠다.

>[!Note]- 버전4 코드
>```python
>template = """\
># INSTRUCTION
>- 당신은 리뷰글 긍/부정 분류기입니다.
>- 긍정과 부정이 SENTENCE에 같이 써져있으면 "-"로 분류하세요.
>- SENTENCE를 [긍정, 부정, -]중 하나로 분류하세요.
>- 다음과 같이 딕셔너리 형식으로 출력하세요.
>    "긍/부정": "긍정/부정/-"
># SENTENCE: {sentence}
>"""
>
>model = ChatOpenAI(model_name = "gpt-3.5-turbo")
>prompt = PromptTemplate.from_template(template)
>chain = prompt | model
>columns = {"긍/부정" : []}
>
>for i in range(len(review)):
>    sentence = review.Review_Text[i]
>    result = chain.invoke({"sentence":sentence})
>    final = json.loads(result.content)
>    columns["긍/부정"].append(final["긍/부정"])
>    
>review["긍/부정"] = columns["긍/부정"]
>```

```python
ten_review
```

>[!example]- 실행 결과
>![](https://imgur.com/069jV0l.png)

## 3) 긍/부정으로 분류된 데이터 시각화 및 분석

- 여기에 쓰인 버전은 `버전1`이다.

### Setting
```python
import pandas as pd

s_hotel_buffet_review_parsed = pd.read_csv("C:\\Users\\pps\\Desktop\\Restaurant_Review\\Review_Analyzer\\Data_Analyze\\S_hotel_buffet_review_parse.csv", index_col=0, encoding="utf-8")

review = s_hotel_buffet_review_parsed.copy()
```

```python
# 한글 글꼴 설정
import matplotlib.pyplot as plt
import numpy as np

font_path = 'C:/Users/pps/AppData/Local/Microsoft/Windows/Fonts/NanumBarunGothic.ttf'
font_name = plt.matplotlib.font_manager.FontProperties(fname=font_path).get_name()

plt.rcParams['font.family'] = font_name
```

### 3-1. 각 카테고리별 긍/부정 개수 확인

- `value_counts()`로 column별 value 개수 확인
```python
def emotion_eval_counts(df, column_name):
    """긍정/부정/- 개수 추출하는 함수"""
    return df[column_name].value_counts()
```

```python
print(emotion_eval_counts(review, "만족도"))
print(emotion_eval_counts(review, "맛"))
print(emotion_eval_counts(review, "서비스"))
print(emotion_eval_counts(review, "가격"))
```

>[!example]- 실행 결과
>![](https://imgur.com/RKxwbKb.png)

### 3-2. 각 카테고리별 긍/부정 개수 막대 그래프 시각화

- 막대 그래프를 통해 `만족도`와 `맛` 카테고리에 대한 긍/부정 평가가 `서비스`, `가격`에 비해 월등히 많았다는 것을 알 수 있다.

- 최종 함수
```python
def s_hotel_review_emote_bar(emote1,emote2,emote3,emote4):
    """카테고리 4개인 막대그래프 만드는 함수"""
    x = np.arange(4)

    satisf_Aemo = emote1["긍정"] + emote1["부정"]
    taste_Aemo = emote2["긍정"] + emote2["부정"]
    service_Aemo = emote3["긍정"] + emote3["부정"]
    price_Aemo = emote4["긍정"] + emote4["부정"]
    
    y_axis = [satisf_Aemo,taste_Aemo,service_Aemo,price_Aemo]
    x_axis = ["만족도","맛","서비스","가격"]

    plt.bar(x,y_axis)
    plt.xticks(x, x_axis)
    plt.title("카테고리별 긍/부정 갯수")
    plt.show()
```

```python
satisfy_emote = emotion_eval_counts(review, "만족도")
taste_emote = emotion_eval_counts(review, "맛")
service_emote = emotion_eval_counts(review, "서비스")
price_emote = emotion_eval_counts(review, "가격")

s_hotel_review_emote_bar(satisfy_emote,taste_emote,service_emote,price_emote)
```

>[!example]- 실행 결과
>![](https://imgur.com/M8ktRZq.png)

### 3-3. 각 카테고리별 긍/부정 비율 파이그래프(Pie Chart) 시각화

- 파이그래프를 통해 `만족도`,`맛`,`서비스` 카테고리에서 긍정의 비율이 **<U>90%</U>** 이상 많았다.
- 하지만, `가격` 카테고리에선 부정의 비율이 **<U>80%</U>** 이상 많았다.

- 긍/부정/- 평가 개수 구하기
```python
# 평가 개수 구하기
satisfy_emo = emotion_eval_counts(review, "만족도")
taste_emo = emotion_eval_counts(review, "맛")
service_emo = emotion_eval_counts(review, "서비스")
price_emo = emotion_eval_counts(review, "가격")
```

- 긍/부정 총 평가 개수 구하기
```python
# 긍정 + 부정 평가 개수 구하기
satisfy_total = satisfy_emo["긍정"] + satisfy_emo["부정"]
taste_total = taste_emo["긍정"] + taste_emo["부정"]
service_total = service_emo["긍정"] + service_emo["부정"]
price_total = price_emo["긍정"] + price_emo["부정"]
```

- 백분율 구하는 함수
```python
def percent_emotion(reviews, total_number):
    """백분율 구하는 함수"""
    return reviews / total_number * 100
```

- 각 카테고리별 긍/부정 비율 구하기
```python
# 각 카테고리별 긍/부정 비율 구하기
satisfy_positive = percent_emotion(satisfy_emo["긍정"], satisfy_total)
satisfy_negative = percent_emotion(satisfy_emo["부정"], satisfy_total)

taste_positive = percent_emotion(taste_emo["긍정"], taste_total)
taste_negative = percent_emotion(taste_emo["부정"], taste_total)

service_positive = percent_emotion(service_emo["긍정"], taste_total)
service_negative = percent_emotion(service_emo["부정"], taste_total)

price_positive = percent_emotion(price_emo["긍정"], price_total)
price_negative = percent_emotion(price_emo["부정"], price_total)
```

- 각 카테고리별 긍/부정 비율 파이그래프 시각화
```python
import matplotlib.pyplot as plt
  
ratio1 = [satisfy_positive, satisfy_negative]
ratio2 = [taste_positive, taste_negative]
ratio3 = [service_positive, service_negative]
ratio4 = [price_positive, price_negative]

labels = ["긍정", "부정"]
colors = ["#00539C", "#EEA47F"]

fig, ax = plt.subplots(2,2, figsize=(6, 6))

ax[0,0].set_title("만족도 긍/부정 비율")
ax[0,0].pie(ratio1, labels= labels, autopct='%.1f%%', colors = colors)

ax[0,1].set_title("맛 긍/부정 비율")
ax[0,1].pie(ratio2, labels= labels, autopct='%.1f%%', colors = colors)

ax[1,0].set_title("서비스 긍/부정 비율")
ax[1,0].pie(ratio3, labels= labels, autopct='%.1f%%', colors = colors)

ax[1,1].set_title("가격 긍/부정 비율")
ax[1,1].pie(ratio4, labels= labels, autopct='%.1f%%', colors = colors)

plt.tight_layout()
plt.show()
```

>[!example]- 실행 결과
>![](https://imgur.com/7WI6Yg6.png)

### 3-4. 카테고리별 긍/부정 리뷰글 명사, 동사/형용사 추출 및 시각화

- 모든 카테고리의 리뷰글을 추출하지 않고 `만족도`와 `가격` 카테고리만 한다.
	- `만족도` 카테고리는 리뷰글 수가 제일 많았고 긍정 리뷰글 수도 제일 많았다.
	- `가격` 카테고리는 리뷰글 수가 적지만 부정 리뷰글 수가 많았다.
- `만족도` 카테고리의 긍정 리뷰글과 `가격` 카테고리의 부정 리뷰글만 추출한 이유는 긍정 리뷰글중 제일 돋보이는 것과 부정 리뷰글중 제일 돋보이는 카테고리들이라 생각했기 때문이다.
- **하지만**, `만족도` 카테고리의 부정 리뷰글과 `가격` 카테고리의 긍정 리뷰글도 그 비율은 매우 적지만 적은 만큼 이유가 확실할거라 생각하여 추출하였다.

- 카테고리에 긍정으로 표시된 리뷰글만 추출하는 함수
```python
def positive_review_extractor(df, string_column, category_column):
    """카테고리에 긍정으로 표시된 리뷰글만 추출하는 함수
    Args:
        df (_type_): 데이터프레임
        string_column (_type_): 리뷰글항목
        category_column (_type_): 카테고리명
    """
    positive_reviews = {"긍정":[]}
    for i in range(len(df)):
        if df[category_column][i] == "긍정":
            positive_reviews["긍정"].append(df[string_column][i])
    return positive_reviews
```

- 카테고리에 부정으로 표시된 리뷰글만 추출하는 함수
```python
def negative_review_extractor(df, string_column, category_column):
    """카테고리에 부정으로 표시된 리뷰글만 추출하는 함수
    Args:
        df (_type_): 데이터프레임
        string_column (_type_): 리뷰글항목
        category_column (_type_): 카테고리명
    """
    negative_reviews = {"부정":[]}
    for i in range(len(df)):
        if df[category_column][i] == "부정":
            negative_reviews["부정"].append(df[string_column][i])
    return negative_reviews
```

- 리뷰글 `string`으로 추출하는 함수
```python
def positive_review_string_extractor(df):
    """딕셔너리로 받은 긍정 리뷰글 string으로 추출하는 함수"""
    for key, value in df.items():
        df[key] = ', '.join(value)
    positive_string = df.get("긍정")
    return positive_string

def negative_review_string_extractor(df):
    """딕셔너리로 받은 부정 리뷰글 string으로 추출하는 함수"""
    for key, value in df.items():
        df[key] = ', '.join(value)
    negative_string = df.get("부정")
    return negative_string
```

- `만족도` 카테고리 리뷰글 추출
```python
satisfy_review = positive_review_extractor(review, "Review_Text","만족도")
satisfy_positive_string = positive_review_string_extractor(satisfy_review)
kiwi_satisfy_positive_noun = kiwi_noun_extractor(satisfy_positive_string)
kiwi_satisfy_positive_verb_adj = kiwi_verb_adj_extractor(satisfy_positive_string)

satisfy_negative_review = negative_review_extractor(review, "Review_Text", "만족도")
satisfy_negative_string = negative_review_string_extractor(satisfy_negative_review)
kiwi_satisfy_negative_noun = kiwi_noun_extractor(satisfy_negative_string)
kiwi_satisfy_negative_verb_adj = kiwi_verb_adj_extractor(satisfy_negative_string)
```

- `가격` 카테고리 리뷰글 추출
```python
price_reviews = negative_review_extractor(review, "Review_Text","가격")
price_negative_string = negative_review_string_extractor(price_reviews)
kiwi_price_negative_noun = kiwi_noun_extractor(price_negative_string)
kiwi_price_negative_verb_adj = kiwi_verb_adj_extractor(price_negative_string)

price_positive_review = positive_review_extractor(review, "Review_Text", "가격")
price_positive_string = positive_review_string_extractor(price_positive_review)
kiwi_price_positive_noun = kiwi_noun_extractor(price_positive_string)
kiwi_price_positive_verb_adj = kiwi_verb_adj_extractor(price_positive_string)
```

- 명사, 동사/형용사 워드클라우드로 나타내는 함수
```python
from wordcloud import WordCloud
import matplotlib.pyplot as plt
from collections import Counter

def wordcloud_noun(noun_text):
    """명사만 워드클라우드로 나타내는 함수"""
    cnt = len(noun_text)
    counts = Counter(noun_text)
    tags_noun = counts.most_common(cnt)
    wc = WordCloud(font_path='C:/Users/pps/AppData/Local/Microsoft/Windows/Fonts/NanumBarunGothic.ttf', background_color='white', width=800, height=600)
    cloud_noun = wc.generate_from_frequencies(dict(tags_noun))
    return cloud_noun

def wordcloud_verb_adj(verb_adj_text):
    """동사/형용사 워드 클라우드로 나타내는 함수"""
    cnt = len(verb_adj_text)
    counts = Counter(verb_adj_text)
    tags_verb_adj = counts.most_common(cnt)
    wc = WordCloud(font_path='C:/Users/pps/AppData/Local/Microsoft/Windows/Fonts/NanumBarunGothic.ttf', background_color='white', width=800, height=600)
    cloud_verb_adj = wc.generate_from_frequencies(dict(tags_verb_adj))
    return cloud_verb_adj
```

- 만족도 카테고리 명사, 동사/형용사 시각화
```python
wc_satisfy_noun = wordcloud_noun(kiwi_satisfy_positive_noun)
wc_satisfy_verb_adj = wordcloud_verb_adj(kiwi_satisfy_positive_verb_adj)
wc_negative_satisfy_noun = wordcloud_noun(kiwi_satisfy_negative_noun)
wc_negative_satisfy_verb_adj = wordcloud_verb_adj(kiwi_satisfy_negative_verb_adj)

fig, ax = plt.subplots(2,2, figsize=(8, 6))
title_font = {
        'fontsize':16,
        'fontweight': 'bold'
    }
plt.axis('off')

ax[0,0].imshow(wc_satisfy_noun)
ax[0,0].set_title("만족도 긍정 리뷰글 명사모음", title_font)
ax[0,0].axis('off')

ax[0,1].imshow(wc_satisfy_verb_adj)
ax[0,1].set_title("만족도 긍정 리뷰글 동사/형용사모음", title_font)
ax[0,1].axis('off')

ax[1,0].imshow(wc_negative_satisfy_noun)
ax[1,0].set_title("만족도 부정 리뷰글 명사모음", title_font)
ax[1,0].axis('off')

ax[1,1].imshow(wc_negative_satisfy_verb_adj)
ax[1,1].set_title("만족도 부정 리뷰글 동사/형용사모음", title_font)
ax[1,1].axis('off')

plt.tight_layout()
plt.show()
```

>[!example]- 실행 결과
>![](https://imgur.com/9apfgUt.png)

-  가격 카테고리 명사, 동사/형용사 시각화
```python
wc_price_noun = wordcloud_noun(kiwi_price_negative_noun)
wc_price_verb_adj = wordcloud_verb_adj(kiwi_price_negative_verb_adj)
wc_price_positive_noun = wordcloud_noun(kiwi_price_positive_noun)
wc_price_positive_verb_adj = wordcloud_verb_adj(kiwi_price_positive_verb_adj)

fig, ax = plt.subplots(2,2, figsize=(8, 6))
title_font = {
        'fontsize':16,
        'fontweight': 'bold'
    }
plt.axis('off')
ax[0,0].imshow(wc_price_positive_noun)
ax[0,0].set_title("가격 긍정 리뷰글 명사모음", title_font)
ax[0,0].axis('off')

ax[0,1].imshow(wc_price_positive_verb_adj)
ax[0,1].set_title("가격 긍정 리뷰글 동사/형용사모음", title_font)
ax[0,1].axis('off')

ax[1,0].imshow(wc_price_noun)
ax[1,0].set_title("가격 부정 리뷰글 명사모음", title_font)
ax[1,0].axis('off')

ax[1,1].imshow(wc_price_verb_adj)
ax[1,1].set_title("가격 부정 리뷰글 동사/형용사모음", title_font)
ax[1,1].axis('off')

plt.tight_layout()
plt.show()
```

>[!example]- 실행 결과
>![](https://imgur.com/7W4jciI.png)

### 3-5. 빈도 높은 명사 3개 긍정/부정 분류

- `만족도` 카테고리의 긍정/부정 리뷰글 빈도 높은 명사 3개를 위의 워드 클라우드를 통해 뽑는다.
- 최대한 다양한 명사들로 하고 싶어 의미가 비슷한 명사는 제외하겠다.
- `가격` 카테고리의 긍정/부정 리뷰글 빈도 높은 명사 3개를 위의 워드 클라우드를 통해 뽑는다.
- `뷔페`, `호텔`등의 식당이름 혹은 식당을 나타내는 단어는 빼도록 하겠다.

- 만족도/가격

|  만족도   |        | 가격     |        |
| :----: | ------ | ------ | ------ |
| **긍정** | **부정** | **긍정** | **부정** |
|   음식   | 음식     | 가격     | 디저트    |
|   친절   | 사람     | 음식     | 가격     |
|  디저트   | 가격     | 생일     | 친절     |

- 만족도 빈도 높은 명사 긍/부정 분류하는 클래스
>[!Note]- 만족도 빈도 높은 명사 긍/부정 분류하는 코드
>```python
># Setting
>import os
>import pandas as pd
>import json
>from dotenv import load_dotenv
>from langchain_openai import ChatOpenAI
>from langchain_core.prompts import PromptTemplate
>
>class MyChain:
>    """chain을 만들어 프롬프트와 연결하는 클래스"""
>    
>    def __init__(self, template):
>        self.llm = ChatOpenAI()
>        self.prompt = PromptTemplate.from_template(template)
>        
>    def invoke(self, review_text):
>        input_data = {"sentence": review_text}
>        result = (self.prompt | self.llm).invoke(input_data)
>        return result
>        
>def parsing(output):
>    """분류된 json형식을 딕셔너리로 바꾸는 함수"""
>    
>    try:
>        result_dict = json.loads(output)
>    except json.JSONDecodeError:
>        result_dict = {}
>    return result_dict
>    
>def save_satisfy_positive_reviews(df, chain):
>    """분류된 데이터들을 데이터프레임에 저장하는 함수"""
>    
>    temp = {"디저트": [], "음식":[], "친절":[]}
>    for sentence in df["긍정"]:
>        emo_eval = chain.invoke(sentence)
>        test_result = parsing(emo_eval.content)
>        temp["디저트"].append(test_result["디저트"])
        temp["음식"].append(test_result["음식"])
>        temp["친절"].append(test_result["친절"])
>    df["긍정_디저트"] = temp["디저트"]
>    df["긍정_음식"] = temp["음식"]
>    df["긍정_친절"] = temp["친절"]
>    df.to_csv("./july_eighteenth_satisfy_positive_noun_classify.csv", index = False)
>    return df
>    
>def save_satisfy_negative_reviews(df, chain):
>    """분류된 데이터들을 데이터프레임에 저장하는 함수"""
>    
>    temp = {"음식": [], "사람":[], "가격":[]}
>    for sentence in df["부정"]:
>        emo_eval = chain.invoke(sentence)
>        test_result = parsing(emo_eval.content)
>        temp["음식"].append(test_result["음식"])
>        temp["사람"].append(test_result["사람"])
>        temp["가격"].append(test_result["가격"])
>    df["부정_음식"] = temp["음식"]
>    df["부정_사람"] = temp["사람"]
>    df["부정_가격"] = temp["가격"]
>    df.to_csv("./july_eighteenth_satisfy_negative_noun_classify.csv", index = False)
>    return df
>    
>load_dotenv()
>
>positive_template = """\
># INSTRUCTION
>- 당신은 긍/부정 분류기입니다.
>- 각 대상 '디저트', '음식', '친절'에 대한 평가가 긍정적인지 부정적인지를 분류하세요.
>- 대상에 대한 평가가 없는 경우 '-'을 표시하세요.
>- 예시를 보고 결과를 다음과 같은 딕셔너리 형식으로 출력하세요:
>    "디저트": "긍정/부정/-",
>    "음식": "긍정/부정/-",
>    "친절": "긍정/부정/-",
># SENTENCE:{sentence}
>"""
>
>negative_template = """\
># INSTRUCTION
>- 당신은 긍/부정 분류기입니다.
>- 각 대상 '음식', '사람', '가격'에 대한 평가가 긍정적인지 부정적인지를 분류하세요.
>- 대상에 대한 평가가 없는 경우 '-'을 표시하세요.
>- 예시를 보고 결과를 다음과 같은 딕셔너리 형식으로 출력하세요:
>    "음식": "긍정/부정/-",
>    "사람": "긍정/부정/-",
>    "가격": "긍정/부정/-",
># SENTENCE:{sentence}
>"""
>```

```python
import pandas as pd

satisfy_review_positive = positive_review_extractor(review, "Review_Text","만족도")
satisfy_review_negative = negative_review_extractor(review, "Review_Text","만족도")
df_satisfy_positive = pd.DataFrame(satisfy_review_positive)
df_satisfy_negative = pd.DataFrame(satisfy_review_negative)
```

```python
positive_satisfy_chain = MyChain(positive_template)
satisfy_positive_parse_review = save_satisfy_positive_reviews(df_satisfy_positive, positive_satisfy_chain)

negative_satisfy_chain = MyChain(negative_template)
satisfy_negative_parse_review = save_satisfy_negative_reviews(df_satisfy_negative,negative_satisfy_chain)
```

```python
satisfy_positive_parse_review.head()

satisfy_negative_parse_review.head()
```

>[!example]- 실행 결과
>![](https://imgur.com/JxqlRk5.png)
>  
>![](https://imgur.com/rqHCBVI.png)

- 가격 빈도 높은 명사 긍/부정 분류하는 클래스
>[!Note]- 가격 빈도 높은 명사 긍/부정 분류하는 코드
>```python
># Setting
>import os
>import pandas as pd
>import json
>from dotenv import load_dotenv
>from langchain_openai import ChatOpenAI
>from langchain_core.prompts import PromptTemplate
>
>class MyChain:
>    """chain을 만들어 프롬프트와 연결하는 클래스"""
>    
>    def __init__(self, template):
>        self.llm = ChatOpenAI()
>        self.prompt = PromptTemplate.from_template(template)
>        
>    def invoke(self, review_text):
>        input_data = {"sentence": review_text}
>        result = (self.prompt | self.llm).invoke(input_data)
>        return result
>        
>def parsing(output):
>    """분류된 json형식을 딕셔너리로 바꾸는 함수"""
>    try:
>        result_dict = json.loads(output)
>    except json.JSONDecodeError:
>        result_dict = {}
>    return result_dict
>    
>def save_price_positive_reviews(df, chain):
>    """분류된 데이터들을 데이터프레임에 저장하는 함수"""
>    temp = {"가격": [], "음식":[], "생일":[]}
>    for sentence in df["긍정"]:
>        emo_eval = chain.invoke(sentence)
>        test_result = parsing(emo_eval.content)
>        temp["가격"].append(test_result.get("가격"))
>        temp["음식"].append(test_result.get("음식"))
>        temp["생일"].append(test_result.get("생일"))
>        
>    df["긍정_가격"] = temp["가격"]
>    df["긍정_음식"] = temp["음식"]
>    df["긍정_생일"] = temp["생일"]
>    df.to_csv("./july_eighteenth_price_positive_noun_classify.csv", index = False)
>    return df
>    
>def save_price_negative_reviews(df, chain):
>    """분류된 데이터들을 데이터프레임에 저장하는 함수"""
>    temp = {"디저트": [], "가격":[], "친절":[]}
>    
>    for sentence in df["부정"]:
>        emo_eval = chain.invoke(sentence)
>        test_result = parsing(emo_eval.content)
>        temp["디저트"].append(test_result.get("디저트"))
>        temp["가격"].append(test_result.get("가격"))
>        temp["친절"].append(test_result.get("친절"))
>        
>    df["부정_디저트"] = temp["디저트"]
>    df["부정_가격"] = temp["가격"]
>    df["부정_친절"] = temp["친절"]
>    df.to_csv("./july_eighteenth_price_negative_noun_classify.csv", index = False)
>    return df
>    
>load_dotenv()
>
>template_positive = """\
># INSTRUCTION
>- 당신은 긍/부정 분류기입니다.
>- 각 대상 '가격', '음식', '생일'에 대한 평가가 긍정적인지 부정적인지를 분류하세요.
>- 대상에 대한 평가가 없는 경우 '-'을 표시하세요.
>- 예시를 보고 결과를 다음과 같은 딕셔너리 형식으로 출력하세요:
>    "가격": "긍정/부정/-",
>    "음식": "긍정/부정/-",
>    "생일": "긍정/부정/-",
># SENTENCE:{sentence}
>"""
>
>template_negative = """\
># INSTRUCTION
>- 당신은 긍/부정 분류기입니다.
>- 각 대상 '디저트', '가격', '친절'에 대한 평가가 긍정적인지 부정적인지를 분류하세요.
>- 대상에 대한 평가가 없는 경우 '-'을 표시하세요.
>- 예시를 보고 결과를 다음과 같은 딕셔너리 형식으로 출력하세요:
>    "디저트": "긍정/부정/-",
>    "가격": "긍정/부정/-",
>    "친절": "긍정/부정/-",
># SENTENCE:{sentence}
>"""
>```

```python
import pandas as pd

price_review_positive = positive_review_extractor(review, "Review_Text","가격")
price_review_negative = negative_review_extractor(review, "Review_Text","가격")
df_price_positive = pd.DataFrame(price_review_positive)
df_price_negative = pd.DataFrame(price_review_negative)
```

```python
positive_price_chain = MyChain(template_positive)
price_positive_parse_review = save_price_positive_reviews(df_price_positive, positive_price_chain)

negative_price_chain = MyChain(template_negative)
price_negative_parse_review = save_price_negative_reviews(df_price_negative,negative_price_chain)
```

```python
price_positive_parse_review.head()

price_negative_parse_review.head()
```

>[!example]- 실행 결과
>![](https://imgur.com/VP6i1p2.png)
>
>![](https://imgur.com/u9YrNJB.png)

### 3-6. 빈도 높은 명사들의 총 긍/부정 개수

- 빈도 높은 명사들의 총 긍/부정 개수를 구하여 리뷰글에서 언급되는 양을 확인한다.

- `만족도` 카테고리의 빈도 높은 명사들의 총 긍/부정 개수를 구하는 코드
```python
print(emotion_eval_counts(satisfy_positive_parse_review, "긍정_디저트"))
print(emotion_eval_counts(satisfy_positive_parse_review, "긍정_음식"))
print(emotion_eval_counts(satisfy_positive_parse_review, "긍정_친절"))

print(emotion_eval_counts(satisfy_negative_parse_review, "부정_음식"))
print(emotion_eval_counts(satisfy_negative_parse_review, "부정_사람"))
print(emotion_eval_counts(satisfy_negative_parse_review, "부정_가격"))
```

>[!example]- 실행 결과
>![](https://imgur.com/e7tg5Y9.png)

- `가격` 카테고리의 빈도 높은 명상들의 총 긍/부정 개수를 구하는 코드
```python
print(emotion_eval_counts(price_positive_parse_review, "긍정_가격"))
print(emotion_eval_counts(price_positive_parse_review, "긍정_음식"))
print(emotion_eval_counts(price_positive_parse_review, "긍정_생일"))

print(emotion_eval_counts(price_negative_parse_review, "부정_디저트"))
print(emotion_eval_counts(price_negative_parse_review, "부정_가격"))
print(emotion_eval_counts(price_negative_parse_review, "부정_친절"))
```

>[!example]- 실행결과
>![](https://imgur.com/4suQ0jS.png)


- `만족도` 카테고리의 빈도 높은 명사들의 긍/부정/- 평가 개수 구하기
```python
# 평가 개수 구하기
# 긍정
satisfy_pos_dessert_emo = emotion_eval_counts(satisfy_positive_parse_review, "긍정_디저트")
satisfy_pos_food_emo = emotion_eval_counts(satisfy_positive_parse_review, "긍정_음식")
satisfy_pos_kindness_emo = emotion_eval_counts(satisfy_positive_parse_review, "긍정_친절")

# 부정
satisfy_neg_food_emo = emotion_eval_counts(satisfy_negative_parse_review, "부정_음식")
satisfy_neg_customer_emo = emotion_eval_counts(satisfy_negative_parse_review, "부정_사람")
satisfy_neg_price_emo = emotion_eval_counts(satisfy_negative_parse_review, "부정_가격")
```

- 긍정/부정 총 평가 개수 구하기
```python
# 긍정 + 부정 평가 개수 구하기
# 긍정
satisfy_pos_dessert_total = satisfy_pos_dessert_emo["긍정"] + satisfy_pos_dessert_emo["부정"]
satisfy_pos_food_total = satisfy_pos_food_emo["긍정"] + satisfy_pos_food_emo["부정"]
satisfy_pos_kindness_total = satisfy_pos_kindness_emo["긍정"] + satisfy_pos_kindness_emo["부정"]

# 부정
satisfy_neg_food_total = satisfy_neg_food_emo["긍정"] + satisfy_neg_food_emo["부정"]
satisfy_neg_customer_total = satisfy_neg_customer_emo["긍정"] + satisfy_neg_customer_emo["부정"]
satisfy_neg_price_emo_total = satisfy_neg_price_emo["부정"]
```

- 막대그래프로 총 긍/부정 개수 확인하기
```python
x_positive = np.arange(3)
satisfy_pos_dessert_emo = satisfy_pos_dessert_total
satisfy_pos_food_emo = satisfy_pos_food_total
satisfy_pos_kindness_emo = satisfy_pos_kindness_total

x_negative = np.arange(3)
satisfy_neg_food_emo = satisfy_neg_food_total
satisfy_neg_customer_emo = satisfy_neg_customer_total
satisfy_neg_price_emo = satisfy_neg_price_emo_total

fig, ax = plt.subplots(1,2, figsize=(12, 6))

y_s_pos_axis = [satisfy_pos_dessert_emo,satisfy_pos_food_emo,satisfy_pos_kindness_emo]
x_s_pos_axis = ["디저트","음식","친절"]
ax[0].bar(x_positive,y_s_pos_axis)
ax[0].set_xticks(x_positive)
ax[0].set_xticklabels(x_s_pos_axis)
ax[0].set_title("만족도 긍정 리뷰글")

y_s_neg_axis = [satisfy_neg_food_emo,satisfy_neg_customer_emo,satisfy_neg_price_emo]
x_s_neg_axis = ["음식","사람","가격"]
ax[1].bar(x_negative,y_s_neg_axis)
ax[1].set_xticks(x_negative)
ax[1].set_xticklabels(x_s_neg_axis)
ax[1].set_title("만족도 부정 리뷰글")

plt.suptitle("빈도 높은 카테고리별 명사들 총 긍/부정 개수")
plt.show()
```

>[!example]- 실행 결과
>![](https://imgur.com/nPQxAVQ.png)

- `가격` 카테고리의 빈도 높은 명사들의 긍/부정/- 평가 개수 구하기
```python
# 평가 개수 구하기
# 긍정
price_pos_price_emo = emotion_eval_counts(price_positive_parse_review, "긍정_가격")
price_pos_food_emo = emotion_eval_counts(price_positive_parse_review, "긍정_음식")
price_pos_birthday_emo = emotion_eval_counts(price_positive_parse_review, "긍정_생일")

# 부정
price_neg_dessert_emo = emotion_eval_counts(price_negative_parse_review, "부정_디저트")
price_neg_price_emo = emotion_eval_counts(price_negative_parse_review, "부정_가격")
price_neg_kindness_emo = emotion_eval_counts(price_negative_parse_review, "부정_친절")
```

- 긍/부정 총 평가 개수 구하기
```python
# 긍정 + 부정 평가 개수 구하기
# 긍정
price_pos_price_total = price_pos_price_emo["긍정"] + price_pos_price_emo["부정"]
price_pos_food_total = price_pos_food_emo["긍정"] + price_pos_food_emo["부정"]
price_pos_birthday_total = price_pos_birthday_emo["긍정"] + price_pos_birthday_emo["부정"]

# 부정
price_neg_dessert_total = price_neg_dessert_emo["긍정"] + price_neg_dessert_emo["부정"]
price_neg_price_total = price_neg_price_emo["긍정"] + price_neg_price_emo["부정"]
price_neg_kindness_total = price_neg_kindness_emo["긍정"] + price_neg_kindness_emo["부정"]
```

- 막대그래프로 총 긍/부정 개수 확인하기
```python
x_positive = np.arange(3)
price_pos_price_emo = price_pos_price_total
price_pos_food_emo = price_pos_food_total
price_pos_birthday_emo = price_pos_birthday_total

x_negative = np.arange(3)
price_neg_dessert_emo = price_neg_dessert_total
price_neg_price_emo = price_neg_price_total
price_neg_kindness_emo = price_neg_kindness_total

fig, ax = plt.subplots(1,2, figsize=(12, 6))

y_p_pos_axis = [price_pos_price_emo,price_pos_food_emo,price_pos_birthday_emo]
x_p_pos_axis = ["가격","음식","생일"]
ax[0].bar(x_positive,y_p_pos_axis)
ax[0].set_xticks(x_positive)
ax[0].set_xticklabels(x_p_pos_axis)
ax[0].set_title("가격 긍정 리뷰글")

y_p_neg_axis = [price_neg_dessert_emo,price_neg_price_emo,price_neg_kindness_emo]
x_p_neg_axis = ["디저트","가격","친절"]
ax[1].bar(x_negative,y_p_neg_axis)
ax[1].set_xticks(x_negative)
ax[1].set_xticklabels(x_p_neg_axis)
ax[1].set_title("가격 부정 리뷰글")

plt.suptitle("빈도 높은 카테고리별 명사들 총 긍/부정 개수")
plt.show()
```

>[!example]- 실행 결과
>![](https://imgur.com/7zvt20h.png)

### 3-7. 빈도 높은 명사들의 긍/부정 비율 시각화

- 비율 시각화를 통해 긍/부정 리뷰글에서 명사들이 긍정적인지 부정적인 확인한다.
- `"-"`은 비중이 큰 것도 있지만 긍/부정 비율만 비교했을 때 긍/부정 차이가 애매해질 수 있어서 뺐다.
- `만족도` 부정 리뷰글의 가격 긍/부정 비율은 긍정이 없기에 `"-"`로 대체하였다.
	- 부정적인 평가만 존재한다.

#### `만족도` 긍/부정 비율 시각화

- 명사들의 긍/부정/- 평가 개수 구하기
```python
# 평가 개수 구하기
# 긍정
satisfy_pos_dessert_emo = emotion_eval_counts(satisfy_positive_parse_review, "긍정_디저트")
satisfy_pos_food_emo = emotion_eval_counts(satisfy_positive_parse_review, "긍정_음식")
satisfy_pos_kindness_emo = emotion_eval_counts(satisfy_positive_parse_review, "긍정_친절")

# 부정
satisfy_neg_food_emo = emotion_eval_counts(satisfy_negative_parse_review, "부정_음식")
satisfy_neg_customer_emo = emotion_eval_counts(satisfy_negative_parse_review, "부정_사람")
satisfy_neg_price_emo = emotion_eval_counts(satisfy_negative_parse_review, "부정_가격")
```

- 긍/부정 총 평가 개수 구하기
```python
# 긍정 + 부정 평가 개수 구하기
# 긍정
satisfy_pos_dessert_total = satisfy_pos_dessert_emo["긍정"] + satisfy_pos_dessert_emo["부정"] #+ satisfy_pos_dessert_emo["-"]
satisfy_pos_food_total = satisfy_pos_food_emo["긍정"] + satisfy_pos_food_emo["부정"] #+ satisfy_pos_food_emo["-"]
satisfy_pos_kindness_total = satisfy_pos_kindness_emo["긍정"] + satisfy_pos_kindness_emo["부정"] #+ satisfy_pos_kindness_emo["-"]

# 부정
satisfy_neg_food_total = satisfy_neg_food_emo["긍정"] + satisfy_neg_food_emo["부정"] #+ satisfy_neg_food_emo["-"]
satisfy_neg_customer_total = satisfy_neg_customer_emo["긍정"] + satisfy_neg_customer_emo["부정"] #+ satisfy_neg_customer_emo["-"]
satisfy_neg_price_total = satisfy_neg_price_emo["부정"] + satisfy_neg_price_emo["-"]
```

- 긍/부정 비율 구하기
```python
# 긍/부정 비율 구하기
sp_dessert_positive = percent_emotion(satisfy_pos_dessert_emo["긍정"], satisfy_pos_dessert_total)
sp_dessert_negative = percent_emotion(satisfy_pos_dessert_emo["부정"], satisfy_pos_dessert_total)

sp_food_positive = percent_emotion(satisfy_pos_food_emo["긍정"], satisfy_pos_food_total)
sp_food_negative = percent_emotion(satisfy_pos_food_emo["부정"], satisfy_pos_food_total)

sp_kindness_positive = percent_emotion(satisfy_pos_kindness_emo["긍정"], satisfy_pos_kindness_total)
sp_kindness_negative = percent_emotion(satisfy_pos_kindness_emo["부정"], satisfy_pos_kindness_total)

sn_food_positive = percent_emotion(satisfy_neg_food_emo["긍정"], satisfy_neg_food_total)
sn_food_negative = percent_emotion(satisfy_neg_food_emo["부정"], satisfy_neg_food_total)

sn_customer_positive = percent_emotion(satisfy_neg_customer_emo["긍정"], satisfy_neg_customer_total)
sn_customer_negative = percent_emotion(satisfy_neg_customer_emo["부정"], satisfy_neg_customer_total)

sn_price_none = percent_emotion(satisfy_neg_price_emo["-"], satisfy_neg_price_emo_total)
sn_price_negative = percent_emotion(satisfy_neg_price_emo["부정"], satisfy_neg_price_emo_total)
```

- `만족도` 긍정 리뷰글 빈도 높은 명사들 긍/부정 비율 파이그래프 시각화
```python
import matplotlib.pyplot as plt

ratio1 = [sp_dessert_positive, sp_dessert_negative]
ratio2 = [sp_food_positive, sp_food_negative]
ratio3 = [sp_kindness_positive, sp_kindness_negative]

labels = ["긍정", "부정"]
colors = ["#00539C", "#EEA47F"]
fig, ax = plt.subplots(1,3, figsize=(12, 5))
ax[0].set_title("디저트 긍/부정 비율")
ax[0].pie(ratio1, labels= labels, autopct='%.1f%%', colors = colors)

ax[1].set_title("음식 긍/부정 비율")
ax[1].pie(ratio2, labels= labels, autopct='%.1f%%', colors = colors)

ax[2].set_title("친절 긍/부정 비율")
ax[2].pie(ratio3, labels= labels, autopct='%.1f%%', colors = colors)

plt.suptitle("만족도 긍정 리뷰글 빈도 높은 명사들 긍/부정 비율")
plt.tight_layout()
plt.show()
```

>[!example]- 실행 결과
>![](https://imgur.com/eHJexBL.png)

- `만족도` 부정 리뷰글 빈도 높은 명사들 긍/부정 비율 파이그래프 시각화
```python
import matplotlib.pyplot as plt

ratio1 = [sn_food_positive, sn_food_negative]
ratio2 = [sn_customer_positive, sn_customer_negative]
ratio3 = [sn_price_none, sn_price_negative]

labels = ["긍정", "부정"]
colors = ["#00539C", "#EEA47F"]
fig, ax = plt.subplots(1,3, figsize=(12, 5))

ax[0].set_title("음식 긍/부정 비율")
ax[0].pie(ratio1, labels= labels, autopct='%.1f%%', colors = colors)

ax[1].set_title("사람 긍/부정 비율")
ax[1].pie(ratio2, labels= labels, autopct='%.1f%%', colors = colors)

labels = ["-", "부정"]
colors = ["#00539C", "#EEA47F"]

ax[2].set_title("가격 긍/부정 비율")
ax[2].pie(ratio3, labels= labels, autopct='%.1f%%', colors = colors)

plt.suptitle("만족도 부정 리뷰글 빈도 높은 명사들 긍/부정 비율")
plt.tight_layout()
plt.show()
```

>[!example]- 실행 결과
>![](https://imgur.com/vk6LxHx.png)

#### 가격 긍/부정 비율 시각화

- 명사들의 긍/부정/- 개수 구하기
```python
# 평가 개수 구하기
# 긍정
price_pos_price_emo = emotion_eval_counts(price_positive_parse_review, "긍정_가격")
price_pos_food_emo = emotion_eval_counts(price_positive_parse_review, "긍정_음식")
price_pos_birthday_emo = emotion_eval_counts(price_positive_parse_review, "긍정_생일")
  
# 부정
price_neg_dessert_emo = emotion_eval_counts(price_negative_parse_review, "부정_디저트")
price_neg_price_emo = emotion_eval_counts(price_negative_parse_review, "부정_가격")
price_neg_kindness_emo = emotion_eval_counts(price_negative_parse_review, "부정_친절")
```

- 긍/부정 총 평가 개수 구하기
```python
# 긍정 + 부정 평가 개수 구하기
# 긍정
price_pos_dessert_total = price_pos_price_emo["긍정"] + price_pos_price_emo["부정"]
price_pos_food_total = price_pos_food_emo["긍정"] + price_pos_food_emo["부정"]
price_pos_birthday_total = price_pos_birthday_emo["긍정"] + price_pos_birthday_emo["부정"]
  
# 부정
price_neg_dessert_total = price_neg_dessert_emo["긍정"] + price_neg_dessert_emo["부정"]
price_neg_price_total = price_neg_price_emo["긍정"] + price_neg_price_emo["부정"]
price_neg_kindness_total = price_neg_kindness_emo["긍정"] + price_neg_kindness_emo["부정"]
```

- 긍/부정 비율 구하기
```python
# 긍/부정 비율 구하기
ps_price_positive = percent_emotion(price_pos_price_emo["긍정"], price_pos_dessert_total)
ps_price_negative = percent_emotion(price_pos_price_emo["부정"], price_pos_dessert_total)

ps_food_positive = percent_emotion(price_pos_food_emo["긍정"], price_pos_food_total)
ps_food_negative = percent_emotion(price_pos_food_emo["부정"], price_pos_food_total)

ps_birthday_positive = percent_emotion(price_pos_birthday_emo["긍정"], price_pos_birthday_total)
ps_birthday_negative = percent_emotion(price_pos_birthday_emo["부정"], price_pos_birthday_total)

pn_dessert_positive = percent_emotion(price_neg_dessert_emo["긍정"], price_neg_dessert_total)
pn_dessert_negative = percent_emotion(price_neg_dessert_emo["부정"], price_neg_dessert_total)

pn_price_positive = percent_emotion(price_neg_price_emo["긍정"], price_neg_price_total)
pn_price_negative = percent_emotion(price_neg_price_emo["부정"], price_neg_price_total)

pn_kindness_positive = percent_emotion(price_neg_kindness_emo["긍정"], price_neg_kindness_total)
pn_kindness_negative = percent_emotion(price_neg_kindness_emo["부정"], price_neg_kindness_total)
```

- `가격` 긍정 리뷰글 빈도 높은 명사들 긍/부정 비율 파이그래프 시각화
```python
import matplotlib.pyplot as plt

ratio1 = [ps_price_positive, ps_price_negative]
ratio2 = [ps_food_positive, ps_food_negative]
ratio3 = [ps_birthday_positive, ps_birthday_negative]

labels = ["긍정", "부정"]
colors = ["#00539C", "#EEA47F"]
fig, ax = plt.subplots(1,3, figsize=(12, 5))

ax[0].set_title("가격 긍/부정 비율")
ax[0].pie(ratio1, labels= labels, autopct='%.1f%%', colors = colors)

ax[1].set_title("음식 긍/부정 비율")
ax[1].pie(ratio2, labels= labels, autopct='%.1f%%', colors = colors)

ax[2].set_title("생일 긍/부정 비율")
ax[2].pie(ratio3, labels= labels, autopct='%.1f%%', colors = colors)

plt.suptitle("가격 긍정 리뷰글 빈도 높은 명사들 긍/부정 비율")
plt.tight_layout()
plt.show()
```

>[!example]- 실행 결과
>![](https://imgur.com/rHyXI7p.png)

- `가격` 부정 리뷰글 빈도 높은 명사들 긍/부정 비율 파이그래프 시각화
```python
import matplotlib.pyplot as plt

ratio1 = [pn_dessert_positive, pn_dessert_negative]
ratio2 = [pn_price_positive, pn_price_negative]
ratio3 = [pn_kindness_positive, pn_kindness_negative]

labels = ["긍정", "부정"]
colors = ["#00539C", "#EEA47F"]
fig, ax = plt.subplots(1,3, figsize=(12, 5))

ax[0].set_title("디저트 긍/부정 비율")
ax[0].pie(ratio1, labels= labels, autopct='%.1f%%', colors = colors)

ax[1].set_title("가격 긍/부정 비율")
ax[1].pie(ratio2, labels= labels, autopct='%.1f%%', colors = colors)

ax[2].set_title("친절 긍/부정 비율")
ax[2].pie(ratio3, labels= labels, autopct='%.1f%%', colors = colors)

plt.suptitle("가격 부정 리뷰글 빈도 높은 명사들 긍/부정 비율")
plt.tight_layout()
plt.show()
```

>[!example]- 실행 결과
>![](https://imgur.com/x5uBlNR.png)

### 3-8. 키워드를 리뷰글에서 확인하기

- `가격` 카테고리에서 긍/부정 리뷰글 전부 디저트 키워드가 많은 언급이 되었다.
- 디저트에 부정적인 면이 어떤 것이 있는지 확인한다.
	- 사람들이 가격이 비싸지만 디저트는 맛있다와 같은 긍정적인 평가를 남겼다.
- 디저트 키워드가 언급된 리뷰글들을 보면 긍정적인 평가가 많다는 것을 알 수 있다.
- 다른 키워드들도 확인한다.

- 키워드: 디저트
```python
dessert_review=review[review["Review_Text"].str.contains("디저트")]
dessert_review.tail(10)
```

>[!example]- 실행 결과
>![](https://imgur.com/E1MdBMl.png)

- 키워드: 친절
```python
kindness_review=review[review["Review_Text"].str.contains("친절")]
kindness_review.head(10)
```

>[!example]- 실행 결과
>![](https://imgur.com/OuiC9dg.png)

- 키워드: 음식
```python
food_review=review[review["Review_Text"].str.contains("음식")]
food_review.head(10)
```

>[!example]- 실행 결과
>![](https://imgur.com/01GGe8M.png)

