module이 어떻게 구성되었는지 - https://pytorch.org/docs/stable/generated/torch.nn.Module.html

module이 어떤 내용으로 구성되었는지 - https://pytorch.org/docs/stable/notes/modules.html

nn.Module 클래스는 여러 function을 조합하는 상자 역할을 한다.

- nn.Module에 function을 모아놓으면 basic building block
- nn.Module에 basic building block인 nn.Module을 모아놓으면 딥러닝 모델
- nn.Module에 딥러닝 모델인 nn.Module을 모아놓으면 더 큰 딥러닝 모델

이렇게 nn.Module을 어떻게 사용할 지는 설계자의 몫이다.

# Module

nn.Module을 상속받아 새로운 클래스를 만들 때 super().\_\_init__()을 해야하는 이유 - https://stackoverflow.com/questions/63058355/why-is-the-super-constructor-necessary-in-pytorch-custom-modules


# Container
https://pytorch.org/docs/stable/nn.html#containers


nn.Sequential - https://pytorch.org/docs/stable/generated/torch.nn.Sequential.html#torch.nn.Sequential
Module들을 순차적으로 실행시킴.

nn.ModuleList - https://pytorch.org/docs/stable/generated/torch.nn.ModuleList.html#torch.nn.ModuleList
Module들을 인덱싱을 통해 호출해 실행시킴.

nn.ModuleDict - https://pytorch.org/docs/stable/generated/torch.nn.ModuleDict.html#torch.nn.ModuleDict
Module들을 key값을 통해 호출해 실행시킴.

파이썬의 List와 Dict가 있는데 굳이 ModuleList, ModuleDict를 쓰는 이유는 이들은 \_\_setattr__ 메서드를 통해 각 ModuleList가 클래스 내부에서 정의될 때 submodule로 등록이 되기 때문이다.
이런 submodule들은 클래스 외부에서 쉽게 확인할 수 있다.
파이썬의 List와 Dict로 구현할 경우 동작은 완전히 똑같지만 submodule로 등록이 되지 않기 때문에 클래스 외부에서 내부에 어떤 Module들이 있는지 파악하기 쉽지 않고 torch.save를 통한 모델 저장시에도 해당 Module들이 저장되지 않는다.

