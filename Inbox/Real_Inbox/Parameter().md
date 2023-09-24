https://pytorch.org/docs/stable/generated/torch.nn.parameter.Parameter.html?highlight=parameter

Tensor와 Parameter의 차이
왜 Parameter를 써야하는지

forward까지는 동일하게 작동하지만 Parameter는 Module의 속성으로 할당될때 자동적으로 Module Parameter 리스트에 등록되므로 back progation시 gradient를 계산해 값을 업데이트 할 수 있고, 모델 저장시에도 값들을 쉽게 가져와 저장할 수 있게 한다.

하지만 대부분의 경우 이미 만들어진 클래스를 사용하기 때문에 Parameter 클래스를 다룰 일은 많지 않다.
