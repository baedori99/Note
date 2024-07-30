---
title: Fast API
create_date: 2024-07-10 11:07 - 2024-07-10 11:07
draft: false
---
이 노트는 [Fast API 공식 문서](https://fastapi.tiangolo.com/ko/tutorial/)를 보고 실습한 내용에 대해서 적는 것이다.
[[네트워크 관련 공부#GET과 POST|GET과 POST]]에 대해서 개념이라도 알고 있어야 한다.

## FastAPI란?

Python 3.6 부터 제공되는 트렌디하고 높은 성능을 가진 ***파이썬 프레임워크***

### 특징

- API 문서 자동 생성 (Swagger와 Redoc 스타일 동일)
- 의존성 주입 위주의 설계를 통한 DB 등에 대한 관리 편리
- 비동기 동작으로 빠른 성능 보장
- Pydantic을 사용한 Validation 체크
- 뛰어난 공식 문서 가이드

## Installation

- 자습시에는 모든 패키지를 설치하는 것을 추천한다.
- 여기에는 코드를 실행하는 서버로 사용할 수 있는 `uvicorn`도 포함이다.
```
pip install fastapi[all]
```

>[!info]- 참고
>부분적으로 설치할 수 있다.  
>- 애플리케이션을 운영 환경에 배포하려는 경우
>```
>pip install fastapi
>```
>- 추가로 서버 역할을 하는 `uvicorn`을 설치한다.  
>```
>pip install uvicorn
>```

## 첫걸음

>[!tip] Tip 
>-  `main.py`에서 실습을 한다.  
>- 라이브 서버를 실행할 때  
>	- `python -m uvicorn main:app --reload`를 `cmd`에 쳐야 실행을 한다.

- 가장 단순한 FastAPI파일
```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
async def root():
    return {"message": "Hello World"}
```

```
# 실행방법
(study) C:\Users\pps\Desktop\TIL\Study_API\Study_FastAPI>python -m  uvicorn main:app --reload

INFO:     Will watch for changes in these directories: ['C:\\Users\\pps\\Desktop\\TIL\\Study_API\\Study_FastAPI']
INFO:     Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit)
INFO:     Started reloader process [25412] using WatchFiles
INFO:     Started server process [21840]
INFO:     Waiting for application startup.
INFO:     Application startup complete.
INFO:     127.0.0.1:11839 - "GET / HTTP/1.1" 200 OK
```

![](https://imgur.com/FLBXmDB.jpg)

>[!info] 참고
>`uvicorn main:app` 명령은 다음을 의미한다.  
>- `main`: 파일 `main.py` (파이썬 "모듈).  
>-  `app`: `main.py` 내부의 `app = FastAPI()` 줄에서 생성한 object
>- `--reload`: 코드 변경 시 자동으로 서버 재시작. 개발 시에만 사용

- `INFO:     Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit)`
	- 해당 줄은 로컬에서 앱이 서비스되는 URL을 보여준다.

### 확인하기

브라우저로 [http://127.0.0.1:8000](http://127.0.0.1:8000) 를 열면 위의 사진처럼 JSON 응답으로 나온다.

### 대화형 API 문서

[http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs)를 열면 자동 대화형 API 문서를 볼 수 있다.

![](https://imgur.com/r2tXbox.jpg)

### 대안 API문서

[http://127.0.0.1:8000/redoc](http://127.0.0.1:8000/redoc)를 열면 대안 자동 문서를 볼 수 있다.

![](https://imgur.com/mmRgIXC.jpg)


### Open API

**FastAPI**는 API를 정의하기 위한 **OpenAPI** 표준을 사용하여 사용자의 모든 API를 이용해 "스키마(Schema)"를 생성한다.

#### 스키마(Schema)

- 무언가의 정의 또는 설명이다.
- 추상적인 설명이다.

#### API Schema

- [OpenAPI](https://github.com/OAI/OpenAPI-Specification)([[과제 공부용#Open API? OpenAPI?|RESTful API]])는 API Schema를 어떻게 정의하는지 지시하는 규격이다.
	- API 경로, 가능한 매개변수등을 포함한다.

#### Data Schema

- 스키마라는 용어는 JSON처럼 어떤 데이터의 형태를 나타낼 수 있다.
	- JSON 속성, 가지고 있는 데이터 타입등을 뜻한다.

#### OpenAPI와 JSON Schema

- OpenAPI는 사용자의 API에 대한 API Schema를 정의한다.
- 이 Schema는 JSON Data Schema의 표준인 **JSON Schema**를 사용하여 사용자의 API가 보내고 받는 데이터의 정의(Schema)를 포함한다.

##### `openapi.json` 확인

- FastAPI는 <U>자동으로 API의 설명과 함께 JSON(Schema)를 생성</U>한다.
- 가공되지 않은 OpenAPI Schema는 [http://127.0.0.1:8000/openapi.json](http://127.0.0.1:8000/openapi.json)
![](https://imgur.com/3m86PiV.jpg)

##### OpenAPI의 용도

- OpenAPI Schema는 포함된 두 개의 대화형 문서 시스템을 제공한다.
- **FastAPI**로 빌드한 애플리케이션에 이러한 대안을 쉽게 추가할 수 있다.
- API와 통신하는 클라이언트(프론트엔드, 모바일, IoT 애플리케이션 등)를 위해 코드를 자동으로 생성하는 데에도 사용할 수 있다.

### 단계별 요약

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
async def root():
    return {"message": "Hello World"}
```

1. `FastAPI` import
2. `app` 인스턴스 생성
3. 경로 작동 생성
	- 경로
		- 경로는 첫 번째 `/`부터 시작하는 URL의 뒷부분을 의미한다.  
		- `https://example.com/items/foo`의 경로는 `/items/foo`이다.  
		- 경로는 일반적으로 "Endpoint" 또는 "Route"라고 불린다.  
		- API를 설계할 때 "경로"는 "관심사"와 "리소스"를 분리하기 위한 주요 방법이다.  
	- 작동
		- 작동(Operation)은 HTTP 메소드 중 하나를 나타낸다.
			- `POST`: 데이터를 생성하기 위함
			- `GET`: 데이터를 읽기 위함
			- `PUT`: 데이터를 수정하기 위함
			- `DELETE`: 데이터를 삭제하기 위함
		- 자주 사용안되는 HTTP 메소드
			- `OPTIONS`
			- `HEAD`
			- `PATCH`
			- `TRACE`
		- HTTP 프로토콜에서는 이러한 메소드를 하나(또는 이상) 사용하여 각 경로와 통신할 수 있다.
	- `@app.get("/")`은 **FastAPI**에게 바로 아래에 있는 함수가 다음으로 이동하는 요청을 처리한다는 것을 알려준다.
		- 경로 `/`
		- `get` 작동 사용
	- `@decorater`: `decorater`는 아래에 있는 함수를 받아 그것으로 무언가를 한다. (함수 맨 위에 적는다.)
		- FastAPI에게 아래 함수가 경로 `/`의 `get`작동에 해당한다고알려준다.
	- 다른 작동으로는
		- `@app.post()`
		- `@app.put()`
		- `@app.delete()`
4. 경로 작동 함수 정의
	- 경로: `/`
	- 작동: `/`
	- 함수: `decorater` 아래에 있는 함수(`@app.get("/")`아래)
	- URL "`/`"에 대한 `GET` 작동을 사용하는 요청을 받을 때마다 FastAPI에 의해 호출된다.
	- `async def`를 대신에 `def`와 같은 일반 함수로 정의 가능하다.
5. 콘텐츠 반환
	- `dict`, `list`, 단일 값을 가진 `str`,`int`등을 반환할 수 있다.
	- Pydantic 모델을 반환할 수 있다.
	- JSON으로 자동 변환되는 객체들과 모델들이 많이 있다.

## 경로 매개변수

>[!abstract] 요약
>**FastAPI**를 이용하면 짧고 직관적인 표준 파이썬 타입 선언을 사용하여 다음을 얻을 수 있다.  
>- 편집기 지원: 오류 검사, 자동완성 등  
>- 데이터 **파싱**  
>- 데이터 검증  
>- API 주석(Annotation)과 자동 문서  
>단 한번의 선언만으로 위 사항들을 모두 선언할 수 있다.

파이썬의 포맷 문자열에서 사용되는 문법을 이용해 경로 `매개변수` 또는 `변수`를 선언할 수 있다.

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/items/{item_id}")
async def read_item(item_id):
    return {"item_id": item_id}
```

- 경로 매개변수 `item_id`의 값은 함수의 `item_id`인자로 전달된다.

- 실행 후 [http://127.0.0.1:8000/items/foo](http://127.0.0.1:8000/items/foo)를 열면
```python
{"item_id":"foo"}
```

![](https://imgur.com/VPalQAe.jpg)

### 타입이 있는 매개변수

파이썬 표준 Type Annotation을 사용하여 함수에 있는 경로 매개변수의 타입을 선언할 수 있다.

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/items/{item_id}")
async def read_item(item_id: int):
    return {"item_id": item_id}
```

- `item_id`는 `int`로 선언되었다.

### 데이터 변환

위의 실습예제를 실행하고 [http://127.0.0.1:8000/items/3](http://127.0.0.1:8000/items/3)을 열면
```python
{"item_id":3}
```

- 만약 `http://127.0.0.1:8000/items/`의 `item_id`자리에 `3`대신 다른 `int`형이 들어가면 바뀐 `int`형으로 나온다.
- 함수가 받은(반환도 하는) 값은 문자열 `"3"`이 아니라 파이썬 `int`형인 `3`이다.
	- 타입 선언을 하면 <U>FastAPI는 자동으로 요청을 파싱한다.</U>

### 데이터 검증

`http://127.0.0.1:8000/items/3`대신 [http://127.0.0.1:8000/items/foo](http://127.0.0.1:8000/items/foo)를 열면 HTTP오류가 뜬다.
```python
{
    "detail": [
        {
            "loc": [
                "path",
                "item_id"
            ],
            "msg": "value is not a valid integer",
            "type": "type_error.integer"
        }
    ]
}
```

- 경로 매개변수 `item_id`는 `int`가 아닌 `"foo"`자료형인 `str`로 받았기 때문이다.
- `int`가 아닌 `float`도 HTTP오류가 발생한다.

>[!info]
>- <U>파이썬 타입 선언을 하면 FastAPI는 데이터 검증을 한다.</U>  
>- 오류에는 정확히 어느 지점에서 검증을 통과하지 못했는지 명시된다.  
>- 이는 API와 상호 작용하는 코드를 개발하고 디버깅하는 데 매우 유용하다.  

### 문서화

[http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs)를 열면 자동 대화식 API 문서를 볼 수 있다.
![](https://imgur.com/okML1si.jpg)

- 파이썬 타입 선언을 하기만 하면 FastAPI는 자동 대화형 API 문서(Swagger UI)를 제공한다.
- 경로 매개변수가 정수형으로 명시된 것을 확인할 수 있다.

### 순서 문제

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/users/me")
async def read_user_me():
    return {"user_id": "the current user"}


@app.get("/users/{user_id}")
async def read_user(user_id: str):
    return {"user_id": user_id}
```

- `/users/me`: 현재 사용자의 데이터를 가져온다.
- `/users/{user_id}`: 사용자 ID를 이용해 특정 사용자의 정보를 가져온다.

- 경로 작동은 순차적으로 실행된다.
- `/users/{user_id}`는 `/users/me`요청 또한 매개변수 `user_id`의 값이 `me`인 것으로 생각할 수 있기 때문에 `/users/{user_id}`이전에 `/users/me`가 먼저 선언되어야한다.\


### 사전정의 값

경로 매개변수를 받는 경로 작동이 있지만, 경로 매개변수로 가능한 값들을 미리 정의하고 싶다면 파이썬 표준 `Enum`을 사용할 수 있다.

#### `Enum` 클래스 생성

```python
from enum import Enum
from fastapi import FastAPI

class ModelName(str, Enum):
    alexnet = "alexnet"
    resnet = "resnet"
    lenet = "lenet"

app = FastAPI()

@app.get("/models/{model_name}")
async def get_model(model_name: ModelName):
    if model_name is ModelName.alexnet:
        return {"model_name": model_name, "message": "Deep Learning FTW!"}

    if model_name.value == "lenet":
        return {"model_name": model_name, "message": "LeCNN all the images"}

    return {"model_name": model_name, "message": "Have some residuals"}
```

- `Enum`을 import하고 `str`과 `Enum`을 상속하는 서브 클래스를 만든다.
	- `str`을 상속함으로써 API문서는 값이 `string`형이어야 하는것을 알게 되고 이는 문서에 제대로 표시가 된다.
- 고정된 값의 클래스 속성을 만든다.

### 경로 매개변수 선언

생성한 열거형 클래스(`ModelName`)를 사용하는 Type Annotation으로 경로 매개변수를 만든다.

#### 문서 확인

경로 매개변수에 사용할 수 있는 값은 미리 정의되어 있으므로 대화형 문서에서 잘 표시 된다.

![](https://imgur.com/84jaAnX.jpg)

#### 파이썬 열거형(Enumeration)으로 작업하기

경로 매개변수의 값은 열거형 멤버가 된다.

- 열거형 `ModelName`의 열거형 멤버를 비교할 수 있다.
	- `if model_name is ModelName.alexnet:`
- 열거형 값 가져오기
	- `if model_name.value == "lenet":`
		- `model_name.value` 또는 일반적으로 `your_enum_member.value`를 이용하여 실제 값(위의 경우 `str`)을 가져올 수 있다.
		- `ModelName.lenet.value`로도 값`lenet`에 접근할 수 있다.
- 열거형 멤버 반환
	- `return {"model_name": model_name, "message": "Deep Learning FTW!"}`
	- `return {"model_name": model_name, "message": "LeCNN all the images"}`
	- `return {"model_name": model_name, "message": "Have some residuals"}`
		- 경로 작동에서 열거형 멤버를 반환할 수 있다. 중첩 JSON 본문(예시:`dict`)내의 값으로도 가능하다.
		- 클라이언트에 반환하기 전에 해당 값으로 변환된다.
		- 클라이언트는 JSON응답을 얻는다.
```python
{
  "model_name": "alexnet",
  "message": "Deep Learning FTW!"
}
```

### 경로를 포함하는 경로 매개변수

- 경로를 폼하는 경로 작동 `/files/{file_path}`이 있다고 가정한다.
	- `file_path`자체가 `home/johndoe/myfile.txt` 와 같은 경로를 포함해야한다.
	- 해당 파일의 URL은 `/file/home/johndoe/myfile.txt`가 된다.

#### OpenAPI 지원

테스트와 정의가 어려운 시나리오로 이어질 수 있으므로 OpenAPI는 경로를 포함하는 <U>경로 매개변수를 내부에 선언하는 방법을 지원하지 않는다.</U>

하지만 Starlette의 내부 도구중 하나를 사용하여 FastAPI에서는 가능하다.

문서에 매개변수에 경로가 포함되어야 한다는 정보가 명시되지는 않지만 여전히 작동한다.

#### 경로 변환기

- Starlette의 옵션을 직접 이용하여 아래와 같은 URL을 사용함으로써 path를 포함하는 경로 매개변수를 선언할 수 있다.
	- `/files/{file_path:path}`
	- 매개변수의 이름은 `file_path`가 되며, 마지막 부분 `:path`는 매개변수가 경로와 일치해야함을 명시한다.
```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/files/{file_path:path}")
async def read_file(file_path: str):
    return {"file_path": file_path}
```

- 매개변수가 가져야 하는 값이 `/home/johndoe/myfile.txt`와 같이 슬래시(`/`)로 시작해야 할 수 있다.
	- 이 경우 URL은 `/files//home/johndoe/myfile.txt`이다
	- `files`와 `home` 사이에 이중 슬래시(`//`)가 생긴다.


## 쿼리 매개변수

경로 매개변수의 일부가 아닌 다른 함수 매개변수를 선언하면 쿼리 매개변수로 자동해석한다.\

```Python
from fastapi import FastAPI

app = FastAPI()

fake_items_db = [{"item_name": "Foo"}, {"item_name": "Bar"}, {"item_name": "Baz"}]

@app.get("/items/")
async def read_item(skip: int = 0, limit: int = 10):
    return fake_items_db[skip : skip + limit]
```

**쿼리**는 URL에서 `?`후에 나오고 `&`으로 구분되는 키-값 쌍의 집합이다.

- 예시
	- `http://127.0.0.1:8000/items/?skip=0&limit=10`
	- 쿼리 매개변수
		- `skip`: 값 `0`을 가진다.
		- `limit`: 값 `10`을 가진다.

URL의 일부이기에 문자열이다.

파이썬 타입과 함께 선언할 경우 (예시와 같은 `int`), 해당 타입으로 변환 및 검증된다.

경로 매개변수에 적용된 동일한 프로세스가 쿼리 매개변수에도 적용된다.

- 편집기 지원
- 데이터 파싱
- 데이터 검증
- 자동 문서화

### 기본값

쿼리 매개변수는 경로에서 고정된 부분이 아니기 때문에 선택적일 수 있고 기본값을 가질 수 있다.
위의 예시와 같이 `skip=0`과 `limit=10`은 기본값을 가지고 있다.

URL로 이동하는 것은
- `http://127.0.0.1:8000/items/`
- `http://127.0.0.1:8000/items/?skip=0&limit=10`
	- 전부 같은 곳으로 이동하는 것과 같다.

![|1150](https://imgur.com/n06samg.jpg)

- `http://127.0.0.1:8000/items/?skip=20`
	- 함수의 매개변수 값
		- `skip=20`: URL에서 지정했다
		- `limit=10`: 기본값이다.


### 선택적 매개변수

같은 방법으로 기본값을 `None`으로 설정하여 선택적 매개변수를 선언할 수 있다.

```python
from typing import Union
from fastapi import FastAPI

app = FastAPI()

@app.get("/items/{item_id}")
async def read_item(item_id: str, q: Union[str, None] = None):
    if q:
        return {"item_id": item_id, "q": q}
    return {"item_id": item_id}
```

함수 매개변수 `q`는 선택적이고 기본값은 `None`값이 된다.
- FastAPI는 `item_id`가 경로 매개변수이고 `q`는 경로 매개변수가 아닌 쿼리 매개변수라는 것을 인지하고 있다.
- FastAPI는 `q`가 `=None`이므로 선택적이라는 것을 인지한다.
- `Union[str, None]`에 있는 `Union`은 FastAPI(FastAPI는 `str`부분만 사용)가 사용하는게 아니지만, `Union[str,None]`은 편집기에게 코드에서 오류를 찾아낼 수 있게 도와준다.


### 쿼리 매개변수 형변환

`bool`형으로 선언할 수도 있고 변환도 가능하다.
```python
from typing import Union
from fastapi import FastAPI

app = FastAPI()

@app.get("/items/{item_id}")
async def read_item(item_id: str, q: Union[str, None] = None, short: bool = False):
    item = {"item_id": item_id}
    if q:
        item.update({"q": q})
    if not short:
        item.update(
            {"description": "This is an amazing item that has a long description"}
        )
    return item
```

- 모두 같은 URL이다.
	- `http://127.0.0.1:8000/items/foo?short=1`
	- `http://127.0.0.1:8000/items/foo?short=True`
	- `http://127.0.0.1:8000/items/foo?short=on`
	- `http://127.0.0.1:8000/items/foo?short=yes`
- 다른 어떤 변형(대문자, 첫글자만 대문자등)이더라도 함수는 매개변수 `bool`형을 가진 `short`의 값이 `True`임을 안다. 그렇지 않은 경우 `False`이다.

![](https://imgur.com/SBOkRsd.jpg)


### 여러 경로/쿼리 매개변수

여러 경로 매개변수와 쿼리 매개변수를 동시에 선언할 수 있으며, FastAPI는 어느 것이 무엇인지 알고 있다. 그리고 특정 순서로 선언할 필요가 없다.

매개변수들은 이름으로 감지가 된다.
- `@app.get("/users/{user_id}/items/{item_id}")`
- `user_id: int, item_id: str, q: Union[str, None] = None, short: bool = False`
```python
from typing import Union
from fastapi import FastAPI

app = FastAPI()


@app.get("/users/{user_id}/items/{item_id}")
async def read_user_item(
    user_id: int, item_id: str, q: Union[str, None] = None, short: bool = False
):
    item = {"item_id": item_id, "owner_id": user_id}
    if q:
        item.update({"q": q})
    if not short:
        item.update(
            {"description": "This is an amazing item that has a long description"}
        )
    return item
```


### 필수 쿼리 매개변수

경로가 아닌 매개변수에 대한 기본값을 선언할 때, 해당 매개변수는 필수적(Required)이지 않다.
특정값을 추가하지 않고 선택적으로 만들기 위해선 기본값을 `None`으로 설정하면 된다.

쿼리 매개변수를 필수로 만들려면 단순히 기본값을 선언하지 않으면 된다.

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/items/{item_id}")
async def read_user_item(item_id: str, needy: str):
    item = {"item_id": item_id, "needy": needy}
    return item
```

```python
@app.get("/items/{item_id}")
async def read_user_item(item_id: str, needy: str):
```

여기 쿼리 매개변수 `needy`는 `str`형인 필수 쿼리 매개변수이다.

브라우저에서 아래의 URL을 열면
- `http://127.0.0.1:8000/items/foo-item`
필수 매개변수 `needy`를 넣지 않아 오류가 발생한다.

![](https://imgur.com/Kn3qoU1.jpg)

`needy`는 <U>필수 매개변수이므로 URL에 반드시 설정해줘야한다.</U>
- `http://127.0.0.1:8000/items/foo-item?needy=sooooneedy`

![](https://imgur.com/uFkC82O.jpg)

일부 매개변수는 필수로, 다른 일부는 기본값을, 또 다른 일부는 선택적으로 선언할 수 있다.

```python
from typing import Union
from fastapi import FastAPI

app = FastAPI()

@app.get("/items/{item_id}")
async def read_user_item(
    item_id: str, needy: str, skip: int = 0, limit: Union[int, None] = None
):
    item = {"item_id": item_id, "needy": needy, "skip": skip, "limit": limit}
    return item
```

위 예시에서 3가지 쿼리 매개변수가 있다.
- `needy`: 필수적인 `str`
- `skip`: 기본값이 `0`인 `int`
- `limit`: 선택적인 `int`


## Postman을 통해 통신 확인하기

위의 예시를 이용하여 Postman으로 통신이 잘되는지 확인해본다.

1. 먼저, `http://127.0.0.1:8000/items/foo-item?needy=plot&skip=10&limit=100`을 넣어 쿼리 값을 넣어준다.
	- URL옆에 `GET`이 있는걸 볼 수 있다. 이를 통해 데이터를 요청한다라는 뜻이 된다.
![](https://imgur.com/eZ7mx0p.jpg)

- 또는 난 아래의 이미지처럼 value에 값을 넣어 설정하였다.
![](https://imgur.com/5Kso0aw.jpg)

2. **Send**키를 누르게 되면 URL에 있는 내용이 출력이 된다.
![](https://imgur.com/NFoYhfI.jpg)


## 요청 본문

클라이언트로부터 사용자의 API로 데이터를 보내야 할 때, **요청 본문**으로 보낸다.

**요청 본문**은 클라이언트에서 API로 보내지는 데이터이다.
**응답 본문**은 API가 클라이언트로 보내는 데이터이다.

사용자의 API는 대부분의 경우 **응답 본문**을 보내야 한다. 하지만 클라이언트는 **요청 본문**을 매번 보낼 필요가 없다.

- 데이터를 보내기 위해 `POST`,`PUT`,`DELETE` 혹은 `PATCH`중에 하나를 사용하는 것이 좋다.
- `GET` 요청에 본문을 담아 보내는 것은 명세서에 정의되지 않은 행동이다.
- `GET` 요청에 본문을 담는 것은 권장되지 않기에, `Swagger UI`같은 대화형 문서에서는 `GET` 사용시 담기는 본문에 대한 문서를 표시하지 않으며, 중간에 있는 프록시는 이를 지원하지 않을 수 있다.


### Pydantic의 `BaseModel` Import

```python
from fastapi import FastAPI
from pydantic import BaseModel

class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None

app = FastAPI()

@app.post("/items/")
async def create_item(item: Item):
    return item
```


### 사용자의 데이터 모델 만들기

`BaseModel`를 상속받은 클래스로 사용자의 데이터 모델을 선언한다.
모든 속성에 대해 표준 파이썬 타입을 사용한다.

```python
from fastapi import FastAPI
from pydantic import BaseModel

class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None

app = FastAPI()

@app.post("/items/")
async def create_item(item: Item):
    return item
```

쿼리 매개변수를 선언할 때와 같이, 모델 속성이 기본 값을 가지고 있어도 이는 필수가 아니지만 그외에는 필수다. `None`을 사용하여 선택적으로 만들 수 있다.

- 예시
	- 위의 모델은 JSON "`object`"(혹은 파이썬 `dict`)을 선언한다.
```python
{
    "name": "Foo",
    "description": "선택적인 설명란",
    "price": 45.2,
    "tax": 3.5
}
```

- `description`과 `tax`는 (기본 값이 `None`으로 되어 있어) 선택적이기 때문에, 이와는 다르게 쓰여도 유효하다.
```python
{
    "name": "Foo",
    "price": 45.2
}
```


### 매개변수로서 선언하기

사용자의 경로 작동에 추가하기 위해, 경로 매개변수 그리고 쿼리 매개변수에서 선언했던 것과 같은 방식으로 선언하면 된다.

```python
from fastapi import FastAPI
from pydantic import BaseModel

class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None

app = FastAPI()

@app.post("/items/")
async def create_item(item: Item):
    return item
```


#### 결과

위의 파이썬 타입 선언으로, FastAPI는 다음과 같이 동작한다.

- 요청의 본문을 JSON으로 읽어들인다.
- (필요하다면) 대응되는 타입으로 변환한다.
- 데이터를 검증한다.
	- 데이터가 유효하지 않다면, 정확히 어떤 것이 그리고 어디에서 데이터가 잘못되었는지 지시하는 명료한 에러를 반환할 것이다.
- 매개변수 `item`에 포함된 수신 데이터를 제공한다.
	- 함수 내에서 매개변수를 `Item`타입으로 선언했기 때문에, 모든 속성과 그에 대한 타입에 대한 편집기 지원(완성 등)을 받을 수 있다.
- 사용자의 모델을 위한 JSON Schema정의를 생성한다. 사용자의 프로젝트에 적합하다면 사용하고 싶은 곳 어디에서나 사용이 가능하다.
- 이러한 Schema는, 생성된 OpenAPI Schema 일부가 될 것이며, 자동 문서화 UI에 사용된다.


### 자동 문서화

모델의 JSON 스키마는 생성된 OpenAPI 스키마에 포함되며 대화형 API 문서에 표시된다.
![](https://imgur.com/ZV5lAUF.jpg)

이를 필요로 하는 각각의 경로 작동내부의 API 문서에도 사용된다.
![](https://imgur.com/ewQSxh4.jpg)


### 모델 사용하기

함수 안에서 모델 객체의 모든 속성에 직접 접근이 가능하다.
- `price_with_tax = item.price + item.tax`

```python
from fastapi import FastAPI
from pydantic import BaseModel

class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None

app = FastAPI()

@app.post("/items/")
async def create_item(item: Item):
    item_dict = item.dict()
    if item.tax:
        price_with_tax = item.price + item.tax
        item_dict.update({"price_with_tax": price_with_tax})
    return item_dict
```


### 요청 본문 + 경로 매개변수

경로 매개변수와 요청 본문을 <U>동시에 선언</U>할 수 있다.

**FastAPI**는 경로 매개변수와 일치하는 함수 매개변수가 **경로에서 가져와야 한다**는 것을 인지하며,
Pydantic 모델로 선언된 그 함수 매개변수는 **요청 본문에서 가져와야 한다**는 것을 인지할 것이다.
```python
@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Item):
```

```python
from fastapi import FastAPI
from pydantic import BaseModel

class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None

app = FastAPI()

@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Item):
    return {"item_id": item_id, **item.dict()}
```


### 요청 본문 + 경로 + 쿼리 매개변수

**본문**, **경로** 그리고 **쿼리 매개변수** 모두 동시에 선언할 수 있다.

**FastAPI**는 각각을 인지하고 데이터를 올바른 위치에 가져올 것이다.
- `async def update_item(item_id: int, item: Item, q: str | None = None):`
```python
from fastapi import FastAPI
from pydantic import BaseModel

class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None

app = FastAPI()

@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Item, q: str | None = None):
    result = {"item_id": item_id, **item.dict()}
    if q:
        result.update({"q": q})
    return result
```

함수 매개변수는 다음을 따라서 인지하게 된다.
- 만약 매개변수가 **경로**에도 선언되어 있다면, 이는 경로 매개변수로 사용될 것이다.
- 만약 매개변수가 (`int`,`float`,`str`,`bool` 등과 같은) **유일한 타입**으로 되어 있으면, **쿼리 매개변수**로 해석될 수 있다.
- 만약 매개변수가 **Pydantic 모델** 타입으로 선언되어 있으면, **요청본문**으로 해석될 것이다.

FastAPI는 `q`의 값이 필요없음을 알게 될 것이다. 기본값이 `= None`이기 때문이다.

- `Union[str, None]`에 있는 `Union`은 FastAPI에 의해 사용된 것이 아니지만, 편집기로 더 나은 지원과 에러 탐지를 지원할 것이다.

---

>[!reference]
>[FastAPI 공식 문서](https://fastapi.tiangolo.com/ko/tutorial/)
>[Wikidocs](https://wikidocs.net/book/8531) (참고하지는 않았지만 좋은 참고자료)