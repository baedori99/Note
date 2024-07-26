---
title: Stack
create_date: 2024-07-26 10:07 - 2024-07-26 10:07
draft: false
---
## 스택(Stack)이란?

<p align="center">
	<img src="https://imgur.com/5ymyDOf.png" height="400px" width="300px">
	</p>

스택은 "쌓다"라는 의미로, **데이터를 차곡차곡 쌓아 올린 형태의 자료구조**이다.

**후입선출**/ **LIFO(Last In First Out)** 의 자료 구조 
즉 <U>데이터가 순서대로 쌓이며 </U>**<U>가장 마지막에 삽입된 자료가 가장 먼저 삭제되는 구조</U>** 를 가지고 있다.

## 스택의 작동원리

<p align="center">
	<img src="https://imgur.com/8nEUkx7.png" height="500px" width="300px">
	</p>

스택은 후입선출 구조로 이루어져 있다.  

위의 그림을 보면 가장 먼저 들어온 `a`가 가장 마지막에 쌓여있고 가장 나중에 들어온 `c`가 가장 위에 놓여져 있다. 데이터를 넣고 빼는 입구가 하나뿐인 스택은 가장 위에 쌓인 데이터를 먼저 꺼내도록 작동한다.

## Stack 구조

스택은 아래 그림과 같은 구조로 이루어져 있다. 

![|500](https://imgur.com/Kv21lgZ.png)

- **Bottom**: 가장 밑에 있는 데이터 또는 Index
- **Top**: 가장 위에 있는 데이터 또는 Index
- **Capacity**: 스택에 담을 수 있는 데이터의 총 용량
- **Size**: 현재 스택에 담겨져있는 데이터의 개수

## Stack 기본 연산

- `pop()`: 스택에서 가장 위에 있는 원소를 삭제하고 그 원소를 반환한다.
- `append(item)`: `item` 하나를 스택의 가장 윗 부분에 추가한다.
- `peek()`: 스택의 가장 위에 있는 원소를 반환한다. (삭제하지는 않는다.)
- `empty()`: 스택이 비어있따면 1, 아니면 0을 반환한다.

## Stack 구현

파이썬에서는 스택을 `List` 또는 `Linked List`로 구현한다.

- `S.append()`: 리스트 맨 끝에 원소를 추가한다. 

- `S.pop(i)`: 리스트의 i번째 원소를 삭제한다.
	- `S.pop(-1)`: 맨 앞에 있는 원소를 삭제한다.


>[!Note]- Stack Implement Code
>```python
># Stack 구현 코드 (Python)
>class Stack:
>    # 리스트를 이용한 스택 구현
>    def __init__(self):
>        self.top = []
>    # 스택 크기 반환
>    def __len__(self) -> bool:
>        return len(self.top)
>
>    # 구현 함수
>    # 스택에 원소 삽입
>    def push(self, item):
>        self.top.append(item)
>
>    # 스택 가장 위에 있는 원소를 삭제하고 반환
>    def pop(self):
>        if not self.isEmpty():
>            return self.top.pop(-1)
>        else:
>            print("Stack underflow")
>            exit()
>
>    # 스택 가장 위에 있는 원소를 반환
>    def peek(self):
>        if not self.isEmpty():
>            return self.top[-1]
>        else:
>            print("underflow")
>            exit()
>
>    # 스택이 비어 있는지를 bool값으로 반환
>    def isEmpty(self) -> bool:
>        return len(self.top)==0
>```

>[!Note]- Stack Extended Implement Code
>```python
># Stack 구현 코드 (Python)
>class Stack:
>
>    # 리스트를 이용한 스택 구현
>    def __init__(self):
>        self.top = []
>        self.limit = limit
>
>    # 스택 크기 반환
>    def __len__(self) -> bool:
>        return len(self.top)
>
>    # 구현 함수
>    # 스택에 원소 삽입
>    def push(self, item):
>        if (len(self.pop)>= self.limit):
>            self.top.append(item)
>
>    # 스택 가장 위에 있는 원소를 삭제하고 반환
>    def pop(self):
>        if not self.isEmpty():
>            return self.top.pop(-1)
>        else:
>            print("Stack underflow")
>            exit()
>
>    # 스택 가장 위에 있는 원소를 반환
>    def peek(self):
>        if not self.isEmpty():
>            return self.top[-1]
>        else:
>            print("underflow")
>            exit()
>
>    # 스택이 비어 있는지를 bool값으로 반환
>    def isEmpty(self) -> bool:
>        return len(self.top)==0
>
>    # 스택을 비움
>    def clear(self):
>        self.top = []
>
>    # 스택 안에 특정 item이 포함되어 있는지를 bool값으로 반환
>    def isContain(self, item)-> bool:
>        return item in self.top
>
>    # 스택이 가득 차 있는 지를 bool값으로 반환
>    def isFull(self) -> bool:
>        return self.size() == self.limit
>
>    # 스택의 크기를 int값으로 반환
>    def size(self) -> int:
>        return len(self.top)
>```

##  Stack 활용

- 재귀 알고리즘
- DFS(Depth First Search) 알고리즘
- 웹 브라우저 뒤로가기
- 문자열 역순 출력
- 후위 표기법 계산
- 실행 취소(undo)

## Time Complexity

Stack의 삽입이나 삭제시 맨 위의 데이터를 삭제하기 때문에 늘 `O(1)`이다.

하지만 특정 데이터를 찾을 때는 데이터를 찾을 때까지 순차적으로 수행해야 하므로 `O(n)` 

>[!reference]
>[스택(Stack)](https://velog.io/@alkwen0996/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%EC%8A%A4%ED%83%9DStack)  
>[스택(Stack)과 큐(Queue)에 대해서 알아보자!](https://jud00.tistory.com/entry/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%EC%8A%A4%ED%83%9DStack%EA%B3%BC-%ED%81%90Queue%EC%97%90-%EB%8C%80%ED%95%B4%EC%84%9C-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90)  
>[스택(Stack), 큐(Queue)](https://c4u-rdav.tistory.com/14)  
>[스택(Stack)](https://velog.io/@yeseolee/Python-%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%EC%8A%A4%ED%83%9DStack#-stack-%EA%B5%AC%ED%98%84)  

