확률에 대한 고찰 : https://brunch.co.kr/@amangkim/107
즉, stochastic이란 데이터에 없는, 우리가 관측하지 못한 잠재적인 부분을 포함해 계량적으로 표현할 수 있는 방법.
deterministic은 우리가 관측하지 못한 부분은 제외하고 오직 데이터로만 표현하는 방법. 즉, 실제 세계보다는 이상적인 세계에서 사용할 수 있는 것. (예를 들어 물리 문제에서 마찰이 없고, 공기저항이 없고, 바람이 불지 않는 상황을 가정하는 것처럼..)

**불확실성이란 "판단이나 의사결정에 필요한 적절한 정보의 부족" 이라 할 수 있다.**

---

Non-deterministic은 입력이 같아도 출력은 다를 수 있지만, 확률적 개념이 없다.
그냥 확률을 생각하지 않고 입력이 같아도 출력이 다른 모든 것을 Non-deterministic이라 한다.

하지만 Stochastic은 확률로 정량화하는 개념이 있다.

---

1. stochastic은 약간은 랜덤한 결과, 약간의 불확실성을 포함한 다양한 프로세스를 의미한다.
---
- stochastic domains는 불확실성을 가지고 있다. 머신러닝은 항상 확실하지 않은 양을 다루고 stochastic quantities를 다루는것이 필요하게 된다. 랜덤 오류, 통계적 노이즈의 대상이 되는 타겟, 목적함수에서 불확실성이 생길 수 있다. 모델에 사용된 데이터가 방대한 population에서 기인한 불완전한 샘플이면 불확실성이 생길 수 있다. 모델은 도메인의 모든 측면을 포착할 수 없고 보이지 않는 상황들을 일반화 하고 일부의 충실도(fidelity)를 잃어야 한다.
---
- 많은 머신러닝 알고리즘이 stochastic인 이유는 최적화나 학습에 랜덤을 explicitly 사용하기 때문
---
- Stochastic Optimization Algorithms은 검색 공간을 탐색(exploring)하는 것과 검색 공간에 대해 이미 학습 한 것을 활용(exploiting)하는 것 사이의 균형을 맞추는 정도를 찾는 알고리즘이다. 최근 어떤 영역이 찾아졌는지 기인해 통계적으로 다음 로케이션을 선택하게 된다

Popular stochastic optimization algorithms:  
- Simulated Annealing  
- Genetic Algorithm  
- Particle Swarm Optimization

Stochastic Gradient Descent (optimization algorithm)
Stochastic Gradient Boosting (ensemble algorithm)

---
SGD를 통해 머신러닝은 무작위성을 얻을 수 있다.

stochastic은 inherent variability가 있는 복잡한 시스템을 모델링하는데에 유용.

이들을 통해 실제 세계의 변동성과 불확실성을 포착할 수 있음. ('포착'이 맞는 표현인가? 맞는거 같기도 하고, 변동성과 불확실성을 알 수 없지만 포착했다는 것은 어느정도 모델링에 이런 불확실성이 포함 되었다는 뜻이니까?)

Deterministic은 시스템의 동작을 정확하게 정의할 수 있는 시나리오에서 사용된다.
(Analytic Solution이 deterministic하게 해결하는 방법이라고 함)
Deterministic과 Stochastic 중 뭘 선택할지는 문제의 성격과 관련된 불확실성 수준에 따라 결정된다. 만약 효율성과 재현성이 중요한 경우에는 Deterministic을 선택.

