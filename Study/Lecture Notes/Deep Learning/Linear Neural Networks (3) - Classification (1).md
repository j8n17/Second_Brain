# Softmax Regression

여러 클래스에 대한 조건부 확률(입력 데이터가 주어짐)을 추정하기 위해서는 다중 출력 모델이 필요하다.

다중 출력을 위해서는 많은 affine functions이 필요하다.

각 affine functions에게서 logits을 구할 수 있다.

$$\mathbf{o} = \mathbf{W}^T\mathbf{x} + \mathbf{b}$$

![[Pasted image 20231018194352.png|300]]

여기서 사용하는 주된 접근법은 모델의 출력을 확률로서 생각하는 것이다.

관찰된 데이터의 가능도를 최대화하는 확률을 만들기 위해 파라미터를 최적화한다.
(가능도 최대화 = 관찰된 데이터 $\mathbf{x}$에 대한 정답 클래스 $o_i$일 확률의 최대화)

하지만 logits $o$를 바로 사용할 수는 없다.

- 모든 logits를 더했을 때 1이 되지 않고
- 모든 logits은 음수를 가질 수 있기 때문이다.

이는 확률의 기본 공리를 위반한다.

## Softmax

따라서 logits에 softmax를 취해서 모든 outputs의 합을 1로 만들고, 각각의 output이 양수만 존재하도록 한다.
$$\hat{\mathbf{y}} = softmax(\mathbf{o})\ \ \ where \  \hat{y_j}= \frac{exp(o_j)}{\sum_kexp(o_k)}$$
따라서 한 샘플 데이터 $\mathbf{x}$를 입력받은 모델의 출력 $\hat{\mathbf{y}}$의 각 요소들은 각 클래스에 대한 조건부 확률로 생각할 수 있다.
$$\begin{align} &\hat{y_0} = P(\mathbf{y}=[1,0,0]|\mathbf{x}),\\ &\hat{y_1} = P(\mathbf{y}=[0,1,0]|\mathbf{x}), \\ &\hat{y_2} = P(\mathbf{y}=[0,0,1]|\mathbf{x}) \end{align}$$

또한 softmax는 비선형 함수이지만, 출력은 여전히 affine transformation이므로 softmax regression은 linear model이다.

## Minibatch

여러 데이터(minibatch)에 대한 식을 행렬로 나타내면 다음과 같다.

- minibatch features $\mathbf{X} \in \mathbf{R}^{n \times d}$
- weights $\mathbf{W} \in \mathbf{R}^{d \times q}$
- bias $\mathbf{b} \in \mathbf{R}^{q}$
$$\mathbf{O = XW + b}, \ \mathbf{\hat{Y}} = softmax(\mathbf{O})$$

# Log-Likelihood

먼저 엄밀히 말하자면, 여기서의 가능도는 $P(\mathbf{Y}|\hat{\mathbf{Y}})$ 또는 $P(\mathbf{Y}|\mathbf{X, W, b})$로 $\mathbf{Y}$가 주어진 $\mathbf{X, W, b}$로부터 만들어지는 $\mathbf{\hat{Y}}$의 분포에서 나올 가능도를 뜻한다. 이를 간단히 $P(\mathbf{Y}|\mathbf{X})$로 표현한다.

따라서 n개의 전체 데이터 $\mathbf{X}$가 주어졌을 때 $\mathbf{Y}$일 가능도는 다음과 같이 각 데이터에 대한 가능도의 곱으로 나타낼 수 있다.
$$P(\mathbf{Y}|\mathbf{X}) = \prod^n_{i=1}P(\mathbf{y}^{(i)}|\mathbf{x}^{(i)})$$
$\mathbf{x}^{(i)}$는 i번째 입력데이터,  $\mathbf{y}^{(i)}$는 정답 라벨. 만약 $\mathbf{y}^{(i)} = [1,0,0]$이라면 $P(\mathbf{y}^{(i)}|\mathbf{x}^{(i)}) = \hat{y}_0^{(i)}$이다.


linear regression에서와 마찬가지로 이를 최대화하는 것은 어려우므로(overflow 또는 underflow 문제가 발생할 수 있음) log와 음수를 취한 negative log-likelihood를 최소화하는 파라미터를 찾으면 된다.
$$-log\ P(\mathbf{Y}|\mathbf{X}) = \sum^n_{i=1}-log\ P(\mathbf{y}^{(i)}|\mathbf{x}^{(i)})$$
i번째 데이터의 가능도 $P(\mathbf{y}^{(i)}|\mathbf{x}^{(i)})$는 $\mathbf{y}$의 분포가 $\hat{\mathbf{y}}$의 분포가 같을 때 최대가 된다. *(가능도가 $y$가 $\hat{y}$에서 나올 확률이므로 당연한 얘기이다.)*

따라서 다음과 같이 negative log-likelihood는 두 확률분포의 차이가 없을 때 최소가 되는 Cross-Entropy로 정의할 수 있다.
$$-log\ P(\mathbf{y}^{(i)}|\mathbf{x}^{(i)}) = -\sum^q_{j=1}y_j^{(i)}\ log \ \hat{y}_j^{(i)}$$
>[!info] [Label Smoothing V.S. One-hot Encoding] in Cross-Entropy
>Cross-Entropy 식을 사용하면 일반적인 classification은 원핫인코딩으로 라벨링을 하기 때문에 결국 정답 클래스에 대한 값만 남아 해당 클래스로 예측하도록 학습되고, 만약 Label Smoothing을 사용하면 그 확률분포에 맞는 $\hat{\mathbf{Y}}$로 예측하도록 학습이 된다.

고로, Classification에서 Maximum Likelihood Estimation을 하는 것은 Cross-Entropy를 최소화하는 것과 같으므로 Loss function을 Cross-Entropy로 사용할 수 있다.

$$l(\hat{\mathbf{y}}^{(i)}, \mathbf{y}^{(i)}) = -\sum^q_{j=1}y_j^{(i)}\ log \ \hat{y}_j^{(i)}$$
전체 데이터에 대해 표현하면 다음과 같다.
$$-log \ P(\mathbf{Y}|\mathbf{X}) = \sum^n_{i=1} \ l(\hat{\mathbf{y}}^{(i)}, \mathbf{y}^{(i)})$$

# $o_j$에 대한 Loss 미분

$\mathbf{o} = [o_0, \cdots, o_j, \cdots, o_n]$이고 $\hat{\mathbf{y}} = softmax(\mathbf{o})$, $L = l(\hat{\mathbf{y}}, \mathbf{y})$이므로, 먼저 $\frac{\partial L}{\partial \hat{\mathbf{y}}}$를 계산하고 $\frac{\partial \hat{\mathbf{y}}}{\partial \mathbf{o}}$를 곱하고 $\frac{\partial \mathbf{o}}{\partial o_j}$ 를 곱하는 게 일반적이지만 여기서는 바로 $\frac{\partial L}{\partial o_j}$을 구한다.

먼저 $L$를 다음과 같이 정리할 수 있다.
$$\begin{align} L = l(\hat{\mathbf{y}}^{(i)}, \mathbf{y}^{(i)}) &= -\sum^q_{j=1}y_j^{(i)}\ log \ \frac{exp(o^{(i)}_j)}{\sum^q_{p=1}exp(o^{(i)}_p)} \\ &= log\sum^q_{p=1} \ exp(o^{(i)}_p) - \sum^q_{j=1}y_jo_j \end{align}$$
이를 $o_j$에 대해서 미분하면 다음과 같다.
$$\begin{align} \partial_{o_j}l(\mathbf{y}, \hat{\mathbf{y}}) &= \frac{exp(o_j)}{\sum^q_{p=1}exp(o_p)} - y_j \\ &= softmax(\mathbf{o})_j - y_j \\ &= \hat{y}_j - y_j \end{align}$$
각 $o_j$는 예측한 확률에 실제 확률을 뺀 값을 최소화하는 방향으로 가중치를 업데이트 한다.

즉, label이 0인 경우에는 $o_j$를 작게 하는 방향으로, label이 1인 경우에는 $o_j$를 크게 하는 방향으로 가중치를 업데이트 한다.

# 구현

- pytorch의 CrossEntropyLoss는 target을 정답 인덱스만 줘도 되고, 클래스의 확률로 줘도 된다.
  따라서 predict가 [batch, logits 수]라면 target은 값이 클래스 번호인 경우는 [batch], 값이 확률인 경우는 [batch, logits 수]로 주면 된다.
- 이때 logits을 주는 이유는 CrossEntropyLoss 안에 softmax가 구현되어 있기 때문이다.

- 또한 사용해본 적은 없지만 CrossEntropyLoss에서 label_smoothing=$\alpha$로 입력하면 정답 클래스는 $1-\alpha$, 나머지 클래스는 $\alpha$를 나눠서 uniform distribution으로 라벨을 갖는다.

# 요약

- softmax를 통해 벡터를 확률로 매핑했다.
- Cross-Entropy는 다른 두 확률분포의 차이를 측정하기에 좋다.

### 추가

- 최종적으로는 accuracy로 모델의 성능을 볼 수 있지만, 통계적이고 계산적인 이유(accuracy는 미분이 안됨)로 인해 학습 과정에서는 accuracy보다는 다른 목적함수를 통해 모델을 학습시킨다.