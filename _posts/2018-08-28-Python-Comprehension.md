---
title: "About Python Comprehension"
date: 2018-08-28 13:20:00 -0400
categories: Deep_Dive_into_Basic
---

## 참고
> [https://docs.python.org/3/tutorial/datastructures.html] [1]
> [https://www.slideshare.net/daykim7/pyconkr-2018-deep-dive-into-coroutine-110194978/1] [2]
> 
> Python 기준 버전 3.7.0

## Python Comprehension
Python 에는 데이터 구조(List, Dict, Set) 를 보기 쉽고, 간단하게 만들 수 있는 방법인 Comprehension이 있다

```python
squares = []
for x in range(10):
    squares.append(x**2)

print(squares)

>>> [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```

다음과 같이 긴 코드도 좀더 간결하게 다음과 같이 만들수 있다

```python
squares = [x**2 for x in range(10)]

print(squares)

>>> [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```

아래와 같이 Comprehension 을 사용한 코드가 좀더 간결하고 읽기 쉬운것을 알수 있다


이처럼 간단한 List Comprehension 의 경우에는 Loop 문을 돌면서 List들을 만들어 줄수 있고,

dictionary, set 또한 Comprehension 을 통하여 간결하게 dictionary, set을 만들어 줄수 있다.

#### how to use Comprehension
###### List
```[x for x in range(10)]``` 와 같은 형태가 기본적인 형태이며, Loop 문을 돌면서 0 ~ 9 까지 들어있는 List를 만드는 구조이다

이 Comprehension 을 사용할때 변수를 다음과 같이 계산할수도 있다
```python
>>> [x * x for x in range(10)]
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```
과 같이 x 값을 계산하여 연산도 가능하며,

if 문과 함께 사용하여 데이터를 가공할수도 있다
```python
>>> [x if x > 5 else 0 for x in range(10)]
[0, 0, 0, 0, 0, 0, 6, 7, 8, 9]
```

또한 리스트를 순회하며 method를 호출할수도 있다
```python
>>> [fruit.upper() for fruit in ['apple', 'banana', 'grape']]
```

###### dictionary
Dictionary 또한 순회하며 Comprehension 을 사용할수 있으며,

다음과 같이 쓰일수 있다
```python
>>> {key: value for key, value in [[1, 3], [2, 6], [3, 2]]}
{1: 3, 2: 6, 3: 2}
```

###### Set
set 도 동일하다. { } 로 감싸주면 된다.
```python
>>> {x for x in [1, 1, 3, 3, 3, 4, 5, 6]}
{1, 3, 4, 5, 6}
```

[1]: https://docs.python.org/3/tutorial/datastructures.html
[2]: https://www.slideshare.net/daykim7/pyconkr-2018-deep-dive-into-coroutine-110194978/1