---
title: Fast API
create_date: 2024-07-10 11:07 - 2024-07-10 11:07
draft: true
---
이 노트는 [Fast API 공식 문서](https://fastapi.tiangolo.com/ko/tutorial/)를 보고 실습한 내용에 대해서 적는 것이다.
[[네트워크 관련 공부#GET과 POST|GET과 POST]]에 대해서 개념이라도 알고 있어야 한다.

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