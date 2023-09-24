---
tags:
- __init__
- __del__
- 생성자
- 소멸자
- constructor
- destructor
- class
---

# Constructor

정의된 클래스로 인스턴스가 생성될 때 값을 초기화 하지 않고 메서드를 사용하면 값이 없기 때문에 오류가 발생하는 경우가 종종 있다.

이런 경우 생성자\_\_init\_\_()를 통해 값을 초기화할 수 있다.

# Destructor

반대로 소멸자는 객체가 삭제될 때의 상황을 통제하는 메서드이다.

소멸자 \_\_del\_\_()를 정의하므로써 삭제될 때의 상황을 통제할 수 있다.

# Code

```python
>>> class A():
...     def __init__(self, name, age):
...             self.name = name
...             self.age = age
...     def __del__(self):
...             print(f"{self.name}가 삭제되었습니다.")
...
>>> a = A('Eric', 10)
>>> a.name
'Eric'
>>> a.age
10
>>> del a
Eric가 삭제되었습니다.
```