---
tags:
 - decorator
 - private
 - class
 - getter
 - setter
---

@property와 @name.setter는 인스턴스의 private 속성을 [[Class - getter, setter|getter, setter]]보다  직관적이고, 간결하고, 간편하게 다루기 위한 방법이다.

이를 통해 메서드를 이용하지 않고 변수를 다루는 것처럼 private의 값을 다룰 수 있게 된다.

여기서 @property로 지정되는 메서드, name의 이름은 모두 동일해야 한다.

```python
>>> class A():
...     def __init__(self):
...             self.__value = 10
...     @property
...     def v(self):
...         return self.__value
...     @v.setter
...     def v_change(self, value):
...         self.__value = value
...     def get_v(self):
...         return self.__value
...     def set_v(self, value):
...         self.__value = value
...
>>> a = A()
>>> a.set_v(7) # 기존의 getter, setter
>>> a.get_v()
7
>>> a.v = 5 # @property, @name.setter
>>> a.v
5
```

## @property만 사용

@name.setter는 혼자만 사용할 수 없지만 @property는 혼자만 정의되어 사용할 수 있다.

예를 들어 어떤 private 변수의 호출을 카운팅하는 함수로 사용할 수 있다.

```python
>>> class A():
...     def __init__(self):
...         self.__data = 0;
...     @property
...     def data(self):
...         self.__data = self.__data + 1;
...         return self.__data;
>>> a = A()
>>> a.data
1
>>> a.data
2
>>> a.data
3
```

그냥 프로퍼티의 설명만 보면 public 변수와 사용법이 같기 때문에 의미가 없어보이지만, 사실 oop에서는 매우 중요한 변수 관리 문법이라고 한다.