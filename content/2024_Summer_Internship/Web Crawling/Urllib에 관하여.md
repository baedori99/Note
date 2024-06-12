---
title: Urllib Module
create_date: 2024-06-04 14:06 - 2024-06-04 14:06
description: 네이버 API를 통해 파이썬으로 뉴스 기사를 불러오는 코드를 공부하는 중에 urllib모듈에 관하여 궁금하여 공부하고자 정리해봄!!!
---
![](https://imgur.com/INp5Ud8.png)
[Python모듈 | urllib 모듈에 대한 출처](https://ctkim.tistory.com/entry/%ED%8C%8C%EC%9D%B4%EC%8D%AC-urllib-%EB%AA%A8%EB%93%88)

***!!!일단 urllib모듈의 request, urlopen, parse.quote, urlencode에 대해서만 다루도록 하자!!!***

### 1. urllib 모듈이란?

urllib 모듈은 **파이썬에서 URL을 다루기 위한 모듈**입니다.
이 모듈은 HTTP, FTP, SMTP 등과 같은 프로토콜을 사용하여 URL을 열고 읽고 쓰는 기능입니다.
- <span style="color:gray"> HTTP (HyperText Transfer Protocol) : 웹 상에서 문서, 이미지, 동영상 등 다양한 리소스를 전송하기 위한 프로토콜 </span>
- <span style="color:gray"> FTP (File Transfer Protocol) : 컴퓨터 간에 파일을 송수신하기 위해 사용되며, 로컬과 원격 시스템 간의 파일 전송을 쉽게 할 수 있습니다. </span>
- <span style="color:gray"> SMTP (Simple Mail Transfer Protocol) : 이메일 클라이언트와 메일 서버 간의 통신에 사용되며, 메일 서버에서 메일 서버로 메시지를 전달합니다. </span>

urllib 모듈은 별도의 설치 과정이 필요없습니다. 
```python
import urllib
```


---
### 2. urllib.request

urllib.request는 **다양한 방식으로 URL을 열고 데이터를 가져오는 기능을 제공**합니다.


---
#### 2.1 urlopen()

urlopen() 함수는 **URL을 열어서 HTTPResponse 객체를 반환하는 함수**
이 함수는 HTTP, HTTPS, FTP 등의 프로토콜을 지원합니다.
urlopen() 함수는 하나의 인자를 받습니다. 이 인자는 열고자 하는 URL을 나타내는 문자열입니다. 
urlopen() 함수를 호출하면 URL을 열어서 HTTPResponse 객체를 반환합니다. 이 객체는 read() 메서드를 사용하여 응답 본문을 가져올 수 있습니다.

- 코드에서는 urlopen() 함수를 사용하여 'https://www.example.com' URL을 열고, 반환된 HTTPResponse 객체에서 read() 메서드를 사용하여 웹 페이지의 HTML 코드를 읽어들입니다.
```python
import urllib.request

response = urllib.request.urlopen('https://www.example.com')
html = response.read()

print(html)
```


---
#### 2.2 urlretrieve()

urlretrieve() 함수는 **URL에서 파일을 다운로드하는 함수**
이 함수는 URL에서 데이터를 읽어서 로컬 파일로 저장합니다.
urlretrieve() 함수는 두 개의 인자를 받습니다. 첫 번째 인자는 다운로드하려는 파일의 URL을 나타내는 문자열입니다. 두 번째 인자는 로컬에 저장할 파일 경로를 나타내는 문자열입니다.

- 코드에서는 urlretrieve() 함수를 사용하여 'https://www.example.com/image.jpg' URL에서 이미지 파일을 다운로드하고, 로컬 파일 'image.jpg'로 저장합니다.
```python
import urllib.request

url = 'https://www.example.com/image.jpg'
filename = 'image.jpg'

urllib.request.urlretrieve(url, filename)
```


---
#### 2.3 Request

Request는 urllib.request 모듈에 포함된 클래스 중 하나입니다. 이 클래스는 **HTTP 요청을 만들기 위한 클래스로, 헤더 정보, 쿠키 정보 등을 설정**할 수 있습니다. HTTP 요청을 보낼 때 사용되는 GET, POST, PUT, DELETE 등의 메서드를 설정할 수 있습니다. Request는 두 개의 인자를 받습니다. 첫 번째 인자는 URL을 나타내는 문자열입니다. 두 번째 인자는 옵션을 나타내는 딕셔너리입니다. 딕셔너리의 키는 헤더 정보를 나타내는 문자열이며, 값은 해당 헤더 정보의 값입니다.

- 아래 코드는 Python언어로 작성된 웹 스크레이핑 코드입니다. (**웹 스크레핑이란 웹사이트에서 원하는 정보를 추출하는 과정**을 말합니다.)
- 코드에서는 urllib.parse 모듈의 urlencode를 사용하여 URL에 데이터를 추가합니다. HTTP 요청을 만들고, 헤더 정보를 설정합니다. 코드의 urlencode는 아래 4.2에서 설명합니다.
```python
import urllib.request
import urllib.parse

# 검색어인 'python'을 'data'딕셔너리에 할당. 여기서 'q':'python'은 검색어 파라미터를 나타냄
url = 'https://www.example.com/search'
# urlencode를 사용하여 data 딕셔너리를 URL 인코딩된 문자열로 반환.이 경우 변환된 문자열은 
# 'q=python'이 됩니다.
data = {'q' : 'python'}

data = urllib.parse.urlencode(data)
# 변환된 문자열을 URL 끝에 붙여 완성된 URL을 생성
url = url + '?' + data

# 여기서 User-Agent는 웹 브라우저의 종류를 나타내고 Referer는 요청이 시작된 웹페이지의 URL을 나타냄
headers = {
		   'User=Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64)',
		   'Referer' : 'https://www.example.com'
}

req = urllib.request.Request(url, headers=headers)
response = urllib.request.urlopen(req)

html = response.read()
print(html)
```


---
### 4. urllib.parse

urllib.parse 모듈은 **URL을 구성 요소로 분리**하고, **URL을 인코딩하고 디코딩**하며, URL 쿼리 문자열을 **구문 분석**하는 등의 기능을 제공합니다.


---
#### 4.2 urlencode()

urlencode() 함수는 딕셔너리를 URL 쿼리 문자열로 인코딩하기 위해 사용됩니다. 이 함수는 **딕셔너리의 키-값 쌍을 인코딩하여 URL 쿼리 문자열을 생성**합니다. urlencode() 함수는 주로 웹 애플리케이션에서 GET 요청의 쿼리 문자열을 생성하는 데 사용됩니다.

- 코드에서는 딕셔너리를 생성하고, 이를 urlencode() 함수에 전달하여 URL 쿼리 문자열을 생성합니다. 이를 통해 생성된 URL 쿼리 문자열을 기존 URL과 조합하여 새로운 URL을 만듭니다.
```python
from urllib.parse import urlencode

params = {'name': 'Alice', 'age': 25, 'city': 'New York'}

quary_string = urlencode(params)

url = 'https://www.example.com/search?' + query_string

print(url)
```


---
#### 4.4 quote()

quote() 함수는 URL 안전 문자열(**문자열이 변환되지 않고 그대로 사용되는 문자열**/ URL에 사용할 수 있는 문자)로 인코딩하기 위해 사용됩니다. 이 함수는 문자열의 특정 문자를 URL 안전 문자열로 인코딩하여 반환합니다. 인코딩된 문자열은 URL의 일부로 사용될 수 있습니다. quote() 함수는 대부분의 ASCII 문자를 인코딩하지 않고, 일부 특수 문자만 인코딩합니다. 이 함수는 주로 URL에 포함될 수 없는 문자를 처리하는 데 사용됩니다.

***URL에 사용할 수 있는 문자***
	*1. ASCII chatacters
		- 영문자, 숫자
		- 달러기호($), 하이폰(-), 밑줄(_), 마침표(.), 덧셈기호(+), 느낌표(!), 별표(*), 아포스트로피('), 괄호(())

- 코드에서는 문자열을 생성하고, 이를 quote() 함수에 전달하여 URL 안전 문자열로 인코딩합니다. 이를 통해 생성된 인코딩된 문자열을 기존 URL과 조합하여 새로운 URL을 만듭니다.
```python
from urllib.parse import quote

url = 'https://www.example.com/search?q=' + quote('Alice & Bob')

print(url)
```


---
#### 4.5 unquote()

unquote() 함수는 URL 안전 문자열을 디코딩하기 위해 사용됩니다. 이 함수는 URL 안전 문자열로 인코딩된 문자열을 디코딩하여 반환합니다. unquote() 함수는 quote() 함수와 반대로 동작합니다.

- 코드에서는 URL을 생성하고, 이를 파싱하여 딕셔너리로 변환합니다. 이를 통해 URL 안전 문자열로 인코딩된 문자열을 디코딩하고, 디코딩된 값을 딕셔너리로 저장합니다.
```python
from urllib.parse import unquote

url = 'https://www.example.com/search?q=Alice%20%26%20Bob'

query_string = url.split('?')[1]

params = {}

for param in query_string.split('&'):
	key, value = param.split('=')
	params[key] = unquote(value)

print(params['q'])
```