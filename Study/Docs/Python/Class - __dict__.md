---
tags:
 - __dict__
---


\_\_dict\_\_ attribute를 통해 클래스 객체의 속성을 Dict 형태로 간단하게 확인할 수 있다.

```python
>>> class A:
...     def __init__(self):
...             self.value = 10
...
>>> a = A()
>>> a.__dict__
{'value': 10}
>>> a.value = 20
>>> a.__dict__
{'value': 20}
>>> a.value_2 = 30
>>> a.__dict__
{'value': 20, 'value_2': 30}
```


