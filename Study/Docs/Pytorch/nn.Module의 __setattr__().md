---
tags:
- __setattr__
- Module
alias:
- __setattr__()
---

nn.Module의 \_\_setattr\_\_()에서는 아래의 코드들에서 초기화되는 속성명(name)이 이미 등록 되어 있는 속 성명인지, 값(value)이 어떤 값인지(Parameter인지, Module인지 등)를 확인한 후 초기화하는 과정을 거친다.

```python
# nn.Module __init__()에서의 self.name = value의 형태

self.param_1 = nn.Parameter(10)
self.fc = nn.Linear(10, 20)
self.num = 10

# 예를 들어 param_1(name)이 _parameter에 등록되어 있는지,
# nn.Parameter(10)(value)가 Parameter인지 확인한 후 초기화 한다.
```

이때 super().\_\_init\_\_()을 통해 nn.Module가 제대로 초기화되지 않았다면 해당 객체의 '\_parameters', '\_modules' 등의 필수 속성이 초기화되지 않았으므로 오류가 발생한다. [[새로운 Layer를 만들 때 super().__init__()을 호출해야 하는 이유|super().__init__()을 호출해야 하는 이유]]

아래 코드는 nn.Module의 \_\_setattr\_\_() 코드이다.

```python
def __setattr__(self, name: str, value: Union[Tensor, 'Module']) -> None:
        def remove_from(*dicts_or_sets):
            for d in dicts_or_sets:
                if name in d:
                    if isinstance(d, dict):
                        del d[name]
                    else:
                        d.discard(name)

        params = self.__dict__.get('_parameters')
        if isinstance(value, Parameter):
            if params is None:
                raise AttributeError(
                    "cannot assign parameters before Module.__init__() call")
            remove_from(self.__dict__, self._buffers, self._modules, self._non_persistent_buffers_set)
            self.register_parameter(name, value)
        elif params is not None and name in params:
            if value is not None:
                raise TypeError("cannot assign '{}' as parameter '{}' "
                                "(torch.nn.Parameter or None expected)"
                                .format(torch.typename(value), name))
            self.register_parameter(name, value)
        else:
            modules = self.__dict__.get('_modules')
            if isinstance(value, Module):
                if modules is None:
                    raise AttributeError(
                        "cannot assign module before Module.__init__() call")
                remove_from(self.__dict__, self._parameters, self._buffers, self._non_persistent_buffers_set)
                modules[name] = value
            elif modules is not None and name in modules:
                if value is not None:
                    raise TypeError("cannot assign '{}' as child module '{}' "
                                    "(torch.nn.Module or None expected)"
                                    .format(torch.typename(value), name))
                modules[name] = value
            else:
                buffers = self.__dict__.get('_buffers')
                if buffers is not None and name in buffers:
                    if value is not None and not isinstance(value, torch.Tensor):
                        raise TypeError("cannot assign '{}' as buffer '{}' "
                                        "(torch.Tensor or None expected)"
                                        .format(torch.typename(value), name))
                    buffers[name] = value
                else:
                    object.__setattr__(self, name, value)
```

위 코드를 보면 attribute가 초기화 될 때 다음과 같은 단계로 \_parameters, \_modules, \_buffers 에 대해 동작한다.

1. 값(value)이 Parameter이고 nn.Module이 정상적으로 초기화 됐다면, 다른 속성(\_\_dict\_\_, \_modules, \_buffers, \_non_persistent_buffers_set)에 등록된 혹시 모를 같은 속성명(name)을 삭제한 후 \_parameter에 해당 값을 등록한다.
2. \_parameter에 해당 속성명(name)이 이미 등록되어 있다면 해당 속성의 값을 초기화한다.
3. 값도 Parameter가 아니고 속성명도 \_parameter에 등록되어 있지 않다면 Module 확인으로 넘어간다.

Module, Buffer도 위와 같은 과정으로 속성명과 값을 확인하고 모두 아니라면 최상위 class인 object의 속성으로 초기화한다.(self.num = 10과 같은 경우)