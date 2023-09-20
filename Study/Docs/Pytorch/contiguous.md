---
tags:
- contiguous
- is_contiguous
---

contiguous한 객체란 원소들의 순서(행렬에서 왼쪽에서 오른쪽, 위에서 아래 순서)와 메모리 주소 순서가 일치하는 객체를 뚯한다.

Tensor 객체를 transpose하면 행이 열로, 열이 행으로 원소의 순서가 변경되기 때문에 기존 메모리 주소 순서와 일치하지 않는다.

아래 코드를 보면 기존 tensor는 원소 순으로 메모리 주소를 출력했을 때 메모리 주소 순서가 일치하지만, transpose 시킨 tensor는 메모리 주소 순서가 일치하지 않음을 확인할 수 있다.

```python
>>> a = torch.randn(2, 3)
>>> a
tensor([[-0.4903, -0.8172,  1.1111],
        [ 0.7116, -0.5329,  0.8261]])
>>> for i in range(a.shape[0]):
...     for j in range(a.shape[1]):
...         print(a[i, j].data_ptr())
...
5662829440
5662829444
5662829448
5662829452
5662829456
5662829460
>>> a.transpose_(0, 1)
tensor([[-0.4903,  0.7116],
        [-0.8172, -0.5329],
        [ 1.1111,  0.8261]])
>>> for i in range(a.shape[0]):
...     for j in range(a.shape[1]):
...         print(a[i, j].data_ptr())
...
5662829440
5662829452
5662829444
5662829456
5662829448
5662829460
```

# is_contiguous()
is_contiguous()는 tensor의 contiguous함을 확인할 수 있는 메서드이다.
```python
>>> a = torch.randn(2, 3)
>>> a.is_contiguous()
True
>>> a.transpose_(0, 1)
tensor([[ 0.3978, -1.7392],
        [ 0.3080,  2.9988],
        [ 0.1620, -0.8751]])
>>> a.is_contiguous()
False
```

# contiguous()
contiguous()는 메모리를 새로 할당해 contiguous한 tensor를 반환하는 메서드이다.
이 메서드는 inplace 연산이 없다.

아래 코드를 보면 기존 tensor의 메모리 주소와 contiguous()를 적용한 tensor의 메모리 주소가 다르다.
```python
>>> a[0,0].data_ptr()
5645286656
>>> a.contiguous()[0,0].data_ptr()
5107631744
```

