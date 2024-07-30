---
title: Queue
create_date: 2024-07-26 13:07 - 2024-07-26 13:07
draft: true
---
## 큐(Queue)란?

큐(Queue)는 [[Stack|스택(Stack)]]과 다르게 먼저 들어온 것이 먼저 나가는 구조이다.  

큐(Queue)의 사전적 의미는 줄, 혹은 줄을 서서 기다리는 것을 의미한다.  
줄의 첫 번째에 서 있는 사람이 먼저 서비스를 받고, 그 다음으로 줄을 선 사람이 서비스를 받는 구조를 말한다.  

양 쪽 끝에서만 데이터를 넣거나 뺄 수 있는 삽입과 삭제의 위치가 제한적인 형태의 **선입선출(FIFO)** 구조를 갖는 선형 자료구조이다.  

- **선입선출** / **FIFO(First In First Out)** 의 자료 구조

![](https://imgur.com/eWjVNlw.png)

## Queue 동작 원리

Queue의 동작원리는 아래 그림과 같다.  
![](https://imgur.com/EUwqnED.png)  


## Queue 특징

- Queue는 삽입과 삭제가 다른 방향으로 이루어진다.
	- 스택(Stack)의 저장구조와 같지만 스택은 top을 통해서만 삽입, 삭제 이루어졌다.  
- **스택**이 바닥에 막힌 세로로 세운 통이라면 **큐**는 양쪽이 뚫린 가로로 된 통의 형태이다.  
- **프론트(front)**: **삭제연산**만 수행되는 곳  
	- **Dequeue**:  프론트에서 이루어지는 삭제연산
	- `front` 원소는 <U>가장 먼저 큐에 들어온 첫번째 원소</U>  
- **리어(Rear)**: **삽입연산**만 이루어지는 곳  
	- **Enqueue**: 리어에서 이루어지는 삽입연산
	- `rear`원소는 <U>가장 늦게 들어온 마지막 원소</U>  

※ 꽉 차 있는 queue에 `push`하려는 경우: `오버플로우(Overflow)`  
※ 비어있는 queue에 `pop`하려는 경우: `언더플로우(Underflow)`  

## Queue 기본 연산

- `push(item)`: 큐 맨 뒤에 데이터 삽입
- `pop()`: 큐 맨 앞에 데이터 삭제 및 반환
- `front()`: 큐 맨 앞 데이터 반환
- `back()`: 큐 맨 뒤 데이터 반환
- `size()`: 큐 크기 반환
- `empty()`: 큐가 비었는지 여부 반환
- `full()`: 큐가 가득 찼는지 여부 반환

### Enqueue

항목을 큐의 끝(`rear` 또는 `back`)에 추가하는 연산이다.  

새로운 항목은 항상 큐의 끝에 추가되므로, 이전에 큐에 들어온 항목들은 뒤로 밀려난다.  이 연산이 수행된 후에는, 새롭게 추가된 항목이 큐의 맨 끝에 위치하게 된다.  

### Dequeue

큐의 시작(`front` 또는  `head`)에서 항목을 제거하고, 그 항목을 반환하는 연산이다.  

Dequeue 연산이 수행되면, 가장ㅁ너저 큐에 추가된 항목이 제거되고, 그 다음으로 추가된 항목이 큐의 시작 위치로 이동한다. 큐가 비어 있을 때 Dequeue 연산을 시도하면, 에러나 예외 상황을 반환하거나, 특정 값(예: `null`)을 반환하도록 정의할 수 있다.   

## Queue 구현

- 배열
	- 장점
		- 배열 내 요소의 주로를 나타내는 인덱스를 통해 원하는 데이터를 빠르게 검색 가능하다.
	- 단점
		- 선언할 때 크기가 고정되어 배열에 들어있는 데이터 양에 따라 배열의 크기좆어이 필요하다.  
		- 데이터를 중간에 삽입하거나 삭제할 경우 해당 데이터 뒤에 있는 요소들의 위치를 모두 이동시켜야하는 비효율적인 상황이 발생한다.

- 배열을 사용한 Queue 구현
>[!Note]- Implementing Queue using List(array)
>```python
># 배열을 이용한 Queue 구현
>class ListQueue(object):
>
>    def __init__(self):
>        self.queue = []
>
>    def dequeue(self):
>        if len(self.queue) == 0:
>            return -1
>        return self.queue.pop(0)
>
>    def enqueue(self, n):
>        self.queue.append(n)
>        pass
>
>    def printQueue(self):
>        print(self.queue)
>
>if __name__ == "__main__":
>    queue_list = ListQueue()
>
>    queue_list.enqueue(1)
>    queue_list.enqueue(2)
>    queue_list.enqueue(3)
>    queue_list.enqueue(4)
>    queue_list.enqueue(5)
>    
>    queue_list.printQueue()
>    print(queue_list.dequeue())
>    print(queue_list.dequeue())
>    print(queue_list.dequeue())
>    print(queue_list.dequeue())
>    print(queue_list.dequeue())
>
>    queue_list.printQueue()
>```

>[!example]- 실행 결과
>![](https://imgur.com/Vsz6KNf.png)

- 연결리스트(Linked List)
	- 장점
		- 데이터의 양에 상관없이 <U>크기가 동적으로 조절</U>된다.
		- Index 대신 이전 데이터 그리고 다음 데이터의 <U>위치를 기억하는 노드 형태를 이용</U>한다.  
		- 리스트 중간에 데이터 삽입, 삭제시 노드들 사이에 연결된 링크들을 끊어주거나 이어주면 되기 때문에 그 과정이 용이하다.  
	- 단점
		- 데이터에 접근할 때 연결되어 있는 노드들을 따라 양 끝에서부터 순차적으로 접근해야하기 때문에 <U>데이터 접근 속도가 배열에 비해 느리다.</U> 

- Linked List를 사용한 Queue 구현
>[!note]- Implementing Queue using Linked List
>```python
># Linked List를 사용한 Queue 구현
>class Node(object):
>
>    def __init__(self, data):
>        self.data = data
>        self.next = None
>
>class SingleLinkedList(object):
>
>    def __init__(self):
>        self.head = None
>        self.tail = None
>
>    def enqueue(self, node):
>        if self.head == None:
>            self.head = node
>            self.tail = node
>        else:
>            self.tail.next = nod
>            self.tail = self.tail.next
>
>    def dequeue(self):
>        if self.head == None:
>            return -1
>
>        v = self.head.data
>        self.head = self.head.next
>        if self.head == None:
>            self.tail = None
>        return v
>
>    # 출력
>    def print(self):
>        current = self.head
>        string = ""
>        while current:
>            string += str(current.data)
>            if current.next:
>                string += "->"
>            current = current.next
>        print(string)
>
>if __name__ == "__main__":
>
>    s = SingleLinkedList()
>
>    # 데이터 삽입
>    s.enqueue(Node(1))
>    s.enqueue(Node(2))
>    s.enqueue(Node(3))
>    s.enqueue(Node(4))
>    s.print()
>
>    # 데이터 제거
>    print(s.dequeue())
>    print(s.dequeue())
>    s.print()
>    print(s.dequeue())
>    print(s.dequeue())
>```

>[!example]- 실행 결과
>![](https://imgur.com/L3JfXmc.png)

📌 모든 원소의 값을 필요로 하거나 중간 데이터를 삽입/삭제할 경우  **Linked List**를 사용하는 것이 좋다.  
📌 특정 데이터에 접근하는 것이 목적이라면 **배열**을 사용하는 것이 좋다.   

- `dequeue` 라이브러리 사용한 Queue 구현
>[!Note]- Implementing Queue using dequeue library
>```python
># dequeue 라이브러리 사용한 Queue 구현
>from collections import deque
>
>dq = deque([])
>
>dq.append(1)
>dq.append(2)
>dq.append(3)
>dq.append(4)
>print(dq)
>
>print(dq.popleft())
>print(dq.popleft())
>print(dq.popleft())
>print(dq.popleft())
>print(dq)
>```

>[!example]- 실행 결과
>![](https://imgur.com/f65t2cP.png)

- queue 라이브러리 사용한 Queue 구현
>[!note]- Implementing Queue using queue library
>```python
># queue 라이브러리 사용하여 Queue 구현
>import queue
>
>data_queue = queue.Queue()
>
># 데이터 삽입
>data_queue.put("data")
>data_queue.put(7)
>
># 현재 큐에 데이터가 몇 개인지 확인
>data_queue.qsize()
>
># 데이터 제거
>data_queue.get()
>data_queue.qsize()
>```

## Queue의 종류

### 선형 큐(Linear Queue)

- 기본적인 Queue 형태
- Linear Queue는 배열, Linked List로 표현할 수 있다.  
- 문제점
	- 배열로 구현 시 크기 제한
	- 빈 공간을 사용하기 위해서 모든 데이터를 꺼내기 or 자료를 한 칸씩 옮겨야 한다.  
	- 연산량이 많은 `enqueue`와 `dequeue` 작업이 있는 경우 어느 시점에서 큐가 비어있어도 데이터를 추가하지 못하는 경우가 발생한다.  

### 원형 큐(Circular Queue)

- 원형으로 연결되어 있는 구조
- 1차원 배열 형태로 큐를 원형으로 구성하여 배열의 처음과 끝을 연결하여 만든다.  
- 삽입/삭제 연산에서, 변경되는 `front`와 `rear` 값이 가리키는 위치가 마지막 index에서 다시 처음 index로 돌아오게 되는 경우를 위해 나머지 연산자 $((front + 1) \bmod maxSize)$ 
![](https://imgur.com/Q2eykFn.png)

>[!note]- Implement Circular Queue
>```python
># Circular Queue
>class Queue:
>
>    maxlen = 10
>
>    def __init__(self):
>        self.array = [None] * self.maxlen
>        self.head = -1
>        self.tail = -1
>
>    def push(self, item):
>        if not self.full():
>            self.tail += 1
>            self.array[self.tail % self.maxlen] = item
>
>    def pop(self):
>        if not self.empty():
>            self.head += 1
>            return self.array[self.head % self.maxlen]
>
>    def front(self):
>        if not self.empty():
>            return self.array[(self.head + 1) % self.maxlen]
>
>    def back(self):
>        if not self.empty():
>            return self.array[self.tail % self.maxlen]
>
>    def size(self):
>        return self.tail - self.head
>
>    def empty(self):
>        return self.size() == 0
>
>    def full(self):
>        return self.size() == self.maxlen
>```

### 우선순위 큐(Priority Queue)

## Queue 활용

- 프로세스 관리
- 너비 우선 탐색(BFS, Breadth-First Search) 구현
	- 처리해야 할 노드의 리스트를 저장하는 용도로 큐를 사용한다.
	- 노드 하나를 처리할 때마다 해당 노드와 인접한 노드들을 큐에 다시 저장한다.  
	- 노드를 접근한 순서대로 처리할 수 있게 된다.  
- 캐시(Cache) 구현
- 대기열 순서와 같은 우선순위의 작업 예약
- 버퍼(Buffer)
	- 데이터를 한 곳에서 다른 한 곳으로 전송하는 동안 일시적으로 데이터를 보관하는 메모리의 영역  
	- 일반적으로 입출력 및 네트워크 관련 기능에서 이용한다.  

## Time Complexity


| Operation       | Average | Worst |
| --------------- | ------- | ----- |
| Access          | Θ(n)    | O(n)  |
| Search          | Θ(n)    | O(n)  |
| Insert(enqueue) | Θ(1)    | O(1)  |
| Delete(dequeue) | Θ(1)    | O(1)  |


>[!reference]
>[큐(Queue)](https://velog.io/@alkwen0996/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%ED%81%90Queue)
>[Python Queue library 공식문서](https://docs.python.org/ko/3.7/library/queue.html)  
>[큐(Queue) With Python](https://velog.io/@eunchae2000/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%ED%81%90Queue-with-Python)  
>[큐(Queue)](https://velog.io/@hysong/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%ED%81%90Queue)
