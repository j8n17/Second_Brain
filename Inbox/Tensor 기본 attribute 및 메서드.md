---
tags:
- 
---

# attribute

- shape : tensor의 형태 반환
- 


# method

- numel() : tensor의 원소의 개수 반환
- [a:, b:] : 슬라이싱. tensor[a:]\[b:]와 같은 방식으로 슬라이싱을 하면 tensor[a:]의 결과에서 [b:]를 진행하므로 다른 결과를 반환한다.
- exp() : 모든 원소에 exponential을 취한다.
- numpy() : ndarray로 변환. 입력된 tensor와 반환된 ndarray는 메모리를 공유.
- reshape(shape), view(shape) :
- transpose(dim1, dim2) : 
- 

# Operations
## Saving Memory
- Y = X + Y는 새로운 메모리를 할당.
- Y[:] = X + Y 또는 Y += X를 사용해야 메모리를 재사용함.
