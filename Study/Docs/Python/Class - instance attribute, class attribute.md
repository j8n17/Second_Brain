# instance attribute

instance attribute는 우리가 흔히 다루는 attribute로, \_\_init\_\_()에서 self.attribute = value의 형태로 초기화되는 attribute이다.

이 instance attribute는 값을 할당하거나 변경하는 것 모두 해당 객체에만 적용된다.

# class attribute
Class attribute란 각 객체마다의 값을 갖는 instance attribute와 달리 클래스 내의 고윳값을 갖는 attribute이다.

즉, 한 클래스에 대해 여러 객체를 생성하면 각 객체마다 instance attribute의 값은 모두 다르지만, class attribute의 값은 언제나 같다.

class attribute는 `class.attribute`꼴로 접근한다.

```python
>>> class Point():
...     count = 0
...     def __init__(self):
...             Point.count += 1 # class.attribute 꼴로 접근.
...             print('create instance!')
...
>>> a = Point() # count = 1
create instance!
>>> b = Point() # count = 2
create instance!
>>> c = Point() # count = 3
create instance!
>>> Point.count += 1 # count = 4
>>> print(Point.count, a.count, b.count, c.count)
4 4 4 4
```

하지만 아래의 코드를 보면 객체를 통해 값을 할당하는 순간 해당 객체의 attribute의 값만 변경된다.

```python
>>> a.count = 10
>>> print(Point.count, a.count, b.count, c.count)
3 10 3 3
```

그 이유는 값을 할당하는 순간 instance attribute가 생성되고, attribute가 호출될 때는 먼저 instance attribute로 할당되어 있는지 확인하기 때문이다.

고로 이런 class attribute를 다룰 때는 [[Class - Name Mangling|under score(\_\_)]]를 통해 변수명을 숨기거나 [[Class - getter, setter|getter, setter]]를 사용하는 등 직접 접근할 수 없게 설정해야 한다.