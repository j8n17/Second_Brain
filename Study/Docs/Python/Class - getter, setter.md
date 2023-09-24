getter와 setter는 클래스의 attribute에 직접 접근하지 않고 메서드를 통해 접근하도록 하는 방식이다.

아래 코드를 보면 음수가 될 수 없는 나이가 음수로 저장되는 것을 볼 수 있다.

```python
>>> class Person :
...     def __init__(self, name : str, age : int) :
...         self.name = name
...         self.age = age
...
>>> person = Person('Jason', '24')
>>> print(person.name, person.age)
Jason 24
>>> person.name = 10
>>> person.age = -100
>>> print(person.name, person.age)
10 -100
```

getter와 setter를 사용하면 유효성 검사를 통해 객체가 잘못된 상태를 가지는 상황을 방지할 수 있다.

```python
>>> class Person :
...     def __init__(self, name : str, age : int) :
...         self.__name = name
...         self.__age = age
...     def get_name(self) :        # getter of name
...         return self.__name
...     def set_name(self, name) :  # setter of name
...         if True != isinstance(name, str) :
...             raise ValueError('name should be str type')
...         self.__name = name
...     def get_age(self) :         # getter of age
...         return self.__age
...     def set_age(self, age) :    # setter of age
...         if True != isinstance(age, int) or 0 > age :
...             raise ValueError('age should be int type and not be negative')
...         self.__age = age
...         
>>> person = Person('Jason', '24')
>>> print(person.get_name(), person.get_age())
Jason 24
>>> person.set_name(10)
ValueError: name should be str type
>>> person.set_age(-100)
ValueError: age should be int type and not be negative
```