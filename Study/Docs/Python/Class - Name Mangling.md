---
tags:
- class
- private
- attribute
---

# Name Mangling이란

Name Mangling은 under score(\_\_) 2개를 사용해 클래스 속성 및 메서드의 이름을 변경하는 것을 뜻한다.

under score 2개를 사용해 정의된 클래스의 속성은 private 변수처럼 인스턴스에서 직접 접근할 수 없다.
하지만 이는 엄밀히 말하자면 Information Hiding(정보은닉)이 아닌 Name Mangling이다.

Python에서는 under score로 정의된 클래스 속성의 이름을 '\_클래스이름.\_\_속성이름'으로 변경한다.

```python
>>> class A():
...     def __init__(self):
...             self.__b = 10
...
>>> a = A()
>>> a.__dict__
{'_A__b': 10}
```

위의 코드와 같이 속성의 이름이 변경되어 기존 속성명이 없기 때문에 기존에 지정했던 속성명으로 접근할 수 없는 것이다.

또한 속성명이 변하기 때문에 해당 클래스가 상속되면 파생 클래스에서는 쉽게 해당 속성을 변경할 수 없다.

고로 해당 속성에 직접 접근하는 일은 피하고 최대한 클래스에 정의된 [[Class - getter, setter|getter, setter]] 메서드를 사용하도록 하자.


###### 참고
[[Class - __dict__]]
