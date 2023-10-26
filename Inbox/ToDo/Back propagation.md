Backpropagation(역전파)란 최종 output에 각각의 파라미터가 얼마나 영향을 미치고 있는지 뒤에서부터 순차적으로 계산하는 것이다.

Chain Rule에 의해 각각의 노드에서는 Upstream Gradient를 받아서 Local Gradient와 곱하고 그 결과를 Downstream Gradient로 보냄으로써 최종적으로 각 가중치에 대한 output의 미분값을 계산한다.

# 그래프 생성

먼저 Forward Propagation에 대한 그래프를 생성한다.

![[Pasted image 20230930095908.png]]

<font color="#7f7f7f">위 사진으로 Back Propagation 계산해보기!</font>

이렇게 Forward Propagation에 대한 그래프가 생성되면 맨 뒤에서부터 Back Propagation을 진행한다.

예를 들어 마지막 output을 f라고 하면 f에 대해 f를 미분하면 1이고 이를 Downstream Gradient로 보낸다.

그 다음 $\frac{1}{x}$를 입력 $x$에 대해서 미분하면 $-\frac{1}{x^2}$이므로 Local Gradient는 $-\frac{1}{1.37^2}$이다. 이전 노드에서 온 Upstream Gradient 1과 곱하면 $-\frac{1}{1.37^2} = -0.53$이고 이를 Downstream Gradient로 보낸다.

그 다음 +1 function은 반환값 $x+1$를 입력값 $x$에 대해 미분하므로 Local Gradient는 1이다. 이전 노드에서 온 Upstream Gradient와 곱하면 -0.53이고 이를 Downstream Gradient로 보낸다.

그 다음 exp function은 반환값 $e^x$를 입력값 $x$에 대해 미분하므로 Local Gradient는 $\frac{\partial}{\partial{x}} e^x = e^x = e^{-1} = 0.37$이다. 이를 Upstream Gradient -0.53과 곱하면 0.2이다.

이렇게 계속 Back Propagation을 진행해 다음 사진과 같이 계산한다.

![[Pasted image 20230930101301.png]]

이렇게 되면 최종적으로 각 가중치들에 대한 output의 미분값을 구할 수 있고 이를 이용해 가중치를 업데이트할 수 있다.

# Gradient 계산의 패턴

이렇게 Back Propagation을 진행하면서 Gradient를 계속 계산하다보면 일정한 패턴이 보이고 이를 다음과 같이 정의할 수 있다.

![[Pasted image 20230930101807.png]]

- add gate : 두 입력이 더해지는 연산의 Gradient는 이전 Gradient과 동일하다.
- mul gate : 두 입력이 곱해지는 연산의 Gradient는 다른 쪽의 입력값과 이전 Gradient의 곱으로 계산된다.
- copy gate : 하나의 입력이 두 군데로 전파되는 연산의 Gradient는 각각의 Gradient를 모두 더해서 계산된다.
- max gate : 두 입력의 최대값이 전파되는 연산의 Gradient는 해당 입력의 Gradient만 이전 Gradient를 계승하고 무시되는 입력의 Gradient는 0으로 계산된다.

# 프레임워크를 통한 Back Propagation

pytorch, tensorflow와 같은 프레임워크를 사용할 때, forward 연산만 정의하면 자동적으로 해당 연산의 그래프를 그리고 이를 통해 Back Propagation을 순차적으로 진행한다.