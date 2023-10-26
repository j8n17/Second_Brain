---
tags:
- __dict__
- __slots__
---

[[Class - __dict__|__dict__]]를 사용하면 클래스 객체의 속성을 Dict로 간단하게 볼 수 있다. [[tensor의 storage, offset, stride 개념]]
[[tensor의 storage, offset, stride 개념|storage]]
하지만 \_\_dict\_\_는 메모리 사용량이 많고 외부에서 새로운 attribute를 제한없이 만들어 낼 수 있다.

이런 문제를 해결하기 위해 클래스 내부에 \_\_slots\_\_ class attribute를 할당해 \_\_dict\_\_ 대신 사용할 수 있다.

```python
>>> class Bar :
...
...     __slots__ = ['x', 'y', 'z']
...
...     def __init__(self, x, y) :
...         self.x = x
...         self.y = y
...
>>>
>>> bar = Bar(1, 2)
>>> bar.__slots__
['x', 'y', 'z']
>>> bar.x
1
>>> bar.z
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: z
>>> bar.z = 10
>>> bar.z
10
>>> bar.k = 11
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Bar' object has no attribute 'k'

```

위 코드를 보면 \_\_slots\_\_으로 등록된 attribute들만 사용할 수 있고 등록이 되지 않은 attribute는 사용할 수 없다.