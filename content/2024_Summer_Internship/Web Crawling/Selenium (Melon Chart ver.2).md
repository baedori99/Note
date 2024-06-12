---
title: Selenium_Crawling_practice2
create_date: 2024-06-12 17:06 - 2024-06-12 17:06
---
# Selenium Crawling Melon Hot Chart 100 Information

>[!Todo] STEP
>1. 웹 드라이버 만들기
>2. 웹 열기
>3. 요소 찾기 / 웹 제어하기

---

>[!tip] Selenium 자료
>1. [Selenium Documentation](https://www.selenium.dev/documentation/)
>2. [Selenium-Python Documentation](https://selenium-python.readthedocs.io/index.html)

---
## <실습2>

>[!Todo] 멜론 차트 홈페이지부터 시작
>1. 멜론 차트 선택
>	- HOT 100 선택
>2. 1위 ~ 100위 곡 정보의 아이콘 클릭 for문을 통해 정보 추출
>	`for`
>		`1.곡 정보의 아이콘 클릭`
>		`2.곡 이름, 가수명, 장르, 좋아요, 가사 크롤링` 
>		`3. 뒤로가기`
>3. 데이터프레임

---
### 1. Selenium 받아오기

```python
from selenium import webdriver
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.common.by import By
from selenium.webdriver.common.actions.action_builder import ActionBuilder
from selenium.webdriver.common.actions.mouse_button import MouseButton
from selenium.webdriver import ActionChains
import time
import pandas as pd
```

---

### 2. 크롬 창 열기, 멜론 홈페이지 접속

```python
# 크롬 창 열기
options = Options()
driver = webdriver.Chrome(options=options)

# 멜론 홈페이지 접속
driver.get("https://www.melon.com/index.htm")
```

---

### 3. 멜론 차트 경로, HOT 100 경로 

```python
# 멜론 차트 경로 찾기

melon_charts = driver.find_element(By.XPATH, '/html/body/div[1]/div[2]/div/div[2]/ul[1]/li[1]/a/span[2]')

# 멜론 차트 버튼 누르기
ActionChains(driver).click(melon_charts).perform()

# 2초 시간 기다리기
time.sleep(2)

# HOT100 경로 찾기
hot100_charts = driver.find_element(By.XPATH, '//*[@id="gnb_menu"]/ul[1]/li[1]/div/ul/li[2]/a/span')

# HOT100 버튼 누르기
ActionChains(driver).click(hot100_charts).perform()
```

---
### 4. 곡 정보 경로 찾기, 버튼 누르기

```python
# rows 출력
rows_50 = driver.find_elements(By.CLASS_NAME, 'lst50')
rows_100 = driver.find_elements(By.CLASS_NAME, 'lst100')

  

# 곡 정보 경로 찾기
song_info = rows_50[0].find_element(By.CLASS_NAME, 'btn.button_icons.type03.song_info')

  

# 곡 정보 버튼 누르기
ActionChains(driver).click(song_info).perform()
```

---
### 5. 데이터 받기

```python
# 곡 이름 가져오기
temp = driver.find_elements(By.ID, 'conts')
song_name = temp[0].find_elements(By.CLASS_NAME, 'song_name')

# 곡 이름 출력하기
for i in song_name:
    title = i.text
    print(title)
```

```python
# 가수 이름 가져오기
singer_name = temp[0].find_elements(By.XPATH, '//*[@id="downloadfrm"]/div/div/div[2]/div[1]/div[2]/a/span[1]')

# 가수 이름 출력하기
for i in singer_name:
    singer = i.text
    print(singer)
```

```python
# 장르 가져오기
genres = temp[0].find_elements(By.XPATH, '//*[@id="downloadfrm"]/div/div/div[2]/div[2]/dl/dd[3]')

# 장르 출력하기
for i in genres:
    genre = i.text
    print(genre)
```

```python
# 좋아요수 가져오기
likes = temp[0].find_elements(By.ID, 'd_like_count')

# 좋아요 출력하기
for i in likes:
    like = i.text
    print(like)
```

```python
# 가사 가져오기
lyrics = temp[0].find_elements(By.CLASS_NAME, 'lyric')

# 가사 출력하기
k = []
for i in lyrics:
    k.append(i.text.replace('\n',' '))
```

---
### 6. for문을 통해 데이터 전체 받기

```python

# 데이터를 모아둘 빈 딕셔너리 만들기
all_info = {"Title":[],"Singer":[],"Genre":[],"Like":[],"Lyric":[]}

for i in range(100):

    if i < 100:
        song_info = rows_50[i].find_element(By.CLASS_NAME, 'btn.button_icons.type03.song_info')

        ActionChains(driver).click(song_info).perform()

        info_box = driver.find_elements(By.ID, 'conts')

        song_name = info_box[0].find_element(By.CLASS_NAME, 'song_name').text

        singer_name = info_box[0].find_element(By.XPATH, '//*[@id="downloadfrm"]/div/div/div[2]/div[1]/div[2]/a/span[1]').text

        genres = info_box[0].find_element(By.XPATH, '//*[@id="downloadfrm"]/div/div/div[2]/div[2]/dl/dd[3]').text

        likes = info_box[0].find_element(By.ID, 'd_like_count').text

        lyrics = info_box[0].find_element(By.CLASS_NAME, 'lyric').text.replace("\n",' ')

     elif i >= 50 or i < 100:
         song_info = rows_100[i-50].find_element(By.CLASS_NAME, 'btn.button_icons.type03.song_info')

         ActionChains(driver).click(song_info).perform()

         info_box = driver.find_elements(By.ID, 'conts')

         song_name = info_box[0].find_elements(By.CLASS_NAME, 'song_name')

         singer_name = info_box[0].find_elements(By.XPATH, '//*[@id="downloadfrm"]/div/div/div[2]/div[1]/div[2]/a/span[1]')

         genres = info_box[0].find_elements(By.XPATH, '//*[@id="downloadfrm"]/div/div/div[2]/div[2]/dl/dd[3]')

         likes = info_box[0].find_elements(By.ID, 'd_like_count')

         lyrics = info_box[0].find_elements(By.CLASS_NAME, 'lyric')

    all_info['Title'].append(song_name)
    all_info['Singer'].append(singer_name)
    all_info['Genre'].append(genres)
    all_info['Like'].append(likes)
    all_info['Lyric'].append(lyrics)

	# 뒤로가기
    driver.back()
```

- `driver.back()`: 페이지 뒤로가기

---