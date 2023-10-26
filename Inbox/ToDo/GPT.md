Generative Pre-Trained of a Language Model

# Backgrounds

- Unlabeled Data가 훨씬 많다.
- 그래서 이를 사용해서 학습을 시키면 더 좋은 모델을 얻을 수 있지 않을까?

# Motivation

Unlabeled Text Corpora를 통해 Generative model 학습.
Labeled Text를 통해 fine-tuning을 하면 더 좋은 모델을 얻을 수 있음.

GPT와 ELMo의 차이점
- transformer의 decoder만 사용함.
- unlabeld data의 단어 레벨 이상의 정보를 활용하는 것은 어렵다.
- 단순히 unlabeled data를 가지고서는 어떤 목적함수가 transfer하는데 가장 효과적인지 불분명하다.
- 그리고 task가 정해졌을 때도 어떤 방식으로 transfer하는게 가장 효과적인지도 불분명하다.

# GPT : Unsupervised pre-training

L1(U) 목적함수 : 바로 직전 데이터(i-1)부터 k번째 전 데이터(i-k)까지 주어졌을 때, i번째 데이터일 확률을 최대화하는게 unsupervised language model의 목적이다. (k는 context window의 사이즈)

GPT는 transformer의 decoder만을 쭉 올려쌓는것.
이들은 masked self-attention이므로 현재 예측하고자 하는 단어보다 다음 순서의 단어들은 마스킹한 후 학습한다.

# GPT : Supervised fine-tuning

x_1부터 x_m까지 데이터가 주어졌을 때, transformer의 반복되는 decoder 블록을 마지막까지 통과시킨 히든 벡터를 linaer layer를 통과시킨 후 softmax를 통해 확률을 얻는다.

L2(C) 목적함수 : 주어진 시퀀스에 따라서 정답 y일 확률을 최대화 한다.

L3(C) 목적함수 : L2(C) + $\lambda$ * L1(C)로 supervised learning용 corpus를 사용해 두 목적함수를 모두 사용.

- 이를 통해 supervised model에 대한 일반화 성능이 향상
- 학습 속도(convergence:수렴성)가 빨라짐

# GPT : Task-specific input transformations

Classification, Entailment, Similarity, Multiple Choice 등 Task에 따라 입력이 달라진다.

![[스크린샷 2023-10-08 21.32.07.png]]

# Experiments

## Pre-training

- BookCorpus
- I Billion Word Language Model Benchmark (ELMo에서 사용)

두 데이터셋 사용.

## Tasks & Datasets

![[스크린샷 2023-10-08 21.35.57.png]]

# Ablation studies

데이터셋이 크면 auxiliary objective가 도움이 된다. 하지만 작으면 아니다. (아마 L3 목적함수를 말하는 듯.)

task specific한 목적함수는 데이터셋이 작으면 도움이 안된다.