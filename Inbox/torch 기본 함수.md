==tensor 생성 부분의 random sampling 부분이 있어서 수정 해야한다.==
# [Tensors](https://pytorch.org/docs/stable/torch.html#tensors)
## Creation Operations

## shape 기준
- torch.rand(shape) : 0에서 1사이의 수로 채워진 랜덤한 tensor를 생성.
- torch.randn(shape) : 평균이 0이고 분산이 1인 가우시안 분포에서 뽑은 원소로 이뤄진 tensor 생성.
- torch.randint(start, end, size)) : start부터 end까지의 정수로 채워진 랜덤한 tensor를 생성.
- torch.zeros(shape) : tensor의 shape을 입력받아 0으로 이뤄진 tensor 생성.
- torch.ones(shape) : 1로 이뤄진 tensor 생성.
### \_like() 함수
입력 텐서와 동일한 shape인 텐서를 앞에서 설명한 방식처럼 생성.
- torch.rand_like()
- torch.randn_like()
- torch.randint_like()
- torch.zeros_like()
- torch.ones_like()
https://subinium.github.io/pytorch-Tensor-Variable/

## 데이터 기준
- torch.arange(num) : 0부터 num-1 까지의 원소로 이뤄진 1d tensor를 반환한다.
- torch.tensor(data) : list 또는 ndarray를 입력받아 tensor 생성.
- torch.from_numpy(ndarray) : ndarray를 입력받아 tensor 생성. 입력된 ndarray와 반환된 tensor는 메모리를 공유.
- torch.as_tensor(data): list 또는 ndarray를 입력받아 tensor 생성.

## Indexing, Slicing, Joining, Mutating Operations

# Operations
- torch.exp(tensor) : tensor의 모든 원소에 exponential을 취한다.
- torch.cat(tuple, dim) : tuple 내의 tensor를 dim을 기준으로 concat한다.
- chunk
- swapdims
- scatter_

# [Random Sampling](https://pytorch.org/docs/stable/torch.html#random-sampling)

- randn
- randperm
- poisson

# [Math Operations](https://pytorch.org/docs/stable/torch.html#math-operations)
## Pointwise Ops
- log1p
- rad2deg
- clamp

## Reduction Ops

- prod
- count_nonzero
- argmax

## Comparison Ops

- allclose
- argsort
- topk

## Other Ops
- triu
- einsum
- bucketize

## BLAS and LAPACK Ops
BLAS - Basic Linear Algebra Subprograms
LAPACK - Linear Algebra PACKage

- addmm
- matrix_rank
- qr

