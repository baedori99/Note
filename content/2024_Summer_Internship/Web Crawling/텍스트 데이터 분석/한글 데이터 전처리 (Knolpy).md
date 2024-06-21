---
title: Korean Data Preprocessing (knolpy)
create_date: 2024-06-17 17:06 - 2024-06-17 17:06
draft: false
tags:
  - "#WebCrawling"
  - "#Pandas"
---
# Korean Data Preprocessing

>[!info] Morpheme Analyzer 목록
>1. Hannanum
>2. Komoran
>3. Kkma
>4. Mecab
>5. Okt

>[!reference]
>[konlpy(Korean Natural Language Processing)](https://konlpy.org/ko/stable/)
>[한국어 품사 태그 비교표](https://docs.google.com/spreadsheets/d/1OGAjUvalBuX-oZvZ_-9tEfYD2gQe7hTGsgUpiiBSXI8/edit?gid=0#gid=0)

---
## 형태소 분석기
### Hannanum
```python
from konlpy.tag import Hannanum
hannanum = Hannanum()
```

- `Analyze`: 형태소 후보를 모두 반환
```python
# Analyze
print(hannanum.analyze(u'샌디에이고의 야구엔 감동이 있다.'))
```

```
# 결과값
[[[('샌디에이고', 'ncn'), ('의', 'jcm')], [('샌디에이고', 'ncn'), ('의', 'ncn')]], [[('야구', 'ncn'), ('엔', 'jca')]], [[('감동', 'ncpa'), ('이', 'jcc')], [('감동', 'ncpa'), ('이', 'jcs')], [('감동', 'ncpa'), ('이', 'ncn')], [('감동', 'ncps'), ('이', 'jcc')], [('감동', 'ncps'), ('이', 'jcs')], [('감동', 'ncps'), ('이', 'ncn')]], [[('있', 'paa'), ('다', 'ef')], [('있', 'px'), ('다', 'ef')]], [[('.', 'sf')], [('.', 'sy')]]]
```

- `Morph`: 형태소로 나누기
```python
# morphs
print(hannanum.morphs(u'샌디에이고의 야구엔 감동이 있다.'))
```

```
# 결과값
['샌디에이고', '의', '야구', '엔', '감동', '이', '있', '다', '.']
```

- `Nouns`: 명사로 나누기
```python
# nouns
print(hannanum.nouns(u'샌디에이고의 야구엔 감동이 있다.'))
```

```
# 결과값
['샌디에이고', '야구', '감동']
```

- `pos`: 형태소로 나누고, 태그까지 반환
```python
# pos
print(hannanum.pos(u'샌디에이고의 야구엔 감동이 있다.'))
```

```
# 결과값
[('샌디에이고', 'N'), ('의', 'J'), ('야구', 'N'), ('엔', 'J'), ('감동', 'N'), ('이', 'J'), ('있', 'P'), ('다', 'E'), ('.', 'S')]
```

---
### Komoran

```python
from konlpy.tag import Komoran
komoran = Komoran()
```

- `Morph`: 형태소로 나누기
```python
# morphs
print(komoran.morphs(u'샌디에이고의 야구엔 감동이 있다!'))
```

```
# 결과값
['샌디에이고', '의', '야구', '에', 'ㄴ', '감동', '이', '있', '다', '!']
```

- `Nouns`: 명사로 나누기
```python
# nouns
print(komoran.nouns(u'샌디에이고의 야구엔 감동이 있다!'))
```

```
# 결과값
['샌디에이고', '야구', '감동']
```

- `pos`: 형태소로 나누고, 태그까지 반환
```python
# pos
print(komoran.pos(u'샌디에이고의 야구엔 감동이 있다!'))
```

```
# 결과값
[('샌디에이고', 'NNP'), ('의', 'JKG'), ('야구', 'NNG'), ('에', 'JKB'), ('ㄴ', 'JX'), ('감동', 'NNG'), ('이', 'JKS'), ('있', 'VX'), ('다', 'EF'), ('!', 'SF')]
```

---
### Kkma

- 속도가 오래 걸려서 많이 안쓴다
```python
from konlpy.tag import Kkma
kkma = Kkma()
```

- `Morph`: 형태소로 나누기
```python
# morphs
print(kkma.morphs(u'샌디에이고의 야구엔 감동이 있다!'))
```

```
# 결과값
['새', 'ㄴ', '디에이', '고의', '야구', '에', '는', '감동', '이', '있', '다', '!']
```

- `Nouns`: 명사로 나누기
```python
# nouns
print(kkma.nouns(u'샌디에이고의 야구엔 감동이 있다!'))
```

```
# 결과값
['디에이', '디에이고의', '고의', '야구', '감동']
```

- `pos`: 형태소로 나누고, 태그까지 반환
```python
# pos
# 줄임말 부분도 풀어서 적음
print(kkma.pos(u'샌디에이고의 야구엔 감동이 있다!'))
```

```
# 결과값
[('새', 'VV'), ('ㄴ', 'ETD'), ('디에이', 'NNG'), ('고의', 'NNG'), ('야구', 'NNG'), ('에', 'JKM'), ('는', 'JX'), ('감동', 'NNG'), ('이', 'JKS'), ('있', 'VV'), ('다', 'EFN'), ('!', 'SF')]
```

---
### Okt

- 자주 쓰이고 속도도 빠르다.
```python
from konlpy.tag import Okt
okt = Okt()
```

- `Morph`: 형태소로 나누기
```python
# morphs
print(okt.morphs(u'샌디에이고의 야구엔 감동이 있다!'))
```

```
# 결과값
['샌디에이고', '의', '야구', '엔', '감동', '이', '있다', '!']
```

- `Nouns`: 명사로 나누기
```python
# nouns
print(okt.nouns(u'샌디에이고의 야구엔 감동이 있다!'))
```

```
# 결과값
['샌디에이고', '야구', '감동']
```

- `pos`: 형태소로 나누고, 태그까지 반환
```python
# pos1
print(okt.pos(u'샌디에이고의 야구엔 감동이 있다!'))

# pos2
print(okt.pos(u'샌디에이고의 야구엔 감동이 있다!', norm=True))

# pos3
print(okt.pos(u'샌디에이고의 야구엔 감동이 있다!', norm = True, stem=True))
print(okt.pos(u'샌디에이고의 야구엔 감동이 있다!', join=True))
```

```
# 결과값
# pos1
[('샌디에이고', 'Noun'), ('의', 'Josa'), ('야구', 'Noun'), ('엔', 'Josa'), ('감동', 'Noun'), ('이', 'Josa'), ('있다', 'Adjective'), ('!', 'Punctuation')]
# pos2
[('샌디에이고', 'Noun'), ('의', 'Josa'), ('야구', 'Noun'), ('엔', 'Josa'), ('감동', 'Noun'), ('이', 'Josa'), ('있다', 'Adjective'), ('!', 'Punctuation')]
# pos3
[('샌디에이고', 'Noun'), ('의', 'Josa'), ('야구', 'Noun'), ('엔', 'Josa'), ('감동', 'Noun'), ('이', 'Josa'), ('있다', 'Adjective'), ('!', 'Punctuation')] ['샌디에이고/Noun', '의/Josa', '야구/Noun', '엔/Josa', '감동/Noun', '이/Josa', '있다/Adjective', '!/Punctuation']
```

---
### Analyzer별 결과값

- `Morph`
```python
# Morph
print(hannanum.morphs(temp_string))
print(komoran.morphs(temp_string))
print(kkma.morphs(temp_string))
print(okt.morphs(temp_string))
```

```
# 결과값
['샌디에이고', '의', '야구', '엔', '감동', '이', '있', '다', '!']
['샌디에이고', '의', '야구', '에', 'ㄴ', '감동', '이', '있', '다', '!']
['새', 'ㄴ', '디에이', '고의', '야구', '에', '는', '감동', '이', '있', '다', '!']
['샌디에이고', '의', '야구', '엔', '감동', '이', '있다', '!']
```

- `Noun`
```python
print(hannanum.nouns(temp_string))
print(komoran.nouns(temp_string))
print(kkma.nouns(temp_string))
print(okt.nouns(temp_string))
```

```
# 결과값
['샌디에이고', '야구', '감동']
['샌디에이고', '야구', '감동']
['디에이', '디에이고의', '고의', '야구', '감동']
['샌디에이고', '야구', '감동']
```

- `Pos`
```python
print(hannanum.pos(temp_string))
print(komoran.pos(temp_string))
print(kkma.pos(temp_string))
print(okt.pos(temp_string))
```

```
# 결과값
[('샌디에이고', 'N'), ('의', 'J'), ('야구', 'N'), ('엔', 'J'), ('감동', 'N'), ('이', 'J'), ('있', 'P'), ('다', 'E'), ('!', 'S')]
[('샌디에이고', 'NNP'), ('의', 'JKG'), ('야구', 'NNG'), ('에', 'JKB'), ('ㄴ', 'JX'), ('감동', 'NNG'), ('이', 'JKS'), ('있', 'VX'), ('다', 'EF'), ('!', 'SF')]
[('새', 'VV'), ('ㄴ', 'ETD'), ('디에이', 'NNG'), ('고의', 'NNG'), ('야구', 'NNG'), ('에', 'JKM'), ('는', 'JX'), ('감동', 'NNG'), ('이', 'JKS'), ('있', 'VV'), ('다', 'EFN'), ('!', 'SF')]
[('샌디에이고', 'Noun'), ('의', 'Josa'), ('야구', 'Noun'), ('엔', 'Josa'), ('감동', 'Noun'), ('이', 'Josa'), ('있다', 'Adjective'), ('!', 'Punctuation')]
```

---
## 워드 클라우드 명사, 동사, 형용사 만들기

>[!Todo] 과제 목록
>1. 유튜브 댓글 크롤링한 csv 파일의 댓글 텍스트를 텍스트로 쓴다.
>2. 명사, 동사, 형용사별로 분리하는 okt 함수를 만든다.
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
### 2. Okt를 이용한 명사, 동사, 형용사별로 분리하는 각각의 함수 만들기

>[!reference]
>[Knolpy 태그 비교표](https://docs.google.com/spreadsheets/d/1OGAjUvalBuX-oZvZ_-9tEfYD2gQe7hTGsgUpiiBSXI8/edit?gid=0#gid=0)

#### 명사

```python
from konlpy.tag import Okt
okt = Okt()

# Okt 명사 추출
def okt_noun_extractor(text):
    return okt.nouns(text)
```

---
#### 동사

```
# okt.pos의 형태
[('김하성','Verb')]
```

```python
# Okt 동사 추출 함수
def okt_verb_extractor(text):
    results = []
    result = okt.pos(text)
    # token = 단어 이름, pos = 태그 이름
    for token, pos in result:
        if pos == 'Verb':
            results.append(token)
    return results
```

---
#### 형용사

```python
# Okt 형용사 추출 함수
def okt_adj_extractor(text):
    results = []
    result = okt.pos(text)
    for token, pos in result:
        if len(token) != 1 and pos.startswith('Adj'):
            results.append(token)
    return results
```

---
### 워드 클라우드를 이용한 시각화

- 대표적으로 명사만 하겠다.

```python
from wordcloud import WordCloud
import matplotlib.pyplot as plt
from collections import Counter

def wc_okt_noun(noun_text):
    cnt = len(noun_text)
    counts = Counter(noun_text)
    tags_okt = counts.most_common(cnt)
    wc_k = WordCloud(font_path='C:/Users/pps/AppData/Local/Microsoft/Windows/Fonts/NanumBarunGothic.ttf', background_color='white', width=800, height=600)
    cloud_okt = wc_k.generate_from_frequencies(dict(tags_okt))

    plt.figure(figsize = (10, 8))
    plt.axis('off')
    plt.imshow(cloud_okt)
    return plt.show()
```

```python
word_cloud_noun_okt = wc_okt_noun(nouns_okt)
```

- 결과값
![](https://imgur.com/qRAybtB.jpg)