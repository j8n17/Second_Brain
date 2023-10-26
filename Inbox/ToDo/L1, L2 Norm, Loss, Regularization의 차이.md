
# Norm
입력값 : 하나의 벡터

L1, L2 Norm은 벡터의 크기를 측정하는 방법으로, 벡터의 각 원소를 이용해 계산한다.

참고 : [[norm]]

# Loss
입력값 : 예측 벡터와 정답 벡터

L1, L2 Loss는 모델의 예측값과 정답값 사이의 오차의 정도를 나타내기 위한 방법이다.

L1는 MAE(Mean Absolute Error), L2는 MSE(Mean Squared Error)라고도 불린다.

L2 Loss는 예측값과 정답값의 차이 벡터인 다차원 벡터의 L2 Norm으로 계산한다. (이전에는 Norm과 달리 벡터가 아닌 스칼라 값을 가지고 계산한다고 생각했다. 하지만 이런 경우는 출력값이 스칼라로 1개인 경우만 해당한다.)

L1 Loss는 예측값 - 정답값의 절댓값으로 계산하고 L2 Loss는 예측값 - 정답값의 제곱으로 계산한다.

L2 Loss는 제곱이므로 이상치에 더 민감하게 반응하고 따라서 L1은 L2에 비해 둔감하게(robust) 반응한다. (L2는 오차가 1보다 작은건 더 작게, 1보다 큰건 더 크게 계산되기 때문.)

둔감하게 반응한다고 해서 안좋은 것이 아니라 데이터에 특성에 따라 노이즈가 많은 데이터라면 L1, 이상치가 중요한 데이터라면 L2를 쓰는 것이 좋다.

# Regularization
입력값 : 하나의 가중치 벡터

Regularization은 모델이 오버피팅 되지 않게 규제 하는 것이다.

그 중에서 가중치 감쇠(Weight Decay)라는 기법이 있는데 여기에 L1, L2 Regularization이 포함된다.

먼저 가중치 감쇠는 Loss Function에 가중치 term을 추가해 가중치들이 작아지는 방향으로 학습을 시키는 기법이다.

이런 방식으로 모델을 학습시키면 가중치가 커지는 것을 방지해 모델이 오버피팅 되는 것을 방지할 수 있다.

L1 Regularization은 Loss에 가중치들의 절댓값의 합으로 추가되고, L2 Regularization은 Loss에 가중치들의 제곱의 합으로 추가된다.

그렇기에 여기서 norm, loss에서의 L1, L2 와는 특히 다른 특성을 확인할 수 있는데, 그 이유는 미분에 있다.

먼저 미분의 분배법칙에 의해 Loss에 Regularization term을 추가해도 Loss에 대한 미분은 똑같이 진행되고 거기에 Regularization term에 대한 미분값만 추가되므로 연산 자체는 간단하다는 것을 알고 넘어가자. 이를 수식으로 표현하면 다음과 같다.

$$ J = Loss + \lambda Reg$$
$$\frac{\partial{J}}{\partial{w_i}} = \frac{\partial{L}}{\partial{w_i}} + \lambda\frac{\partial{Reg}}{\partial{w_i}}$$

L1 Regularization의 경우, Regularization term이 $w_i$에 의해 미분되면서 $w_i$의 부호만 남게 된다. (sign함수는 부호만 남는 함수.)

$$\frac{\partial{Reg}}{\partial{w_i}} = \frac{\partial{}}{\partial{w_i}}\sum_{j=0}^m|w_j| = \textrm{sign}(w_i)$$
고로 $w_i$가 업데이트 될 때는 다음과 같이 업데이트 된다. L1의 경우 $\lambda$가 아닌 $\alpha$를 쓴다.

$$w_i = w_i - \eta\frac{\partial{J}}{\partial{w_i}} = w_i - \eta\left[\frac{\partial{L}}{\partial{w_i}} + \alpha \ \textrm{sign}(w_i)\right]$$

L2 Regularization의 경우, Regularization term이 $w_i$에 의해 미분되면서 $w_i$가 남게 된다.

$$\frac{\partial{Reg}}{\partial{w_i}} = \frac{\partial{}}{\partial{w_i}}\sum_{j=0}^n w_j^2 = 2w_i$$
고로 $w_i$가 업데이트 될 때는 다음과 같이 업데이트 된다.

$$w_i = w_i - \eta\frac{\partial{J}}{\partial{w_i}} = w_i - \eta\left[\frac{\partial{L}}{\partial{w_i}} + 2 \lambda w_i \right]$$


L1 Regularization이 가중치를 0으로 만들 수 있는 이유는 가중치가 0에 가까워져도 가중치 규제에 관한 업데이트는 상수($\eta \ \alpha \ \textrm{sign}w_i$)로 하기 때문이고,
L2 Regularization이 가중치를 0으로 만들 수 없는 이유는 가중치가 0에 가까워지면 규제항($2 \eta \lambda w_i$)도 사라져 가중치 규제에 대한 업데이트가 진행되지 않기 때문이다.


그래프에 관한 얘기
https://gaussian37.github.io/dl-concept-regularization/
https://mole-starseeker.tistory.com/34
https://yurmu.tistory.com/20
