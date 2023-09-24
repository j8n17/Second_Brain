---
tags:
- Module
- Parameter
- forward
- backward
- optimizer
- loss
- Autograd
- Sequential
- ModuleList
- ModuleDict
---
module이 어떤 내용으로 구성되었는지 - https://pytorch.org/docs/stable/notes/modules.html
# Module

Module은 신경망 모델을 위한 기반 클래스로, 모든 모델은 Module의 subclass여야 한다.

Module은 다른 Module들을 포함할 수 있고, 이때 attribute 할당을 통해 Module들을 Sub Module로 간단히 등록할 수 있다.

각 Module들은 기능에 따라 다음과 같이 명명할 수 있다. (공식적인 것은 아님.)

- 여러 function들을 모아놓은 Module은 basic building block
- basic building block을 모아놓은 Module은 딥러닝 모델
- 딥러닝 모델을 모아놓은 Module은 더 큰 딥러닝 모델

설계자는 이와 같은 Module들을 다양한 조합과 순서로 구성할 수 있다.

# Container

Container는 Sub Module들을 쉽게 묶을 수 있는 Module의 subclass이다.

Container는 크게 3가지로 나눌 수 있다.

- [Sequential](https://pytorch.org/docs/stable/generated/torch.nn.Sequential.html#torch.nn.Sequential) - Sub Module들을 순차적으로 실행하는 Module.
- [ModuleList](https://pytorch.org/docs/stable/generated/torch.nn.ModuleList.html#torch.nn.ModuleList) - index를 이용해 Sub Module을 실행하는 Module.
- [ModuleDict](https://pytorch.org/docs/stable/generated/torch.nn.ModuleDict.html#torch.nn.ModuleDict) - key를 통해 Sub Module을 실행하는 Module.

간단한 순차구조는 Sequential을 통해 묶을 수 있지만 그렇지 못한 경우 ModuleList를 통해 필요한 Sub Module들만 뽑아서 실행할 수 있다.

하지만 Sub Module이 많아지면 많아질수록 key를 이용해 직접 Sub Module을 찾아서 실행하는 것이 효율적이다.

이런 Container들도 한 Module에서 병합되어 결국 하나의 큰 딥러닝 모델을 이루게 된다.
## Code

```python
import torch
from torch import nn

class Add(nn.Module):
	def __init__(self, value):
		super().__init__()
		self.value = value
	def forward(self, x):
		return x + self.value

# input
x = torch.tensor([1])

# Sequential
calculator = nn.Sequential(Add(3),
						   Add(2),
						   Add(5))
output = calculator(x)

# ModuleList
class Calculator(nn.Module):
	def __init__(self):
		super().__init__()
		self.add_list = nn.ModuleList([Add(2), Add(3), Add(5)])
	
	def forward(self, x):
		x = self.add_list[1](x)
		x = self.add_list[0](x)
		x = self.add_list[2](x)
		return x
calculator = Calculator()
output = calculator(x)

# ModuleDict
class Calculator(nn.Module):
	def __init__(self):
		super().__init__()
		self.add_dict = nn.ModuleDict({'add2': Add(2),
									   'add3': Add(3),
									   'add5': Add(5)})
									   
	def forward(self, x):
		x = self.add_dict['add3'](x)
		x = self.add_dict['add2'](x)
		x = self.add_dict['add5'](x)
		return x
calculator = Calculator()
output = calculator(x)
```
## 추가
사실 ModuleList, ModuleDict가 아닌 파이썬의 기본 List와 Dict를 사용해도 결과는 동일하게 나온다.

하지만 List와 Dict는 Module이 아니기 때문에 Sub Module로 등록이 되지 않아 클래스 내부에 어떤 Module들이 있는지 파악하기 어렵고 모델 저장시에도 List에 속한 Module들이 저장되지 않는다.

###### 참고
Module에서 다른 Module들을 Sub Module로 등록하는 방법은 [[nn.Module의 __setattr__()]]에 나와있다.
