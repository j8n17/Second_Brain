---
input: iterables
return: iterator
tags:
- zip
- iterable
- iterator
- dict
---

# 정의

zip()은 여러개의 iterable을 입력받아 각 iterator의 원소를 튜플의 형태로 순서대로 출력하는 iterator를 반환하는 함수.

# 활용방법

```python
>>> numbers = [1, 2, 3]
>>> letters = ['a', 'b', 'c']
>>> for a, b in zip(numbers, letters):
>>> 	print(a, b)
1 a
2 b
3 c
```

## List로 변환

```python
>>> pairs = list(zip(numbers, letters))
>>> print(pairs)
[(1, 'a'), (2, 'b'), (3, 'c')]
```

## Unzip

위의 list처럼 되어있는 형식도 zip()을 사용해 unzip 할 수 있다.

```python
>>> num, let = zip(*pairs)
>>> print(num, let)
(1, 2, 3) ('a', 'b', 'c')
```

## dict로 변환

두 개의 리스트 또는 튜플을 zip()을 통해 dict로 만들 수 있다.

```python
>>> dict(zip(num, let))
{1: 'a', 2: 'b', 3: 'c'}
```

## 두 iterator의 길이가 다른 경우

zip()을 사용할 때 두 iterator의 길이가 다른 경우, 더 짧은 인자를 기준으로 데이터가 생성된다.

```python
>>> numbers = [1, 2, 3]
>>> letters = ['a']
>>> list(zip(numbers, letters)
[(1, 'a')]
```

이를 이용해 하나의 iterator에서 앞의 원소와 뒤의 원소를 같이 사용하는 for문을 만들 수 있다.

```python
>>> numbers = [1, 2, 3, 4, 5]
>>> list(zip(numbers, numbers[1:]))
[(1, 2), (2, 3), (3, 4), (4, 5
```
