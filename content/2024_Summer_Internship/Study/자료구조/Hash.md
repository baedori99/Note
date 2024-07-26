---
title: Hash
create_date: 2024-07-25 15:07 - 2024-07-25 15:07
draft: false
---
## 해시(Hash)란?

일반적으로 말하는 해시(Hash)는 **해시 테이블(Hash Table)** 로 <U>Key와 Value를 매핑해서 데이터를 저장하는 자료구조</U>이다.

파이썬에서는 기본적으로 제공되는 딕셔너리 자료형이 해시 테이블과 같은 구조이다.

### 용어 정리

- **키(Key)**: 고유의 값으로 해시 함수의 Input. 다양한 길이의 값이 될 수 있다.  
- **해시 테이블(Hash Table)** 또는 **해시 맵(Hash Map)**: Key와 Value를 매핑해서 데이터를 저장하는 자료구조  
- **해시 함수(Hash Function)**: 임의의 값을 고정된 길이의 데이터로 변환하는 함수. 다양한 길이의 키를 고정된 길이의 해시로 변환시키므로 저장소를 효율적으로 운영할 수 있게 해준다.  
- **해시(Hash)**: 해시 함수의 output으로 해시 값과 매칭되어 버킷에 저장된다.  
- **해시 값(Hash Value)** 또는  **해시 주소(Hash Address)**: 키에 해시 함수를 적용하여 얻는 해시 값  
- **버킷(Bucket)**: 한 개의 데이터를 저장할 수 있는 공간  

### 예시
![|550](https://imgur.com/xXjxUNb.png)

위 그림으로 설명을 하자면, 다양한 길이를 갖고 있는 **Key** 값에 **해시 함수(Hash Function)** 를 적용시키면 00, 01, 02 등과 같이 <U>고정된 길이의 데이터로 변환</U>이 된다. 이렇게 변환된 데이터가 **해시 값(Hash Value)** 이고, **버킷(Bucket)** 에는 <U>키와 매핑된 원래 데이터를 저장</U>하게 된다.  

결과적으로 <U>변환된 키값과 버킷에 매핑되어 있는 데이터</U>를 **해시(Hash)** 라고 하고 이러한 자료구조를 **해시 테이블(Hash Table)** 이라고 한다.


## 해시 충돌(Hash Collision)

`Hash Collision`은 해시 함수 서로 다른 두 입력값에 대해 동일한 출력값을 내는 상황을 의미한다.  
해시 함수가 무한한 입력 데이터로부터 유한한 출력 데이터를 생성하는 경우, 비둘기집 원리(Pigeonhole Principle)에 의해 해시 충돌 문제를 피할 수 없다.

>[!info]- 비둘기집 원리(Pigeonhole Principle)
>비둘기집 원리란, n개의 아이템을 m개의 컨테이너에 넣을 때, n > m이라면 적어도 하나의 컨테이너에는 반드시 2개 이상의 아이템이 들어 있다는 원리를 말한다.
>![|300](https://imgur.com/OlW3HIJ.png)

>[!info]- 생일 문제(Birthday Problem)
>생일의 가짓수는 윤년을 제외하면 365개이므로, 여러 사람이 모였을 때 생일이 같은 2명이 존재할 확률은 꽤 낮을거 같다.
>하지만 실제로는 23명만 모여도 그 확률은 50%를 넘고, 57명이 모이면 99%를 넘어선다.
>>[!example]- 코드
>>- 23명의 생일을 비교했을 때 생일이 같은 사람이 있는지 검사하는 것이 한 시행이며, 이러한 시행을 TRAILS번 반복한다.
>>- 실행 시 50%~51% 정도의 결과를 출력하는 것을 볼 수 있다.
>>```python
>># 생일 문제(Birthday Problem)
>>import random
>>TRAILS = 100000
>>same_birthdays = 0
>>for _ in range(TRAILS):
>>    birthdays = []
>>    for _ in range(23):
>>        birthday = random.randint(1,365)
>>        if birthday in birthdays:
>>            same_birthdays += 1
>>            break
>>        birthdays.append(birthday)
>>print(f'{same_birthdays / TRAILS * 100}%')
>>```
>
>>[!example] 실행 결과
>>![](https://imgur.com/ECUg1C0.png)

### 로드 팩터(Load Factor)
$$load factor = \frac{n}{k}$$

`Load Factor`란, `Hash Table`에 저장된 <U>데이터 개수 `n`을 버킷의 개수 `k`로 나눈 것</U>이다.

- `1`이면 `Hash Table`이 꽉 찬것이다
- `1`보다 큰 경우 `Hash Collision`이 발생을 의미한다

`Load Factor`가 증가할수록 `Hash Table`의 성능은 **감소**한다.
`Load Factor` 비율에 따라서 해시 함수를 재작성해야 할지, `Hash Table`의 크기를 조정해야할지 결정할 수 있다.

- 해시 함수가 키들을 잘 분산해주는지 말하는 효율성 측정에도 사용된다.
	- `Java 10`에서는 `Hash Map`의 `Default Load Factor`를 0.75로 정했다.
	- 이는 **시간과 공간 비용의 적절한 절충안**이라고 말한다.
	- `Load Factor`가 0.75를 넘어설 경우 동적 배열처럼 `Hash Table`의 공간을 재할당하게 된다.

## 해시 충돌 해결

해시 충돌을 해결하는 방법에 대해 나열한다
### Separate Chaining

충돌 발생 시 연결 리스트(Linked List)로 연결하는 방식

- 예시
![|500](https://imgur.com/d6as3YO.png)

위 그림에서 `index[2]`에서 `윤아`에 충돌한 `서현`은 `윤아`의 다음 아이템으로 연결되었다.

Separate Chaining의 간단한 원리를 요약하면
1. Key의 Hash Value 계산
2. Hash Value를 이용해 배열의 index 계산
3. 충돌시 `Linked List`로 연결

>[!note]- 구현 코드
>```python
># Separate Chaining 구현
>class Node:
>    def __init__(self, item: int, link=None):
>        self.item = item
>        self.link = link
>
>class HashTable:
>    def __init__(self):
        self.size = 3
        self.table = [None] * self.size
>
    def print_table(self) -> None:
        for i, root in enumerate(self.table):
            items = []
>
            while root:
                items.append(root.item)
                root = root.link
>
            print(i, end=' : ')
            print('->'.join(map(str, items)))
>
    def add(self, item: int) -> None:
        if self.table[item % self.size] is None:
            self.table[item % self.size] = Node(item)
            return
>
        prev = self.table[item % self.size]
>
        while prev.link:
            prev = prev.link
        prev.link = Node(item)
>
    def remove(self, item: int) -> None:
        if self.table[item % self.size] is None:
            return
>
        prev = self.table[item % self.size]
>        
        while prev.link:
            if prev.link.item == item:
                prev.link = prev.link.link
                break
>
ht = HashTable()
>
for i in range(10 + 1):
    ht.add(i)
ht.print_table()
>```

>[!example]- 실행 결과
>![](https://imgur.com/3p2zhq9.png)


### Open Addressing

충돌 발생 시 탐사(Probing)를 통해 다른 빈 공간을 찾는 방식  

- 모든 원소가 반드시 자신의 해시 값에 대응하는 주소에 저장된다는 보장이 없다.  

![|400](https://imgur.com/nr3ow4V.png)

위 그림에서 `윤아`에 충돌한 `서현`은 빈 공간을 탐사해 `index[3]`에 저장된다.  

여러 Open Addressing 방식이 있다.  

#### Linear Probing(선형 탐사)

충돌이 발생하면 해당 위치부터 순차적으로 버킷을 하나하나씩 탐색  

구현이 간단하면서도 전체적인 성능이 좋은 편이다.  
**그러나** 선형 탐사 활용 시, 해시 테이블에 저장되는 데이터들이 <U>고르게 분포되지 않고 뭉치는 경향</U>이 있다.  

- **클러스터링(Clustering)**: 해시 테이블 여기저기에 연속된 데이터 그룹이 생기는 현상 

클러스터들이 점점 커지면 주변 클러스터들과 서로 합쳐져 해시 테이블 특정 위치에 데이터가 몰리게 되고, 이는 탐사 시간을 오래 걸리게 하여 해싱 효율을 떨어뜨린다.

>[!Note]- Linear Probing Code
>```python
>class HashTable:
>
>    def __init__(self):
>        self.size = 3
>        self.table = [None] * self.size
>
>    def _probe(self, item: int, step: int) -> int:
>        index = item % self.size
>        while index < self.size:
>            if self.table[index] is None:
>                return index
>            index += step
>        return -1
>
>    def print_table(self) -> None:
>        for i, item in enumerate(self.table):
>            print("{} : {}".format(i, item))
>
>    def add(self, item: int) -> None:
>        index = self._probe(item, 1)
>
>        if index == -1:
>            index = self._probe(item, -1)
>
>        if index == -1:
>            return
>        self.table[index] = item
>
>    def remove(self, item: int) -> None:
>        start = item % self.size
>
>        for i in range(start, self.size):
>            if self.table[i] == item:
>                self.table[i] = None
>
>        for i in range(0, start):
>            if self.tanle[i] == item:
>                self.table[i] = None
>
>ht = HashTable()
>ht.add(1)
>ht.add(4)
>ht.add(2)
>ht.print_table()
>```

>[!example]- 실행 결과
>![](https://imgur.com/jdrZ9JQ.png)


![](https://imgur.com/jFkqc8v.png)

해시 값 `02`로 충돌이 발생해서 다음 버킷을 탐색했는데 빈 버킷이므로 해당 위치에 데이터를 저장한다.  
선형 탐색 이외에 제곱 탐색, 이중 해시 방법이 있다.  

- **제곱 탐색(Quadratic Probing)**: 충돌이 발생하면 해당 위치부터 제곱만큼 떨어진 다음 버킷을 탐색(1, 4, 9, 16, ...)
- **이중 해시(Double Hashing)**: 충돌이 발생하면 다른 해시 함수를 한번 더 적용

## 장, 단점 및 용도

### 장점  
- 데이터 저장 /  읽기 속도가 빠름 (검색 속도가 빠름)
- 해시는 키에 대한 데이터가 있는지 확인이 쉬움

### 단점  
- 일반적으로 저장공간이 많이 필요하다.
- 여러 키에 해당하는 주소(index)가 동일한 경우 충돌을 해결하기 위한 별도 자료구조 필요하다.

### 용도
- 검색이 많이 필요한 경우
- 저장, 삭제, 읽기가 빈번한 경우
- 캐쉬 구현


## Time Complexity

- O(1)
	- 각각의 Key값은 해시 함수에 의해 고유한 index를 가지게 되어 바로 접근 가능하다.
	- 데이터 저장 및 읽기 처리 속도가 매우 빠르다.
- O(n)
	- 충돌로 인한 Worst Case이다.


>[!reference]
>[해시 테이블(Hash Table)](https://velog.io/@hysong/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%ED%95%B4%EC%8B%9C-%ED%85%8C%EC%9D%B4%EB%B8%94Hash-Table#%ED%95%B4%EC%8B%9C-%EC%B6%A9%EB%8F%8Chash-collision)  
>[해시(Hash)](https://c4u-rdav.tistory.com/18)  
>