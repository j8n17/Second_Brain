---
tags:
  - dot
  - mv
  - mm
  - matmul
  - 행렬곱
  - mul
---

>[!info] torch.mul() - 브로드캐스팅을 통한 행렬간의 <u>element wise 곱</u> 연산 (행렬곱X)


- torch.dot() - 2개의 1D tensor(벡터)의 내적(dot product) 연산
- torch.mv() - 2D tensor(행렬)과 1D tensor(벡터)의 행렬곱(matrix product) 연산
- torch.mm() - 2개의 2D tensor(행렬)의 행렬곱 연산
- torch.matmul() - 브로드캐스팅을 지원하는 행렬곱 연산

# torch.dot(), torch.mv(), torch.mm()

```python
>>> mat = torch.randn(3, 4)
>>> vec = torch.randn(4)
>>> mat2 = torch.randn(4, 5)

# torch.dot
>>> torch.dot(vec, vec)
tensor(3.1581)
>>> torch.dot(vec, vec).shape
torch.Size([])

# torch.mv
>>> torch.mv(mat, vec).shape
torch.Size([3])

# torch.mm
>>> torch.mm(mat, mat2).shape
torch.Size([3, 5])
>>> torch.mm(mat, vec)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
RuntimeError: mat2 must be a matrix

# torch.matmul
>>> torch.matmul(mat, vec).shape
torch.Size([3])
```

# torch.matmul()

```python
# vector x vector, matrix x matrix, matrix x vector
# 생략

# broadcasted vector x matrix
>>> broadcasted_vec = torch.randn(4)
>>> mat = torch.randn(4, 5)
>>> torch.matmul(broadcasted_vec, mat).shape # vec의 차원을 1x4로 늘려서 1x5로 계산 후 다시 줄인다.
torch.Size([5])

# batched matrix x broadcasted vector (순서 반대면 1x4로 브로드캐스팅 후 계산)
>>> batched_mat = torch.randn(2, 3, 4)
>>> broadcasted_vec = torch.randn(4)
>>> torch.matmul(batched_mat, broadcasted_vec).shape
torch.Size([2, 3])

# batched matrix x batched matrix
>>> batched_mat = torch.randn(2, 3, 4)
>>> batched_mat2 = torch.randn(2, 4, 5) # 배치 사이즈가 동일해야 함
>>> torch.matmul(batched_mat, batched_mat2).shape
torch.Size([2, 3, 5])

# batched matrix x broadcasted matrix (순서 반대도 됨)
>>> batched_mat = torch.randn(2, 3, 4)
>>> broadcasted_mat = torch.randn(4, 5)
>>> torch.matmul(batched_mat, broadcasted_mat).shape
torch.Size([2, 3, 5])
```

# tensor 메서드

이 연산들은 모두 tensor의 메서드로도 사용할 수 있다.

```python
>>> vec.dot(vec)
>>> mat.mv(vec)
>>> mat.mm(mat2)
>>> mat.matmul(mat2)
>>> ...
```
