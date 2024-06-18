---
title: Korean Data Preprocessing (Kiwipiepy)
create_date: 2024-06-18 10:06 - 2024-06-18 10:06
draft: false
---
# Kiwipiepy

- 한국어 형태소 분석기인 Kiwi(Korean Intelligent Word Identifier)의 Python 모듈
```
pip install kiwipiepy
```

##  Kiwipiepy의 장점

### 1. 텍스트 분석 속도
![](https://imgur.com/IOWToky.jpg)

- 텍스트 분석 속도가 Mecab(IOS) 다음으로 제일 빠르다는 것을 알 수 있다.

---

### 2. 모호성 해소 정확도
![](https://imgur.com/mx33UGW.jpg)

- 속도면에서 압도적이었던 Mecab이 모호성 해소 부분에서 성능이 떨어지지만 kiwipiepy는 90%에 준하는 성능을 보여준다.

---

### 3. 문장 분리 정확도
![](https://imgur.com/PtfGe8y.jpg)

- 문장 분리 기능을 비롯한 다양한 편의 기능을 제공한다.

---
## 워드 클라우드 명사, 동사, 형용사 만들기

>[!Todo] 과제 목록
>1. 유튜브 댓글 크롤링한 csv 파일의 댓글 텍스트를 텍스트로 쓴다.
>2. 명사, 동사, 형용사별로 분리하는 kiwi 함수를 만든다.
>3. 워드 클라우드를 이용해 시각화한다.

---
### 1. 댓글 텍스트 불러오기

```python
import pandas as pd

# csv파일을 읽어 데이터프레임으로 나타낸다.
comment_file = pd.read_csv("C:/Users/pps/Desktop/TIL/youtube_comments_crawling.csv", encoding= 'utf-8')

# 댓글텍스트 column을 리스트로 변환한다.
comment_list = comment_file['댓글텍스트'].to_list()

# 문자열 만들기(중간에 숫자가 껴 있는 경우)
comment_string = ''.join(str(s) for s in comment_list)
```

---
### 2. 명사, 동사, 형용사별로 분리하는 각각의 함수 만들기

>[!reference]
>[kiwi 태그 목록](https://bab2min.github.io/kiwipiepy/v0.17.1/kr/#_9)

#### 명사

- 참고
```
# kiwi의 Analyze함수 형태
[([Token(form='김하성', tag='NNP', start=0, len=3)])] 
```

```python
# kiwi 명사 추출 함수
from kiwipiepy import Kiwi
kiwi = Kiwi()

def kiwi_noun_extractor(text):
    results = []
    result = kiwi.analyze(text)
	# token = 단어 이름, pos = 태그 이름
    for token, pos, _, _ in result[0][0]:
        # kiwi의 태그 목록 체언:
        # NNG: 일반 명사, NNP: 고유 명사, NNB: 의존 명사, NR: 수사, NP: 대명사
        if len(token) != 1 and pos.startswith('N'): #or pos.startswith('SL'):
            results.append(token)
    return results
```

---
#### 동사

```python
# kiwi 동사 추출 함수
def kiwi_verb_extractor(text):
    results = []
    result = kiwi.analyze(text)
    for token, pos, _,_ in result[0][0]:
	    # kiwi의 태그 목록 용언:
	    # VV: 동사
        if len(token) != 1 and pos.startswith('VV'):
            results.append(token)
    return results
```

---
#### 형용사

```python
# kiwi 형용사 추출 함수
def kiwi_adj_extractor(text):
    results = []
    result = kiwi.analyze(text)
    for token, pos,_,_ in result[0][0]:
        # kiwi의 태그 목록 용언:
        # VA: 형용사
        if len(token) != 1 and pos.startswith('VA'):
            results.append(token)
    return results
```

---
### 워드 클라우드를 이용한 시각화

- 대표적으로 명사만 시각화하겠다.
#### 명사

```python
from wordcloud import WordCloud
import matplotlib.pyplot as plt
from collections import Counter

def wc_kiwi_noun(noun_text):
    cnt = len(noun_text)
    counts = Counter(noun_text)
    tags_Nkiwi = counts.most_common(cnt)
    wc_k = WordCloud(font_path='C:/Users/pps/AppData/Local/Microsoft/Windows/Fonts/NanumBarunGothic.ttf', background_color='white', width=800, height=600)
    cloud_kiwi = wc_k.generate_from_frequencies(dict(tags_Nkiwi))
  
    plt.figure(figsize = (10, 8))
    plt.axis('off')
    plt.imshow(cloud_kiwi)

    return plt.show()
```

```python
word_cloud_noun_kiwi = wc_kiwi_noun(nouns_kiwi)
```

- 결과값
![](https://imgur.com/akXIU2K.jpg)

