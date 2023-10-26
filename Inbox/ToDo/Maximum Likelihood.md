# Likelihood

Likelihood란, 데이터가 특정 분포로부터 만들어졌을 확률을 뜻한다.

예를 들어, $X = (1, 1, 1, 1, 1)$이라면 이는 정규분포보다는 1일 확률이 100%인 분포를 따를 확률이 높을 것이다.

이때 1일 확률이 100%인 분포의 데이터 X에 대한 likelihood가 더 높다고 표현한다.

# Maximum Likelihood

매개 변수를 모르는 상황에서 데이터들이 주어질 때, 이 데이터를 발생시킬 확률을 최대로 하는 매개변수를 추정하는 문제.
$$\hat{\theta} = \underset{\theta}{\mathrm{argmax}} P(\mathbf{X}|\theta)$$
모든 데이터 $x_i$가 독립적이라고 가정하면 다음과 같이 사용할 수 있다.
$$\hat{\theta} = \underset{\theta}{\mathrm{argmax}} P(\mathbf{X}|\theta) = \underset{\theta}{\mathrm{argmax}} \ \prod^N_{i=1} P(\mathbf{x_i}|\theta)$$

# Log Likelihood

log 함수는 monotonic increasing function(단조 증가 함수; x가 증가하면 y도 증가)이므로 최대 우도를 구하는 것과 최대 로그 우도를 구하는 것은 동일하다.

또한 모든 데이터 $x_i$가 독립적이라고 가정한다면, log를 통해 곱 연산을 합 연산으로 변환할 수 있다.

딥러닝에서 목적함수로서 최적화하기 위해서는 미분을 해야하는데 Miximum Likelihood와 같이 식이 곱셈으로 이루어져 있으면 미분을 적용하기 쉽지 않다. 하지만 Log Likelihood를 사용하면 식이 여러 항의 합으로 이루어져 있으므로 쉽게 미분할 수 있다.

$$\hat{\theta} = \underset{\theta}{\mathrm{argmax}} \ \mathrm{log}P(\mathbf{X}|\theta) = \underset{\theta}{\mathrm{argmax}} \ \sum^n_{i=1}\mathrm{log}P(\mathbf{x}_i|\theta)$$
# Classification

Classification에서는 데이터 $x$가 주어졌을 때 $y$가 특정 클래스일 확률을 통해 클래스를 분류한다.

이를 학습시키기 위해서는 데이터가 주어졌을 때, 해당 데이터의 정답 레이블일 확률을 최대화하는 방향으로 파라미터를 업데이트하면 된다.

여기서 Log Likelihood가 사용된다.

우리는 미니배치를 통해 여러 데이터를 한번에 입력하므로 여러 데이터가 주어졌을 때의 상황을 가정하자.

여러 데이터($x^{(i)}$는 데이터 1개)가 주어졌을 때, y가 해당 데이터의 정답 레이블일 확률을 표현하면 다음과 같다.
$$P(Y|X) = \prod^n_{i=1} P(y^{(i)} | x^{(i)})$$
Likelihood인 이를 최대화하는 것은 Log Likelihood를 최대화하는 것과 같고, 딥러닝에서는 최적화를 위해 목적함수를 최소화해야 하므로 $-\mathrm{log}P(Y|X) = \sum^n_{i=1}-\mathrm{log}P(y^{(i)}|x^{(i)})$를 최소화하는 것으로 생각할 수 있다.

이 식은 다시 다음과 같이 정리할 수 있다.
$$-\mathrm{log}P(Y|X) = \sum^n_{i=1}-\mathrm{log}P(y^{(i)}|x^{(i)}) = \sum^n_{i=1}\left( -\sum^q_{j=1} y_j \ \log\hat{y}_j \right)^{(i)}$$
이는 결국 하나의 데이터에 대한 cross entropy loss를 구해서 모두 더한 것과 같다.

* cross entropy loss : $-\sum^q_{j=1} y_j \ \log\hat{y}_j$
* $y$는 원핫인코딩으로 $y_k$가 1이면 나머지는 모두 0이므로 결국 $- \mathrm{log}\hat{y}_k$가 된다.

>[!note] 
> $P(Y|X)$가 가능도인 이유는 사실 $P(Y|\hat{Y})$가 가능도인데 $P(Y|X)$와 $P(Y|\hat{Y})$는 동치이기 때문.

==이게 맞는건가? 동치는 아닐 수도? P(Y|X)를 가능도라고 하는 이유는 이산확률변수일 때는 가능도=확률이기 때문, 이 부분에 관한 얘기가 어디 블로그에 있었는데 다시 한번 확인해보자.==