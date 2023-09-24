---
tags:
- Module
- super
- __init__
- __setattr__
---

nn.Module Class를 상속받아 새로운 Layer 또는 Block Class를 정의할 때 super().\_\_init\_\_()을 해야하는 이유는 nn.Module Class의 \_\_init\_\_()에서 정의된 몇가지 속성들(\_parameter, \_modules 등)을  \_\_setattr\_\_()[^1] 에서 체크하고 있기 때문이다.

[^1]: \_\_setattr\_\_()는 `self.attribute = value`와 같이 객체의 속성이 초기화될 때 호출되는 메서드이다.

만약 super().\_\_init\_\_()을 하지 않아 이 속성들을 정의하지 않고 새로운 속성을 초기화하면 \_\_setattr\_\_()에서 AttributeError가 발생하고 "cannot assign parameters before Module.\_\_init\_\_() call"와 같은 오류메세지가 출력된다.

nn.Module에서는 [\_\_init\_\_()](https://stackoverflow.com/questions/63058355/why-is-the-super-constructor-necessary-in-pytorch-custom-modules)을 다음과 같이 정의해서 사용하고 있다.

```python
def __init__(self):
    """
    Initializes internal Module state, shared by both nn.Module and ScriptModule.
    """
    torch._C._log_api_usage_once("python.nn_module")

    self.training = True
    self._parameters = OrderedDict()
    self._buffers = OrderedDict()
    self._non_persistent_buffers_set = set()
    self._backward_hooks = OrderedDict()
    self._forward_hooks = OrderedDict()
    self._forward_pre_hooks = OrderedDict()
    self._state_dict_hooks = OrderedDict()
    self._load_state_dict_pre_hooks = OrderedDict()
    self._modules = OrderedDict()
```

이 속성들은 대부분 OrderedDict()로 초기화되어 있는데 이는 새로운 Module들을 이 속성들에 등록해서 사용하기 때문이다.

###### 참고
1. [[torch.nn.Module]]
2. [[Class - 상속 및 super()의 의미]]