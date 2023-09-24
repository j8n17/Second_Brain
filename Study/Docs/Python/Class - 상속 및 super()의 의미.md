---
tags:
- class
- super
- MRO
---

```python
class Person:
	def __init__(self, name, age):
		self.name = name
		self.age = age 
	
	def get_name(self):
		print(f'제 이름은 {self.name}입니다.')
		
	def get_age(self):
		print(f'제 나이는 {self.age}세 입니다.')

class Student(Person):
	def __init__(self, name, age, GPA):
		super().__init__(name, age)
		self.GPA = GPA
		
	def get_GPA(self):
		print(f'제 학점은 {self.GPA}입니다.')
```

위의 코드에는 Person 클래스와 이를 상속받는 Student 클래스가 있다.

Student 클래스는 Person을 상속받기 때문에 Person의 get_name(), get_age() 메서드를 사용할 수 있다.

하지만 \_\_init\_\_()의 경우 Student 클래스에서 오버로딩 되므로 기존에 부모 클래스인 Person에서 \_\_init\_\_()을 통해 초기화된 attribute들을 사용하기 위해서는 super()를 사용해야 한다.

특히, \_\_init\_\_() 메서드는 오버로딩 하면 잘 쓸 일이 없는 다른 오버로딩 메서드와 달리 속성을 초기화하는 메서드이므로 super()를 사용해 기존 클래스에서 정의한 속성을 초기화할 필요가 있다.

이런 super()은 MRO(Method Resolution Order) 순서에 따른 '다음' 클래스 객체를 리턴하는 함수다.

밑에서 더 자세히 설명한다.

# super(class, self)

super(class, self)는 입력된 class의 MRO 순서상의 다음 클래스 객체를 가져온다.

예를 들어, Person -> Student -> College_Student 순으로 연속적으로 상속되었을 경우 Person의 메서드를 사용하고 싶다면 super(Student, self)를 사용하면 되는 것이다.

super()는 python3에서 생긴 것으로 해당 클래스의 바로 다음 순서 클래스 객체를 가져온다.
만약 College_Student 객체에서 사용한다면 super(College_Student, self)와 같은 것이다.
# 다중 상속시 super()

다중 상속시에도 똑같이 작동한다.

다중 상속시에는 MRO 순서가 먼저 상속된 순서로 지정되기 때문에 상속 순서로 super()가 동작한다.

```python
class A:
	def __init__(self):
		print("A") 

class B:
	def __init__(self):
		print("B")
		
class C(A, B): # A가 먼저 상속되었다.
	def __init__(self):
		super().__init__() # super(C, self)와 같고 먼저 상속된 A의 메서드를 쓸 수 있다.
		super(A, self).__init__() # B의 메서드를 쓸 수 있다.
	
```

# Method Resolution Order

MRO 순서대로 하는 이유는 메서드를 사용할 때 이 순서로 해당 메서드가 정의되었는지 찾기 때문이다.

만약 메서드가 정의되어 있지 않으면 다음 부모 클래스에서 메서드를 찾는 것이다.