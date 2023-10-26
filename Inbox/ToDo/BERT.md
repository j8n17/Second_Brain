Bidirectional Encoder Representations from Transformer

# BERT

Pretrained 두 목적
- Masked Language Model(MLM) : 특정 단어의 representation을 학습하기 위해서 forward던 backward던 특정 단어만 mask하고 이를 예측하도록 한다.
- Next sentence prediction(NSP) : 특정 두 쌍의 문장이 들어왔을 때, 두 문장이 이어지는 문장인지 아닌지 학습하는 부분.

fine-tuning
BERT의 최상단에 레이어를 쌓아서 task를 수행하게 학습.

# BERT: Model Architecture

transformer의 encoder block만을 사용함.

L : 레이어 개수, H: hidden size, A: num of self attention heads

BERT base : GPT와 비슷한 파라미터 수를 가짐.

BERT large : 레이어 2배, H: 1024, A:16

# BERT Input/Output Representations

한 문장만 입력받을 수도 있고, 한 쌍의 문장을 입력받을 수도 있다.

BERT에서 사용하는 아래 두 단어의 정의

- Sentence : 연속적인 text의 span, 어떤 단어든 연속적인 단어들의 나열.
- Sequence : BERT의 입력 시퀀스, 한 Sentence일 수도 두 Sentence일 수도 있다.

### BERT의 구조

### 입력
- 2개의 Unlabeled Masking Sentence를 입력받음.
- CLS는 모든 시퀀스의 시작을 알리는 첫 토큰, SEP는 두 문장을 구분하는 토큰

### 출력
- 가장 앞에 오는 CLS 토큰 위치의 히든 벡터는 두 문장이 서로 연속적인 문장인지 판별하는 Binary Classification에 사용된다.
-  다른 토큰은 masking된 토큰을 예측하는 데 사용된다.

### Input Representations

Token Embedding, Segment Embedding, Position Embedding이 합쳐져서 입력으로 들어가게 된다.

- Token Embedding - WordPiece 중에서 3만개를 사용.
- Segment Embedding - 첫번째 시퀀스와 두번째 시퀀스를 구분하기 위한 임베딩, 이때 CLS, SEP 토큰도 첫번째 시퀀스로 포함한다.
- Position Embedding - 각 토큰의 위치에 대한 위치 임베딩

# Pre-training BERT
## Task 1 : Masked Language Model (MLM)

- 전체 시퀀스의 15%가 랜덤하게 MASK token으로 마스킹된다.
- 마스킹 된 입력이 BERT를 통과해서 나온 해당 위치의 출력 토큰이 기존 토큰으로 예측되도록 학습한다.

ELMo는 정방향과 역방향을 따로따로 학습해서 특정 토큰의 Representation을 선형결합하는 방식을 사용했는데, BERT는 Bidirectional한 encoder를 사용하되, 마스킹 된 단어들을 잘 예측하도록 학습시키는 것이 BERT의 Pre-training 방식이다.

- BERT가 언급하는 Masked Language Model의 문제점 : 마스킹은 pre-training에서만 사용하지, fine-tuning에서는 사용하지 않는다. 그렇기 때문에 MASK token에 해당하는 miss match가 발생할 수 있다.
- 솔루션 : i번째 token이 마스킹 될 것이라고 선택되면, 그 중 80%만 실제로 마스킹하고 10%는 랜덤한 token으로 치환하고 10%는 마스킹을 하지않는다. -> pre-training, fine-tuning 사이의 miss match 해소 가능.
  비율은 실험적으로 탐색함.
  ![[스크린샷 2023-10-08 22.11.30.png|400]]

## Task 2 : Next Setence Prediction (NSP)

QA(Question Answering), NLI(Natural Language Inference)의 경우 기본적으로 2개 이상의 문장에 대한 관계를 이해해야 풀 수 있음.

문장 단위의 Language Model 같은 경우는 이 관계를 잘 이해할 수 없다라는 것이 저자들의 지적.

ELMo, GPT는 문장 단위의 학습만 하고 두 문장에 대해서는 학습하지 않았음.

그래서 2개 이상의 문장의 관계를 이해해야 풀 수 있는 Binarized next sentence prediction task를 정의했다.

monolingual corpus에서 A문장 다음에 올 B문장을 50%로 가져오고, 50%는 다음 문장이 아닌 것으로 구성해서 둘 사이에 대한 관계를 예측할 수 있게 학습한다.

이렇게 해서 두 문장의 관계를 알아야만 풀 수 있는 downstream task에 대해서 굉장히 많은 도움이 됐다.

## Differences in pre-training model architectures

![[스크린샷 2023-10-08 22.24.14.png]]

- ELMo는 정방향, 역방향으로 LSTM을 여러 단계로 학습을 시킨다음, 모든 hidden state들을 선형결합해 최종 토큰을 획득.
- GPT는 decoder이기 때문에 정방향으로만 학습시켜 다음 단어를 예측하도록 학습
- BERT는 정방향, 역방향으로 학습하지만 입력 시퀀스에 마스킹을 해 마스킹 된 토큰을 예측하도록 학습

# Fine-tuning BERT

BERT의 제일 윗 단에 하나의 layer만 쌓아도 충분히 구현이 가능.

두 문장 입력
- Sentence Pair Classification Tasks - 두 문장이 관련 있는지
- Question Answering Tasks - Question과 Paragraph 두 문장에 대한 답

한 문장 입력
- Single Sentence Classification Tasks - 감성 분석
- Single Sentence Tagging Tasks - 각 토큰에 대한 형태소 분석

![[스크린샷 2023-10-08 22.36.54.png|500]]

