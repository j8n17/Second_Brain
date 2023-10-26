Probability와Likelihood는 모두 확률을 의미하는데 정확히 어떻게 다른 것일까?

- Probability는 확률 분포가 고정된 상태에서, 관측되는 사건이 변화할 때의 '확률'을 뜻하고,
- Likelihood는 관측된 사건이 고정된 상태에서, 확률 분포가 변화할 때의 확률을 표현하는 '가능도'이다.

예를 들면 1, 2, 3, 4, 5 중에서 특정 숫자가 관측될 확률을 구하는 것은 Probability이다.

즉, Probability는 고정된 확률분포에서 1이 관측될 확률, 2가 관측될 확률 등을 뜻한다.

Likelihood는 관측된 사건 2를 고정한 상태에서, 이 2의 확률분포를 바꿔 계산하는 확률이다.

그래서 Probability는 $P(x|\theta)$로, Likelihood는 $L(\theta | x)$로 표현한다. ($\theta$는 확률분포를 구성하는 parameter를 의미하고, $x$는 관측값을 의미한다.)

# 이산 사건에서의 Probability, Likelihood

$P(x|\theta)$와 $L(\theta|x)$는 같은 확률분포에서 같은 관측값을 관측하는 확률을 구하는 것이므로 동일하다.

하지만 $P(\theta|x)$와 $L(\theta|x)$는 다르다. 
- $P(\theta|x)$는 관측값이 주어질 때, 변화되는 확률분포 $\theta$가 나올 확률이고,
- $L(\theta|x)$는 관측값이 주어질 때, 변화되는 확률분포 $\theta$에서 주어진 관측값이 나올 확률이기 때문이다.

# 연속 사건에서의 Probability, Likelihood

연속 사건에서는 $P(x|\theta)$와 $L(\theta|x)$가 다른 값을 가진다.

연속 사건에서는 어떤 구간의 확률을 구하므로 단일 사건(x)에 대한 Probability $P(x|\theta)$는 0이다.

하지만 Likelihood $L(\theta|x)$는 pdf의 y값을 사용한다.

또한 사건 범위($x_1$ ~ $x_2$)에 대한 Probability $P(x|\theta)$는 pdf의 구간적분값이지만, Likelihood $L(\theta|x)$는 범위에 대한 확률을 계산할 수 없다.

연속확률분포에서 y값은 무엇을 뜻하는가?