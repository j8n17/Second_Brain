# Dataset

- Synthetic Regression Data : 노이즈가 정규 분포를 따르는 회귀 데이터
- data_loader
- data_loader를 다시 정의하는데 위랑 뭐가 다른건지? 아래 거는 만들어진 API를 활용하는 방법.

# Model

- \_\_init__
- forward
- loss
- optimizer

# Training Process

- minibatch sampling
- model prediction
- loss calculation
- gradient calculation (backward)
	- gradient clipping?
- update parameters

## Hyperparameters

- epoch
- learning rate

# Evaluation

- validation
	- hyperparameter selection
		- best model save - 이것조차 hyperparameter selection이다. (최적의 epoch을 찾는 것과 같기 때문.)
- test
	- reserved for the final evaluation

- cross validation이란? kfold로 나눈 여러 validation set으로 모두 평가하는 기법
- stacking ensemble - validation set도 학습시키고 싶을 때, 각 fold를 validation set으로 사용하는 모델을 앙상블하는 기법

# Stochastic

어려운 최적화 문제라도 Stochastic Gradient Descent는 상당히 좋은 솔루션을 찾는다.

그 이유는 깊은 신경망에서는 높은 정확도의 성능을 낼 수 있는 다양한 파라미터 구성이 존재하기 때문이다.