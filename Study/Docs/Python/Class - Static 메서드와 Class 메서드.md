
# Static 메서드

메서드의 인자로 self를 안 쓰거나, self를 안 쓰고 @staticmethod 데코레이터를 붙이면 static 메서드.

근데 self를 안 쓴 static 메서드는 객체가 아닌 클래스를 통해서만 사용할 수 있고, @staticmethod 데코레이터를 붙인 static 메서드는 객체를 통해서도 사용할 수 있다.

```python
>>> class Point:
...     def bar(): # 메서드의 인자로 self를 쓰지 않음
...             print('bar')
...
>>> p = Point()
>>> p.bar() # @staticmethod가 아니므로 사용불가
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: bar() takes 0 positional arguments but 1 was given
>>> Point.bar()
bar

>>> class Point:
...     @staticmethod # self를 쓰지 않고 @staticmethod를 명시함
...     def bar():
...             print('bar')
...
>>> p = Point()
>>> p.bar() # @staticmethod 이므로 사용가능
bar
>>> Point.bar()
bar
```

이런 static 메서드는 아래 코드와 같이 비슷한 류의 유틸리티 메서드를 하나의 클래스에 묶어 두고 싶은 경우에 사용할 수 있다.

```python
class StringUtils:
    @staticmethod
    def toCamelcase(text):
        words = iter(text.split("_"))
        return next(words) + "".join(i.title() for i in words)

    @staticmethod
    def toSnakecase(text):
        letters = ["_" + i.lower() if i.isupper() else i for i in text]
        return "".join(letters).lstrip("_")
```

# Class 메서드

@classmethod 데코레이터를 붙인 메서드가 class 메서드이다.

이는 첫번째 인자로 self로 객체를 입력받지 않고, cls로 클래스를 입력받는다.
고로 이를 통해 class attribute 또는 메서드에 접근하거나 호출할 수 있다.

따라서 객체가 아닌 클래스에서 메서드를 호출해야 한다. (첫번째 인자로 클래스를 넘겨주기 위해)

```python
class User:
    def __init__(self, email, password):
        self.email = email
        self.password = password

    @classmethod
    def fromTuple(cls, tup):
        return cls(tup[0], tup[1])

    @classmethod
    def fromDictionary(cls, dic):
        return cls(dic["email"], dic["password"])
```

위의 클래스는 직접 데이터를 입력하거나 튜플, 사전 입력을 통해 객체를 생성할 수 있다.

```python
>>> user = User("user@test.com", "1234")
>>> user.email, user.password
('user@test.com', '1234')

>>> user = User.fromTuple(("user@test.com", "1234"))
>>> user.email, user.password
('user@test.com', '1234')

>>> user = User.fromDictionary({"email": "user@test.com", "password": "1234"})
>>> user.email, user.password
('user@test.com', '1234')
```

이런 클래스 메서드는 팩토리 메서드를 작성할 때 유용하다고 한다.