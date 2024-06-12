---
title: Selenium_Crawling_practice1
create_date: 2024-06-12 12:06 - 2024-06-12 12:06
---
# Selenium Crawling Melon Chart Hot 100

>[!Todo] STEP
>1. 웹 드라이버 만들기
>2. 웹 열기
>3. 요소 찾기 / 웹 제어하기

---

>[!tip] Selenium 자료
>1. [Selenium Documentation](https://www.selenium.dev/documentation/)
>2. [Selenium-Python Documentation](https://selenium-python.readthedocs.io/index.html)

---
## <실습1>

>[!Todo] 멜론 차트 홈페이지부터 시작
>1. 멜론 차트 선택 - HOT 100 선택
>2. 순위, 곡정보(곡 이름), 곡정보(가수), 좋아요 크롤링
>3. 데이터 프레임

---
### 1. Selenium 받아오기

```python
from selenium import webdriver
from selenium.webdriver.chrome.options import Options # options를 써줘야 Chrome 사용가능한듯 하다.
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.common.by import By
from selenium.webdriver.common.actions.action_builder import ActionBuilder
from selenium.webdriver.common.actions.mouse_button import MouseButton
from selenium.webdriver import ActionChains
import time
import pandas as pd
```

---
### 2. 크롬 창 열기, 멜론 홈페이지 접속하기

```python
# 크롬 창 열기
options = Options()
driver = webdriver.Chrome(options=options)

# 멜론 홈페이지 접속
driver.get("https://www.melon.com/index.htm")
```

---
### 3. 멜론 차트 경로 찾기, 멜론 차트 버튼 누르기

```python
# 멜론 차트 경로 찾기
melon_charts = driver.find_element(By.XPATH, '/html/body/div[1]/div[2]/div/div[2]/ul[1]/li[1]/a/span[2]')

# 멜론 차트 버튼 누르기
ActionChains(driver).click(melon_charts).perform()
```

---

>[!tip] Tip!!
>1. `CLASS_NAME` 중간의 공백은 . 으로 바꾸기
>2. find_element().click() 가능 하다.
>	- ex) driver.find_element(By.CLASS_NAME, 'menu_bg.menu02 ').click()

---

>[!caution] Caution!!!
>- 페이지를 다 불러오지 않은 상태에서 다음 코드로 넘어가는 상황이 발생할 수 있다.
>- 위와 같은 상황을 예방하고자 `import time`을 받고 `time.sleep(number)`를 넣어 대기 시간을 갖게 한다.

- `selenium` 에서 자체로 대기시간을 주는 함수가 있다.
- [Waits](https://selenium-python.readthedocs.io/waits.html)
```python
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

# 로드 될 때까지 대기
wait = WebDriverWait(driver, 10)
wait.until(EC.presence_of_element_located((By.TAG_NAME, "body")))
```

---
### 4. HOT100 경로 찾기, HOT100 버튼 누르기

```python
# HOT100 경로 찾기
hot100_charts = driver.find_element(By.XPATH, '//*[@id="gnb_menu"]/ul[1]/li[1]/div/ul/li[2]/a/span')

# HOT100 버튼 누르기
ActionChains(driver).click(hot100_charts).perform()
```

---
### 5. for문을 통해 데이터 받기

```python
# rows 출력
rows_50 = driver.find_elements(By.CLASS_NAME, 'lst50')
rows_100 = driver.find_elements(By.CLASS_NAME, 'lst100')

# 순위, 곡명, 가수명, 좋아요수 받을 빈 딕셔너리 만들기
hot_chart = {"Rank": [],"Title":[],"Singer":[],"Like":[]}

# 1~100위까지 순위, 곡정보(곡명), 곡정보(가수명), 좋아요수 가지고 오기
all_counts = len(rows_50) + len(rows_100)

  

for i in range(all_counts):
    if i < 50:
        rank = rows_50[i].find_element(By.CLASS_NAME, 'rank ').text
        
        title = rows_50[i].find_element(By.CSS_SELECTOR, '#lst50 > td:nth-child(6) > div > div > div.ellipsis.rank01 > span > a').text
        
        singer = rows_50[i].find_element(By.CSS_SELECTOR, '#lst50 > td:nth-child(6) > div > div > div.ellipsis.rank02 > a').text
        
        likes = rows_50[i].find_element(By.CLASS_NAME, 'cnt').text

    elif i >= 50 or i < 100:
        rank = rows_100[i-50].find_element(By.CLASS_NAME, 'rank ').text
        
        title = rows_100[i-50].find_element(By.CSS_SELECTOR, '#lst100 > td:nth-child(6) > div > div > div.ellipsis.rank01 > span > a').text
        
        singer = rows_100[i-50].find_element(By.CSS_SELECTOR, '#lst100 > td:nth-child(6) > div > div > div.ellipsis.rank02 > a').text
        
        likes = rows_100[i-50].find_element(By.CLASS_NAME, 'cnt').text

    hot_chart["Rank"].append(rank)
    hot_chart["Title"].append(title)
    hot_chart["Singer"].append(singer)
    hot_chart["Like"].append(likes)
```

- `driver.quit()`: 더이상 창을 쓰지 않게 되면 자동으로 닫힌다.
---
### 6. 데이터 프레임으로 만들기

```python
pd.DataFrame(hot_chart)
```

---
## 최종 코드

```python
from selenium import webdriver
from selenium.webdriver.chrome.options import Options # options를 써줘야 Chrome 사용가능한듯 하다.
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.common.by import By
from selenium.webdriver.common.actions.action_builder import ActionBuilder
from selenium.webdriver.common.actions.mouse_button import MouseButton
from selenium.webdriver import ActionChains
import time
import pandas as pd

  

def melon_chart_hot100(url):

    # 크롬 창 열기
    options = Options()
    driver = webdriver.Chrome(options=options)

    # 멜론 홈페이지 접속
    driver.get(url)

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

    # rows 출력
    rows_50 = driver.find_elements(By.CLASS_NAME, 'lst50')
    rows_100 = driver.find_elements(By.CLASS_NAME, 'lst100')

    # 순위, 곡명, 가수명, 좋아요수 받을 빈 딕셔너리 만들기
    hot_chart = {"Rank": [],"Title":[],"Singer":[],"Like":[]}

    # 1~100위까지 순위, 곡정보(곡명), 곡정보(가수명), 좋아요수 가지고 오기
    all_counts = len(rows_50) + len(rows_100)

    for i in range(all_counts):
        if i < 50:
            rank = rows_50[i].find_element(By.CLASS_NAME, 'rank ').text

            title = rows_50[i].find_element(By.CSS_SELECTOR, '#lst50 > td:nth-child(6) > div > div > div.ellipsis.rank01 > span > a').text

            singer = rows_50[i].find_element(By.CSS_SELECTOR, '#lst50 > td:nth-child(6) > div > div > div.ellipsis.rank02 > a').text

            likes = rows_50[i].find_element(By.CLASS_NAME, 'cnt').text

        elif i >= 50 or i < 100:
            rank = rows_100[i-50].find_element(By.CLASS_NAME, 'rank ').text

            title = rows_100[i-50].find_element(By.CSS_SELECTOR, '#lst100 > td:nth-child(6) > div > div > div.ellipsis.rank01 > span > a').text

            singer = rows_100[i-50].find_element(By.CSS_SELECTOR, '#lst100 > td:nth-child(6) > div > div > div.ellipsis.rank02 > a').text

            likes = rows_100[i-50].find_element(By.CLASS_NAME, 'cnt').text

        hot_chart["Rank"].append(rank)
        hot_chart["Title"].append(title)
        hot_chart["Singer"].append(singer)
        hot_chart["Like"].append(likes)

    # 창 닫기
    driver.quit()

    # 데이터 프레임 만들기
    df_hotchart = pd.DataFrame(hot_chart)
    
    return df_hotchart

url = "https://www.melon.com/index.htm"
melon_chart_hot100(url)
```