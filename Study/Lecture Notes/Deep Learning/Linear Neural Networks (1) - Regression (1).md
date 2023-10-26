
회귀는 하나 이상의 독립변수(x)와 종속변수(y)에 대한 관계를 모델링하는 일련의 방법이다.

회귀는 하나의 실수값을 예측하는 것이고, 분류는 여러 클래스의 집합을 예측하는 것이다.

---
### 몇가지 가정

- 선형 회귀(Linear Regression)는 독립변수들 $x$와 종속변수 y는 선형적인 관계를 가지고 있다
- 노이즈는 가우시안 분포를 따른다.

### 데이터셋 구성

집값 예측을 하기 위한 모델을 만들 때, 해당 정보가 있는 데이터셋이 필요하다.

이런 데이터셋은 training dataset 또는 training set으로 부른다.

각 행(row)은 sample이라 한다.

예측하고자 하는 값을 label 또는 target이라 한다.

나이, 지역과 같은 독립 변수(independent variables)는 features 또는 covariates라고 부른다.

# Linear Model

선형성 가정에 의해 다음과 같이 표현할 수 있다.

$$ y = w_1 \cdot x_1 + w_2 \cdot x_2 + b$$

이는 입력들의 가중합을 통한 선형변환과 바이어스를 통한 변환이 결합된 affine transformation이다.

이런 affine transformation으로 예측을 할 수 있는 모델을 Linear Model이라고 한다.

이때, $w_1, w_2$를 가중치(weight), $b$를 바이어스(bias or offset or intercept)라고 한다.

이를 가중치($\mathbf{w} \in \mathbf{R}^d$)와 입력데이터($\mathbf{x} \in \mathbf{R}^d$)의 벡터곱을 이용해 모델의 예측값($\hat{y} \in \mathbf{R}^1$)을 나타내면 다음과 같다.

$$\hat{y} = \mathbf{w}^T\mathbf{x} + b$$

## 행렬 표현

이는 하나의 데이터에 대한 예측이므로 여러 데이터($\mathbf{X} \in \mathbf{R}^{n \times d}$)에 대한 모델의 예측값($\hat{\mathbf{y}}\in \mathbf{R}^{n}$)을 행렬곱으로 나타내면 다음과 같다.

$$\hat{\mathbf{y}} = \mathbf{X}\mathbf{w} + \mathbf{b}$$

이때 $\mathbf{b}$는 모두 같은 $b$로 이뤄진 벡터이다. (고로 pytorch에서 계산시 브로드캐스팅을 활용한다.)

![[Pasted image 20231018194453.png|300]]

### Linear Regression의 목표

학습 데이터셋 $X$와 label $y$가 주어졌을때, Linear Regression의 목표는 $X$와 같은 분포에서 온 새로운 데이터에서도 적은 오차로 예측하는 $w$와 $b$를 찾는 것이다.
($y$가 정확하게 $w^Tx + b$와 모두 맞아떨어지는 데이터는 없다. 따라서 입력과 출력간의 관계가 선형적이라고 확신하더라도 noise term이 필요하다.)

하지만 최적의 파라미터를 찾기 전에,

1. 모델의 평가 방법
2. 모델 업데이트 방법

이 필요하다.

