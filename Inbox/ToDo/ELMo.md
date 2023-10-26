Embeddings from Language Models

> Summary
> - pretrain된 단어 표현이 많은 NLP 모델의 핵심 요소가 될 것 이다.
> - 좋은 품질의 단어 표현은 2가지를 모델링할 수 있어야 한다.
> 	- 단어의 복잡한 특성 (구문분석, 의미분석에서 사용하는 것 모두)
> 	- 다의어(linguistic context)는 상황에 맞게 표현할 수 있어야 한다.
> 		- 예를 들어 둘다 눈인 eye와 snow는 서로 다른 임베딩 벡터를 가지고 있어야 한다.
> - 다양한 Downstream Task에서 일반적인 임베딩 벡터를 사용하는 것보다 성능이 좋았다.

GloVe와의 차이점
GloVe에서 play와 가까운 단어를 보면 playing, game, players 등 스포츠에 치우쳐 있는데, biLM은 스포츠에서의 play와 연극에서의 play를 다르게 표현한다.

# 특징

각 토큰은 전체 입력 문장의 함수인 representation을 할당받는다.
임베딩 벡터는 기본적으로 bidirectional LSTM을 통해서 학습시킨다.

즉, 전체 입력 문장을 받아서 각 토큰의 임베딩 벡터를 반환하는 것이 ELMo이다.

ELMo가 가진 특징 중 하나는 ELMo의 표현은 깊은 표현이다. 왜냐면 가장 마지막 층의 벡터(표현)만을 사용하는 것이 아니라 학습된 biLM의 모든 레이어의 히든 벡터들을 결합하기 때문에 훨씬 더 downstream 성능이 향상되었다.
이런 다양한 층의 단어 표현을 통해 낮은 레벨의 히든 벡터들은 구문분석에 대한 부분을 포함하고, 높은 레벨의 히든 벡터들은 context dependent한 단어를 표현할 수 있다는 것을 알 수 있었다. (context independent는 문장에 상관없이 단어가 가지는 임베딩 벡터가 같은 것을 뜻함.)

즉, 각 레이어마다 가지고 있는 정보의 깊이가 다르기 때문에 만약 구문 분석을 수행하면 더 낮은 레이어의 가중치를 높게 두고, 만약 의미가 필요한 NLU나 question answering 같은 task에서는 더 상위 레벨의 가중치를 크게 둠으로써 EMLo가 유연하면서도 여러 downstream task의 성능을 높게 나타내는 표현을 생성할 수 있다. -> 가 논문의 저자들이 주장하는 바.

# 구조

기본적인 구조는 Language Modeling, 즉 현재까지의 단어를 통해 다음 단어를 예측하는 것이다.

Bi-directional LSTM은 앞단어부터 예측하는 Forward Language Model과 뒷단어부터 예측하는 Backward Language Model을 같이 학습시키는 모델이다.

이렇게 학습된 Bi-directional LSTM을 통해 얻을 수 있는 각 레이어별 벡터(임베딩 벡터 포함)들을 레이어를 기준으로 concat한다.

이렇게 concat된 벡터들은 task에 따라 적절한 가중치로 가중합해 하나의 벡터를 얻는다. 이 가중치는 downstream task에 따라 함께 학습이 되는 파라미터이다.

이 하나의 벡터는 특정 한 토큰에 대한 임베딩 벡터로 사용할 수 있다

## Bidirectional Language Model

## Forward Language Model
k번째 토큰을 예측하기 위해서 1, ..., k-1 토큰을 가지고 있다고 가정하면, 각각의 전체 시퀀스의 토큰이 나타날 결합 확률 분포는 k번째 이전의 단어가 있을 때 k번째 단어가 나올 확률을 1부터 N까지 모두 곱한 다음과 같다.

$$p(t_1, t_2, \cdots, t_N) = \prod^N_{k=1} (t_k|t_1, t_2, \cdots, t_{k-1}) $$

이때, 임베딩 벡터 $x^{LM}_k$는 context-independent token representation으로 word2vec, CNN, 등 어떤것으로 임베딩된 벡터든 사용한다.

하지만 LSTM의 히든 레이어의 출력 h는 으로 사용된다.

LSTM의 히든 레이어의 출력 $\overrightarrow h ^{LM}_{k, j}$는 context-dependent representation로 사용되고 k번째 토큰, j번째 레이어의 출력을 뜻한다.

그리고 가장 마지막 레이어의 출력 $\overrightarrow h ^{LM}_{k, L}$이 $t_{k+1}$을 예측하는 데에 사용된다.

## Backward Language Model

Forward와 반대로 k번째 토큰을 예측하기 위해 k+1부터 N까지의 단어를 사용한다.

$$p(t_1, t_2, \cdots, t_N) = \prod^N_{k=1} (t_k|t_{k+1}, t_{k+2}, \cdots, t_N) $$

또한 히든 레이어의 출력은 $\overleftarrow{h}_{k, j}^{LM}$과 같이 반대 방향으로 표현한다.

## 최적화

고로 Bidirectional LSTM은 Forward Language Model과 Backward Language Model의 Likelihood를 최대화해야 한다.

# ELMo

레이어 수가 L인 Bidirectional Language Model이 학습이 되면, 총 2L(Forward, Backward) + 1(Embedding)개의 표현을 얻을 수 있고 이들을 다음과 같이 concat해서 표현할 수 있다.
$$h^{LM}_{k, j} = [\overrightarrow h ^{LM}_{k, j} ; \overleftarrow{h}_{k, j}^{LM}]$$
임베딩 벡터는 편의를 위해 $h^{LM}_{k, 0} = [x^{LM}_k;x^{LM}_k]$로 표현된다.

이들의 가중합을 통해 특정 downstream model의 표현을 얻을 수 있다.

특정 task의 k번째 표현은 다음과 같이 표현할 수 있다.
$$ELMo^{task}_k = E(R_k;\theta^{task}) = \gamma^{task}\sum^L_{j=0}s^{task}_j h^{LM}_{k, j}$$
여기서 $s^{task}_j$는 각각의 task에 맞는 j번째 레이어에 대한 가중치를 뜻하고 softmax-normalized weights로 이들을 모두 합하면 1이 되도록 했다.
$\gamma^{task}$는 전체 EMLo vector에 대해서 전반적으로 스케일링하는 요소이다.

# ELMo의 사용

마지막 EMLo 사용법에 대해서 잘 이해를 못했다.