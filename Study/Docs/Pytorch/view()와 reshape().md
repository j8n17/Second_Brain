---
tags:
- view
- reshape
---

view()와 reshape()은 입력된 행렬의 형태를 각 원소들의 순서를 유지하며 변경하므로 기본적인 동작은 동일하다.

# 차이점

하지만 view()는 [[contiguous]]한 객체만 입력받을 수 있다. 
contiguous 하지 않은 객체의 형태를 변경하려면 reshape()을 사용해야 한다.

contiguous한 객체란 원소들의 순서(행렬에서 왼쪽에서 오른쪽, 위에서 아래 순서)와 메모리 주소 순서가 일치하는 객체를 뚯한다.

따라서 둘다 입력된 원소들의 순서는 유지하지만, view()는 contiguity를 보장하고 reshape은 보장하지 않는다.

```python
>>> a = torch.randn(3, 4)
>>> a_t = a.transpose(0, 1)
>>> a_t.reshape(4, 3)
>>> a_t.view(4, 3)
```

위 코드를 실행시켜보면 a_t는 transpose 되었기 때문에 contiguous 하지 않으므로 view()는 오류가 발생함을 확인할 수 있다.

# 추가

1. 그렇다고 reshape의 반환값이 contiguous 하지 않은 것은 아니다.
   contiguous 하지 않은 행렬을 입력받아도 새로운 메모리 주소를 할당받기 때문에 reshape의 반환값은 언제나 contiguous 하다.
2. torch의 reshape과 numpy의 reshape은 동일하게 동작한다.