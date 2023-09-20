https://pytorch.org/docs/stable/generated/torch.nn.Module.html?highlight=register_buffer#torch.nn.Module.register_buffer

Parameter는 Tensor와 달리 Gradient를 계산을 통해 값을 업데이트할 수 있고, 저장도 가능하다.

하지만 [[BatchNorm]]과 같은 경우, 값을 업데이트할 필요는 없지만 저장할 수는 있어야 한다.
이런 경우에는 Buffer를 사용할 수 있다.

register_buffer는 Module 내에서 Tensor를 Buffer로 등록시켜 저장할 수 있게 하는 클래스(?)함수이다.

# register_buffer와 register_parameter_of_module의 차이
https://discuss.pytorch.org/t/what-is-the-difference-between-register-buffer-and-register-parameter-of-nn-module/32723

```python
self.register_buffer('buffer', torch.Tensor([7]), persistent=True)
```