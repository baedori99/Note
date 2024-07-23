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

