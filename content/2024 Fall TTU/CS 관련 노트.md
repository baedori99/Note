---
title: CS 관련 노트
create_date: 2024-09-02 23:09 - 2024-09-02 23:09
draft: true
---
# Python

## Class (클래스)
### self.variable 와 variable의 차이점
```python
class Student:
    def __init__(self,class_list=None):
        if class_list is None:
            class_list = []
        self.class_list = class_list
    def print_class_list(self):
        print("Enrolled course in this Fall semester:")
        for i in range(len(self.class_list)):
            print(f"{i+1}.{self.class_list[i]}")
```

- Assignment를 하다가 클래스를 만드는데 아래 `class_list`를 쓰고 돌렸지만 정의된 것이 없다고 떠서 `self.class_list`로 바꾸니 해결되었다. 
- 이에 대해 궁금점이 생겨 노트에 적어 놓을려고 한다. 
1. `class_list`
	- `class_list`는 `__init__` 메서드의 매개변수이다. 이는 메서드가 호출될 때 전달되는 값을 받는 변수다. 
	- `class_list`는 `__init__`메서드 안에서만 유효하다. 즉 `__init__`메서드 밖에서는 접근할 수 없다. 
	- 예시로 `class_list`는 `__init__` 메서드가 끝나면 더 이상 사용할 수 없기 때문에, `print_class_list` 메서드에서 사용할 수 없다. 
2. `self.class_list`
	- `self.class_list`는 클래스 인스턴스의 속성이다. `self`는 해당 인스턴스 자체를 가리키는 참조어고, `self.class_list`는 인스턴스의 `class_list`속성을 나타낸다. 
	- `self.class_list`는 클래스의 다른 메서드에서도 접근할 수 있으며, 클래스 인스턴스가 존재하는 한 유지된다. 
	- 이를 통해 `class_list`의 값을 `__init__` 메서드에서 받은 후, 객체의 속성으로 저장하고 다른 메서드에서도 사용할 수 있게 된다. 
- 코드에서의 차이
	- `self.class_list`는 `Student` 클래스의 인스턴스가 가진 속성으로, 클래스 내 다른 메서드에서도 사용할 수 있다.
	- `class_list`는 `__init__`메서드에서만 사용 가능한 매개변수로, `print_class_list`메서드에서 접근하려면 인스턴스 속성으로 저장해야 한다. 