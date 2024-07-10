---
title: Pydantic
create_date: 2024-07-09 16:07 - 2024-07-09 16:07
draft: false
---
# Pydantic 라이브러리

Pydantic은 데이터 유효성 검사와 설정 관리를 하는 라이브러리이다.
Pydantic은 Python Type Hint를 기반으로 데이터 검증 및 구문 분석을 수행하여, 데이터 모델링과 검증을 쉽게 하고, 오류 발생시 유용한 디버깅 정보를 제공한다.

>[!info] Pydantic Document Comments
>Data validation and settings management using Python type annotations.
>_pydantic_ enforces type hints at runtime, and provides user friendly errors when data is invalid.
>Define how data should be in pure, canonical Python; validate it with _pydantic_.
>[Pydantic Document](https://docs.pydantic.dev/1.10/)

Pydantic은 validation 라이브러리가 아닌 parsing 라이브러리이다.
<U>즉, pydantic은 입력 데이터가 아닌 출력 모델의 유형과 제약 조건을 보장한다.</U>

---

## 데이터 유효성 검사(Data Validation)

데이터가 각 속성에 대해 정의한 일련의 규칙, 스키마 또는 제약 조건을 준수하도록 하는 프로세스이다.
데이터 유효성 검사를 통과하면 코드가 예상했던 정확한 방식으로 데이터를 수집하고 반환한다.

데이터 유효성 검사는 잘못된 사용자 입력과 같은 문제로 인해 발생하는 예기치 않은 오류를 방지한다.

---
## Installation

```
pip install pydantic
```

---
## Models

Models는 `BaseModel`로부터 상속받은 Class로, parsing과 validation을 통해 정의된 필드와 제약을 보장해준다.

난 model중 `Nested Models`만 다루도록 하겠다.

### Nested Models

```python
# Concept: Models/Nested models
from typing import List, Optional
from pydantic import BaseModel

class Foo(BaseModel):
    count: int
    size: Optional[float] = None
    
class Bar(BaseModel):
    apple: str = 'x'
    banana: str = 'y'

class Spam(BaseModel):
    foo: Foo
    bars: List[Bar]

m = Spam(foo={'count':4}, bars=[{"apple":"x1"}, {"apple":"x2"}])
```

```python
print(m)
print(m.model_dump())
```

```python
# 출력값

# foo=Foo(count=4, size=None) bars=[Bar(apple='x1', banana='y'), Bar(apple='x2', banana='y')]

# {'foo': {'count': 4, 'size': None}, 'bars': [{'apple': 'x1', 'banana': 'y'}, {'apple': 'x2', 'banana': 'y'}]}
```

각 클래스의 Model Class에서 정의된 필드들을 정리하자면,

- Foo
	- `count`: `int`타입의 변수를 의미한다. 반드시 필요한 필드이다.
	- `size`: `float`타입의 변수를 의미한다. 반드시 필요하지 않은 필드이며, 설정하지 않으면 `None`으로 설정된다.
- Bar
	- `apple`: `str`타입의 변수를 의미한다. 반드시 필요한 필드이며, 설정하지 않으면 기본값으로 `x`가 설정된다.
	- `banana`: `str`타입의 변수를 의미한다. 반드시 필요한 필드이며, 설정하지 않으면 기본값으로 `y`가 설정된다.
- Spam
	- `foo`: `Foo` 클래스를 의미한다.
	- `bars`: `Bar` 클래스의 인스턴스 리스트를 의미한다.

---
## Fields

### Default values

```python
# Concepts: Fields/Default values
from pydantic import BaseModel, Field

class User(BaseModel):
    name: str = Field(default = "John Smith")

user = User()
```

```python
print(user)

# 출력값
# name='John Smith'
```


```python
from uuid import uuid4  # uuid란 네트워크상에 중복되지 않는 ID를 만들기 위한 표준 규약
from pydantic import BaseModel, Field

class User(BaseModel):
    id: str = Field(default_factory=lambda: uuid4().hex)

user = User()
```

```python
print(user)

# 출력값
# id='d1be7377eea34d869ca615a8324ea9d3'
```

---
### Field aliases

Pydantic에서는 필드의 별칭(alias)을 정의하여 모델의 유횽성 검사(validation)와 직렬화(serialization)시 다른 이름을 사용할 수 있다. 이를 통해 외부 입력 데이터와 내부 모델의 필드 이름이 다른 경우 유용하게 사용할 수 있다.

1. `alias`:
	- 유효성 검사와 직렬화 모두에서 동일한 alias를 사용한다.
	- 예를 들어, 외부 입력 데이터의 키 이름이 모델의 필드 이름과 다를 때 유용하다.
```python
# example of using alias parameter
from pydantic import BaseModel, Field

class User(BaseModel):
    name: str = Field(alias='username')

user = User(username="johnsmith")
```

```python
print(user)
print(user.model_dump(by_alias=True))

# 출력값
# name='johnsmith' 
# {'name': 'johnsmith'}
```

2. `validation_alias`:
	- 유효성 검사 시에만 alias를 사용한다.
	- 입력 데이터를 모델로 변환할 때 이 별칭을 사용한다.
```python
# example of validation_alias parameter
from pydantic import BaseModel, Field

class User(BaseModel):
    name: str = Field(validation_alias='username')

user = User(username='johnsmith')
```

```python
print(user)
print(user.model_dump(by_alias=True))

# 출력값
# name='johnsmith' 
# {'name': 'johnsmith'}
```

3. `serialization_alias`:
	- 직렬화 시에만 alias를 사용한다.
	- 모델 데이터를 외부로 내보낼 때 이 alias를 사용한다.
```python
# example of serialization_alias parameter
from pydantic import BaseModel, Field

class User(BaseModel):
    name: str = Field(serialization_alias='username')

user=User(name='johnsmith')
```

```python
print(user)
print(user.model_dump(by_alias=True))

# 출력값
# name='johnsmith' 
# {'username': 'johnsmith'}
```

---
## Constrained Type

Constrained Type을 통해 자신의 제한을 적용할 수 있다.

### Numeric Constraints

숫자로 제한을 적용한다.

숫자 값을 제한하는데 사용되는 keyword arguments

- `gt`: greater than
- `lt`: less than
- `ge`: greater than or equal to
- `le`: less than or equal to
- `multiple_of`: a multiple of the given number
- `allow_inf_nan`: allow `inf`,`-inf`,`nan` values

#### 예제
```python
# example
from pydantic import BaseModel, Field

class Foo(BaseModel):
    positive: int = Field(gt=0)
    non_negative: int = Field(ge=0)
    negative: int = Field(lt=0)
    non_positive: int = Field(le=0)
    even: int = Field(multiple_of=2)
    love_for_pydantic: float = Field(allow_inf_nan = True)

foo = Foo(
    positive=1,
    non_negative=0,
    negative=-1,
    non_positive= 0,
    even = 2,
    love_for_pydantic=float('inf'),
)
```

```python
print(foo)

# 출력값
# positive=1 non_negative=0 negative=-1 non_positive=0 even=2 love_for_pydantic=inf
```

### String Constraints

문자열 제한에 사용되는 fields

- `min_length`: Minimum length of the string
- `max_length`: Maximum length of the string
- `pattern`: A regular expression that the string must match

#### 예제
```python
# example
from pydantic import BaseModel, Field
  
class Foo(BaseModel):
    short: str = Field(min_length=3)
    long: str = Field(max_length=10)
    regex: str = Field(pattern=r'^\d*$')
foo = Foo(short='foo', long='foobarbaz', regex='123')
```

```python
print(foo)

# 출력값
# short='foo' long='foobarbaz' regex='123'
```

###  Decimal Constraints

소수 제한에 사용되는 fields

- `max_digits`: Maximum number of digits within the `Decimal`. It does not include a zero before the decimal poin or trailing decimal zeroes
- `decimal_places`: Maximum number of decimal places allowed. It does not include trailing decimal zeroes

#### 예제
```python
# example
from decimal import Decimal
from pydantic import BaseModel, Field

class Foo(BaseModel):
    precise: Decimal = Field(max_digits=5, decimal_places=2)
foo = Foo(precise=Decimal('123.45'))
```

```python
print(foo)

# 출력값
# precise=Decimal('123.45')
```


## Reference

>[!reference]
>[Pydantic Documentation](https://docs.pydantic.dev/latest/)
>[Python)pydantic 알아보기](https://data-newbie.tistory.com/836#reference)
>[pydantic 살펴보기](https://seoyeonhwng.medium.com/pydantic-%EC%82%B4%ED%8E%B4%EB%B3%B4%EA%B8%B0-27c67273f0be)
>[pydantic을 사용하여, 안정성 높이기](https://re-code-cord.tistory.com/entry/pydantic%EC%9D%84-%EC%9D%B4%EC%9A%A9%ED%95%9C-%EB%B0%B1%EC%95%A4%EB%93%9C-%EA%B0%9C%EB%B0%9C%EC%9E%90-%EA%B4%B4%EB%A1%AD%ED%9E%88%EA%B8%B0)