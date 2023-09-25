
# Norm
입력값 : 벡터

L1, L2 Norm은 벡터의 크기를 측정하는 방법으로, 벡터의 각 원소를 이용해 계산한다.

참고 : [[norm]]

# Loss
입력값 : 스칼라

L1, L2 Loss는 모델의 예측값과 정답값 사이의 오차의 정도를 나타내기 위한 방법이다.

L1는 MAE(Mean Absolute Error), L2는 MSE(Mean Squared Error)라고도 불린다.

Loss는 Norm과 달리 벡터가 아닌 스칼라 값을 가지고 계산한다.

L1 Loss는 예측값 - 정답값의 절댓값으로 계산하고 L2 Loss는 예측값 - 정답값의 제곱으로 계산한다.

L2 Loss는 제곱이므로 이상치에 더 민감하게 반응하고 따라서 L1은 L2에 비해 둔감하게(robust) 반응한다.

둔감하게 반응한다고 해서 안좋은 것이 아니라 데이터에 특성에 따라 노이즈가 많은 데이터라면 L1, 이상치가 중요한 데이터라면 L2를 쓰는 것이 좋다.

# Regularization
입력값 : 가중치 벡터

Regularization은 모델이 오버피팅 되지 않게 규제 하는 것이다.

그 중에서 가중치 감쇠(Weight Decay)라는 기법이 있는데 여기에 L1, L2 Regularization이 포함된다.

먼저 가중치 감쇠는 Loss Function에 가중치 term을 추가해 가중치들이 작아지는 방향으로 학습을 시키는 기법이다.

이런 방식으로 모델을 학습시키면 가중치가 커지는 것을 방지해 모델이 오버피팅 되는 것을 방지할 수 있다.

L1 Regularization은 Loss에 가중치들의 절댓값의 합으로 추가되고, L2 Regularization은 Loss에 가중치들의 제곱의 합으로 추가된다.

그렇기에 여기서 norm, loss에서의 L1, L2 와는 다른 특성을 확인할 수 있는데, 그 이유는 미분에 있다.

L1 Regularization의 경우, Loss가 각 $w_i$에 의해 미분에 의해 미분되면서 다른 