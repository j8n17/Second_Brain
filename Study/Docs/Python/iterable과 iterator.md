---
tags:
- iterable
- iterator
- next
- iter
---

> [!Summary]
iterable은 `__iter__()` 또는 `__getitem__()` 메서드로 정의된 class.
iterator는 next()로 호출가능한 객체.

# iterable

iterable은 원소를 하나씩 반환가능한 객체. ex) dict, list, tuple, set, str, bytes, range
또한 `__iter__()` 또는 `__getitem__()` 메서드로 정의된 class는 모두 iterable이다.

# iterator

iterator는 next() 메서드로 순차적으로 호출가능한 객체이다.
next()로 불러올 다음 데이터가 없는 경우 Stop Exception을 발생시킨다.

# iter()

모든 iterable은 iterator가 아니므로 next()를 통해 호출하지 못하는 경우도 있다.
그럴 경우 iter()을 통해 iterator 타입으로 변경한 후 사용할 수 있다.
```python
>>> x = [1, 2]
>>> type(x)
<type 'list'>

>>> y = iter(x)
>>> type(y)
<type 'listiterator'>

>>> next(y)
1
>>> next(y)
2
>>> next(y)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration
```
