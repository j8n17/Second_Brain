---
tags:
- norm
- vector
- L1_norm
- L2_norm
- Euclidean
- Lp_norm
- Maximum_norm
- Frobenius_norm
- Matrix
- trace
---

norm이란 벡터의 크기를 나타내는 방법이다.
norm은 다음의 3가지 성질을 만족한다.

1. 임의의 벡터 $\mathbf{x}$가 스칼라 $\alpha$로 스케일링 되면 norm도 그에 따라 $|\alpha|$만큼 스케일링된다.$$\|\alpha\mathbf{x}\| = |\alpha|\|\mathbf{x}\|$$
2. 임의의 벡터 $\mathbf{x}, \mathbf{y}$에 대해서 norm은 삼각 부등식을 만족한다.$$ \|\mathbf{x} + \mathbf{y}\| \leq \|\mathbf{x}\| + \|\mathbf{y}\| $$
3. 임의의 벡터 $\mathbf{x}$가 영벡터가 아닐 때, $\mathbf{x}$의 norm은 양수이다.$$\|\mathbf{x}\| > 0 \textrm{ for all } \mathbf{x} \neq 0$$

# L1 norm

L1 norm은 맨해튼 거리(Manhattan distance)라고도 불린다. (맨해튼의 격자 무늬의 골목을 의미한다.)

$$\|\mathbf{x}\|_1 = \sum _{i=1}^n|\mathbf{x_i}|$$

# L2 norm

L2 norm은 벡터의 유클리디안 거리(Euclidean distance)를 측정한다.
$$\|\mathbf{x}\|_2 = \sum _{i=1}^n\mathbf{x_i}^2$$

# Lp Norm

보통 L1, L2 norm을 많이 사용하는데 이는 Lp norm에서 p가 1, 2인 경우이다.
즉, norm을 일반적으로 표현한 것이 Lp norm이다.
$$\|\mathbf{x}\|_p = \left( \sum _{i=1}^n|\mathbf{x_i}|^p\right)^{1/p} $$

# Maximum Norm

p가 무한인 경우의 norm.
임의의 벡터 $\mathbf{x}$의 원소 중 가장 큰 원소의 절대값을 의미한다.
$$\|\mathbf{x}\|_\infty = \lim _{i\to\infty}\left( \sum _{i=1}^n|\mathbf{x_i}|^p\right)^{1/p} =  \textrm{max}(|x_1|, |x_2|, \cdots, |x_n|)$$

# Frobenius norm

행렬도 벡터의 성질을 가지고 있기 때문에 행렬의 크기를 비교할 때 norm을 쓸 수 있다.
이때 행렬의 모든 원소를 제곱해 더하는 Frobenius norm이라는 것을 사용해 행렬의 크기를 비교할 수 있다.

$$\|\mathbf{X}\|_ \textrm{F} = \sqrt{\sum_{i = 1}^m \sum_{j=1}^n x_{i, j}^2}$$

이는 trace를 이용한 아래의 식으로도 나타낼 수 있다.
$$\|\mathbf{X}\|_\textrm{F} = \sqrt{tr(\mathbf{X}^T\mathbf{X})}$$
