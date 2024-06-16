---
title: Youtube 댓글 크롤링
create_date: 2024-06-16 22:06 - 2024-06-16 22:06
draft: true
---
# Youtube 댓글 크롤링

> [!info] 목적
> - 예시 
> 	- 브이로그의 댓글이 악플이 달렸는지 선플이 달렸는지 확인을 하고 싶다. -> 브이로그의 주소를 받아서 쓰면 된다
> 	- 축구경기를 할 때 사람들의 반응을 보고 싶다. -> 유튜브 검색창에 축구를 치고 영상에 들어가서 댓글을 크롤링하면 된다.
> - 목적
> 	- 상황: 대형 유튜버인 내가 내 유튜브 영상에 있는 댓글들에서 악플을 찾고 싶다
> 	- 댓글들을 수집해보자
>	🗒️ 과제 시작하기 전 practice 

---

>[!Todo] Step
>1. scroll 횟수 = 5번
>2. 댓글 수집 시작
>	1. 대댓글 ❌
>	2. ID / 댓글 / 좋아요 Data 크롤링
>	3. Data에 video_id열 추가 (`v=video_id`)

---
### URL 파악하기

- Youtube의 도메인은 "https://www.youtube.com/"
- 내가 크롤링하고자 하는 영상의 주소는 "https://www.youtube.com/watch?v=SsJg6x-inFw"
- 키
    - `watch?v=`
        - 뒤에 영상의 고유주소

---
### 크롬 창 생성 및 유튜브 영상 주소로 접속하기

```python
from selenium import webdriver
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.common.by import By
from selenium.webdriver import ActionChains
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
import time
import pandas as pd
```

```python
# 크롬창 열기
options = Options()
driver = webdriver.Chrome(options=options)
driver.maximize_window()
wait = WebDriverWait(driver, 10)
wait.until(EC.presence_of_element_located((By.TAG_NAME, "body")))
video_id = "SsJg6x-inFw"

# 유튜브 주소 들어가기
driver.get("https://www.youtube.com/watch?v=" + video_id)
```

---
### 스크롤 5번 하기

- 연구원님이 알려주신 방법이다. 
	- `while True`를 `for`문 대신에 사용하면 무한 스크롤이 가능하다.

```python
# 스크롤 5번하기
n_scroll = 5
ActionChains(driver).send_keys("k").perform()
last_height = driver.execute_script("return document.documentElement.scrollHeight")

  

# for i in range(5):

#     driver.execute_script("window.scrollTo(0, document.documentElement.scrollHeight);")

#     new_height = driver.execute_script("return document.documentElement.scrollHeight")

#     if new_height == last_height:

#         break

#     last_height = new_height

  

# time.sleep(2)

  

# 페이지를 5번 스크롤

# for _ in range(n_scroll):

#     # JavaScript를 사용하여 페이지 스크롤

#     driver.execute_script("window.scrollBy(0, window.innerHeight);")

#     # 각 스크롤 후 약간의 지연을 추가하여 페이지가 로드될 시간을 줍니다.

#     time.sleep(1)

#     wait = WebDriverWait(driver, 10)

#     wait.until(EC.presence_of_element_located((By.TAG_NAME, "body")))

  

container=driver.find_element(By.TAG_NAME, "html")
before_top = driver.execute_script("return arguments[0].scrollTop", container)

  

for i in range(5):
    driver.execute_script("arguments[0].scrollTop = arguments[0].scrollHeight", container)
    after_top = driver.execute_script("return arguments[0].scrollTop", container)
    time.sleep(2)

    # if int(before_top) == int(after_top):

    #     break
    before_top = after_top
```

---
### 아이디, 댓글, 좋아요 수 가져오기

- 아이디
```python
# 아이디 가져오기기
y_id = driver.find_elements(By.CSS_SELECTOR, '#author-text')[2]
```

- 댓글
```python
# 댓글 받아오기
y_comment = driver.find_elements(By.CSS_SELECTOR, '#content-text')
```

- 좋아요 수
```python
# 좋아요 받아오기
y_like = driver.find_elements(By.XPATH, '//*[@id="vote-count-middle"]')
```

---
### for문을 통한 아이디, 댓글, 좋아요 수 크롤링

```python
# 댓글의 개수 세기
cnt = driver.find_elements(By.CLASS_NAME, 'style-scope ytd-comment-view-model')

# 데이터 받을 빈 딕셔너리 만들
data_dict = {'ID':[],'Comments':[],'Likes':[]}


for i in range(len(cnt)):
    y_id = driver.find_elements(By.CSS_SELECTOR, '#author-text')[i].text
    y_comment = driver.find_elements(By.CSS_SELECTOR, '#content-text')[i].text
    y_like = driver.find_elements(By.XPATH, '//*[@id="vote-count-middle"]')[i].text

    data_dict["ID"].append(y_id)
    data_dict["Comments"].append(y_comment)
    data_dict["Likes"].append(y_like)

y_comment_info = pd.DataFrame(data_dict)
print(y_comment_info)
```

- 출력값
![](https://imgur.com/Umwv5bS.jpg)

---
### Column에 video_id 추가하기

```python
y_comment_info["Video_id"] = video_id
y_comment_info.set_index("Video_id")
```

- 결과값
![](https://imgur.com/IxmNlsJ.jpg)
