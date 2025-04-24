---
title: Day 1 - Big O Notation
description: 한달 CS 기초 다시 다지기 - Day 1
date: 2025-04-23
draft: true
aliases:
  - CS
  - 기초
  - 알고리즘
  - 자료구조
tags:
  - CS
---

> [!tip] Summary
> - Big-O는 알고리즘의 성능(시간/공간)을 입력 크기에 따라 수학적으로 표현하는 방법이다.
> - O(1), O(N), O($N^2$), O(log N), O(N log N), O($2^n$)


---
## Big-O Notation이란?
- 알고리즘의 효율성을 표기해주는 표기법이다. 
- Big-O는 입력 크기에 따라 성능의 상한선을 수학적으로 표현한 것
	- 최악의 경우를 기준으로 알고리즘의 시간 또는 공간 사용량을 나타내는 수학적 표기법이다.
- 알고리즘의 성능을 결정하기 위해서는 각각의 성능을 평가해야한다. 
	- 1️⃣ Time Complexity (시간 복잡도)
	- 2️⃣ Space Complexity (공간 복잡도)
- **알고리즘의 성능 평가는 시간 복잡도만**, 그중에서도 **Big-O 표기법**을 기준
### Time Complexity (시간 복잡도)
- 알고리즘이 데이터를 처리하는 데 걸리는 시간을 나타내는 개념이다. 
- ✏️ 입력 크기(n)에 따라 연산 횟수가 어떻게 증가하는지를 나타낸다.
### Space Complexity (공간 복잡도)
- 데이터 관리에 필요한 공간을 나타내는 개념이다.
- 알고리즘 과정에서 얼마나 많은 공간(메모리)을 차지하는지 분석하는 것이다.
- 데이터의 흐름에 따라 공간이 어떻게 변하는지 표현하고 측정할 수 있게 도와주는 지표이다.
- ✏️ 알고리즘이 실행되며 사용하는 총 메모리 크기(입력값, 임시 변수 등 포함)을 분석한다.

>[!tip] 
>- 시간과 공간은 반비례적인 경향이 있다.
>- **시간 복잡도는 실행 시간의 효율성**
>- **공간 복잡도는 메모리의 효율성**

## Big-O Notation의 특징
1. 상수항을 무시한다.
	- 어떤 알고리즘이 O(N+3)의 복잡도를 가졌다면 계수를 생략하고 O(N)
2. 입력 크기가 클 때 계수도 무시한다.
	- 알고리즘의 복잡도가 O(2N)의 복잡도를 가졌다면 계수를 생략하고 O(N)으로 표기한다.
3. 가장 큰 영향력이 있는 항, 즉 최고차 항만 표기한다.
	- 알고리즘이 O($N^2+4N+2$)의 복잡도를 가졌다면 O($N^2$)으로만 표기한다. 

### 1. O(1), 상수
```python
# Example 1 - O(1)
def print_first(arr):
    print(arr[0])
    print(arr[0]) # 만약 2개가 있어도 여전히 O(1)으로 표기된다.
```

- 입력 데이터의 크기에 상관없이 언제나 일정한 시간이 걸리고 데이터의 양이 증가해도 성능에 거의 영향을 미치지 않는다.
- 이 함수의 시간 복잡도는 **constant time(상수 시간)**이라고 할 수 있다.
	- N이 얼마나 크든 관계없이 끝내는데 동일한 숫자의 스텝이 필요하다. 
  
>[!example]
>- **Stack**의 **Push**, **Pop**이 대표적이다.

>[!info]- Graph
>![](https://imgur.com/5Wuu9I5.png)

### 2. O(N), 선형
```python
def print_all(arr):
    for n in arr:
        print(n)
    for n in arr: # for문이 두 번 돌지만 표기는 O(N)으로 한다.
        print(n)
```

- 입력 데이터의 크기에 비례해 처리 시간이 증가한다.
- Linear Search와 비슷하다.
	- 배열의 각각 아이템을 위해 모두 작업해야 한다.
	- 배열이 커지게 되면 필요 스텝도 커지게 된다. 

>[!example]
>- for문이 대표적이다.
>- Linear Search(선형 탐색)과 비슷하다.

>[!info]- Graph
>![](https://imgur.com/oA0irDB.png)

### 3. O($N^2$), 다항
```python
def print_twice(arr):
    for n in arr:
        for x in arr:
            print(x,n)
```

- 입력 데이터가 많아질수록 처리시간이 급수적으로 늘어난다.
- `Quadratic Time`이라고도 하며, `Nested Loops`가 있을 때 발생한다.
- 위의 예시도 배열의 각 아이템에 대하여 루프를 반복하여 실행한다. 
>[!Example]
>- Insertion Sort (삽입 정렬)
>- Bubble Sort (버블 정렬)
>- Selection Sort (선택 정렬)
>- 이중 for문이 대표적이다.

>[!info]- Graph
>![](https://imgur.com/Zq9ZpPs.png)

### 4. O(log N), 로그
```python
def find_position(val, Arr, n):
    global steps
    l = 0
    r = n - 1
    while(l <= r):
        steps += 1
        m = l + (r-l)// 2
        if (Arr[m] == val):
            return m
        elif (Arr[m] < val):
            l = m + 1
        else:
            l = m - 1
    return -1

Arr = [2, 4, 6, 8, 10, 12, 14, 16]
steps = 0

idx = find_position(8, Arr, 8)
```

- 입력 데이터의 크기가 커질수록 처리 시간이 로그만큼 짧아진다.
- `Logarithmic Time`이라고도 불린다.
- 이진 탐색을 예로 들어보면. 
	- 입력 데이터의 크기가 2배로 커져도, 검색을 하기 위한 스탭은 +1만 증가한다.
		- 한번만 더 나누면 되기 때문이다.
	- ✏️이진 탐색은 정렬되지 않은 배열에선 사용할 수 없다.
>[!example]
>- Binary Search (이진 탐색)
>- 재귀가 순기능으로 이루어지는 경우에도 해당된다. 

>[!info]- Graph
>![](https://imgur.com/gtyvouF.png)

### 5. O(N log N), 선형 로그
- 입력 데이터가 많아질수록 처리 시간이 로그 배만큼 더 늘어난다.
- 예를 들어 데이터가 10배가 되면 처리 시간은 약 20배가 된다.
>[!example]
>- Quick Sort (퀵 정렬)
>- Merge Sort (합병 정렬)
>- Heap Sort (힙 정렬)

- ✏️`Merge Sort`는 배열을 반으로 나누는 작업 👉 O(log N)
	- 병합 하는데는 O(N) 👉 전체가 O(N log N)이 된다.
### 6. O($2^n$), 지수
- 입력 데이터가 많아질수록 처리시간이 기하급수적으로 늘어난다.
>[!example]
>- Fibonacci (피보나치 수열)
>- 재귀가 역기능으로 이루어질 경우에도 해당된다.

- ✏️ 재귀호출이 분기할 때 나타난다.
  
### 📌주요 Big-O Notation Graph
>[!Summary]
>![](https://imgur.com/3IZJBMF.png)


---

> [!note] References
> 1. [빅오 표기법:Big-O Notation 정의/특징/복잡도/종류/비교/예제](https://aiday.tistory.com/54)
> 2. [개발자라면 이제는 알아야하는 Big O 설명해드림. 10분컷.](https://www.youtube.com/watch?v=BEVnxbxBqi8&list=PL7jH19IHhOLMdHvl3KBfFI70r9P0lkJwL&index=6&ab_channel=%EB%85%B8%EB%A7%88%EB%93%9C%EC%BD%94%EB%8D%94NomadCoders)
> 3. [Big O Notations](https://www.youtube.com/watch?v=V6mKVRU1evU)
> 4. [What is Logarithmic Time Complexity? A Complete Tutorial](https://www.geeksforgeeks.org/what-is-logarithmic-time-complexity/#)

