# Loss Function

모델이 데이터에 얼만큼 잘 맞는지를 fitness라고 함. 즉, 우리는 모델을 데이터에 fitting 시키는 것임.

Loss function은 실제 값과 예측 값의 거리이다.

Regression 문제에서 가장 흔한 loss function은 squared error이다.

$$loss^{(i)}(W, b) = \frac{1}{2}(\hat{y}^{(i)} - y^{(i)})^2$$

특히 MSE Loss는 제곱을 하기 때문에 작은 오차는 더 작게, 큰 오차는 더 크게 계산되므로 큰 오차를 줄이는 방향 위주로 학습된다.

여기서 $(i)$는 i번째 데이터를 의미하고, 1/2로 나누는 이유는 단순히 미분했을 때 계수를 없애기 위함임.

그런데 우리는 전체 데이터에 대해서 모델의 성능을 평가해야 하므로 다음과 같이 전체 데이터셋의 대한 Loss 값의 평균을 구한다.

$$L(W,b) = \frac{1}{n}\sum^n_{i=1}l^{(i)}(W, b)$$

모델을 학습시킬 때는 모든 학습 데이터에 대한 Loss를 최소화하는 $W, b$를 찾는다.

$$W^*, b^* = argmin_{W,b}\ L(W, b)$$
# Minibatch Stochastic Gradient Descent

Linear Regression Model은 Loss를 최소화하는 미분이 0이 되는 지점을 수학적인 솔루션(analytic solution)을 통해 얻을 수 있다.

하지만 수학적인 솔루션을 구할 수 없는 딥러닝 모델들은 Gradient Descent라는 핵심 기법을 통해 최적화할 수 있다는 것이 밝혀졌다.

Gradient Descent는 Loss를 점진적으로 줄이는 방향으로 가중치를 업데이트해서 오차를 줄인다.

가장 쉬운 방법은 모든 데이터셋에 대해서 하는 것이지만, 너무 오래 걸린다.

따라서 업데이트 할 때마다 random하게 minibatch를 뽑아 학습한다. 이를 Minibatch Stochastic Gradient Descent라고 한다.

$$(w, b) \gets (w, b) - \frac{\eta}{|B|} \sum_{i\in B} \partial_{(w,b)}l^{(i)}(w,b)$$

mini batch의 데이터에 대한 평균 Loss를 구하고 미분한 후 학습률($\eta$)과 곱해서 가중치에서 뺀다.

가중치에서 빼는 것은 Loss를 미분한 것은 Loss가 증가하는 방향이므로 감소하는 방향으로 업데이트하기 위함이다.

이때, 학습률과 batch_size 등은 수동적으로 정하는데, 이렇게 학습과정으로 통해 업데이트 되지 않는 파라미터들을 하이퍼 파라미터라고 한다.

### Hyperparameter Tuning

Hyperparameter Tuning은 Validation Dataset을 이용해 모델을 평가해 최적의 하이퍼 파라미터를 찾는 과정이다.

### Generalization

딥러닝의 Loss는 많은 최소값(minima)들을 가지고 있다.

많은 사람들은 Training sets에 대한 Loss를 최소화하는 파라미터를 찾지만, 더 중요한 일은 보지 않은 데이터에 대한 Loss를 최소화하는 것이다.

이를 일반화(Generalization)라고 한다.

### Prediction or Inference

학습된 모델을 통해 얻은 입력에 대한 추정값은 Prediction 또는 Inference라고 한다.


### Vectorization

주어진 mini batch 샘플을 동시에 처리하기 위해 벡터 연산을 사용한다.

# The Normal Distribution and Squared Loss

Linear Regression은 노이즈가 가우시안 분포를 따른다고 가정한다.

따라서 다음과 같이 식을 쓸 수 있다.
$$y - \hat{y} = y - \mathbf{w}^T\mathbf{x} + b = \epsilon \sim N(0, \sigma^2)$$
가우시안 분포의 확률 함수는 다음과 같다.
$$p(x) = \frac{1}{\sqrt{2\pi\sigma^2}}exp(-\frac{1}{2\sigma^2}(x-\mu))^2)$$
따라서 Linear Regression에서의 $x$가 주어질 때 $y$의 확률인 가능도(likelihood)는 다음과 같이 구할 수 있다.
$$P(y|\mathbf{x}) = \frac{1}{\sqrt{2\pi\sigma^2}}exp(-\frac{1}{2\sigma^2}(y-\mathbf{w}^T\mathbf{x}-b)^2)$$
이제 최대 우도 추정(Maximum Likelihood Estimation)에 의해, 전체 데이터에 대한 Likelihood를 최대화하는 $w$와 $b$가 가장 좋은 성능을 낼 수 있다.

전체 데이터에 대한 Likelihood는 다음과 같다.
$$P(\mathbf{y}|\mathbf{X}) = \prod^n_{i=1}p(y^{(i)}|x^{(i)})$$
이는 많은 exponential 함수의 곱으로 이루어져 있어 계산하기 힘들다.

그래서 log를 씌운 log-likelihood로 바꿔 덧셈 연산으로 바꾼다.

또한, 우리는 Loss를 최소화하는 파라미터를 찾으므로 negative log-likelihood를 최소화하는 파라미터를 찾는다. negative log-likelihood는 다음과 같다.
$$-log \ P(\mathbf{y}|\mathbf{X}) = \sum^n_{i=1}\frac{1}{2}log(2\pi\sigma^2) + \frac{1}{2\sigma^2}\left(y^{(i)} - w^Tx^{(i)} - b\right)^2$$
이 식에서 첫번째 항은 고정되어 있으므로 두번째 항을 최소화하는 파라미터를 찾는 것이다.

두번째 항은 $\frac{1}{\sigma^2}$를 제외하고 MSE(Mean Squared Error)와 일치하고, $\frac{1}{\sigma^2}$는 최적화 하는 것과 상관이 없으므로

> 가우시안 노이즈의 가정 하에 Maximum Likelihood Estimation을 하는 것은 MSE를 최소화하는 것과 동치이다.

# Linear Layer

Network의 Layer 수를 셀 때는 입력층은 빼고 센다.

Linear Regression에서는 모든 입력이 모든 출력과 연결되어 있다. 이런 형태를 Fully-Connected Layer 또는 Dense Layer라고 부른다.