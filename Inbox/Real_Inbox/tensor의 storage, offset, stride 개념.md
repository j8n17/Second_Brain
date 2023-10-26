
https://hiddenbeginner.github.io/deeplearning/2020/01/21/pytorch_tensor.html#ref2

view와 slicing, 심지어 transpose에서도 이 개념을 사용하는 것을 알 수 있다.

torch.mul은 element wise 곱 연산인데 storage 순으로 곱하는 것 같음.
a = torch.randn(4, 2)
b = torch.randn(2, 4)
torch.mul(a, b)는 안되고 torch.mul(b, b)는 됨.
