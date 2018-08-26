---
title: "About Python Code Object"
date: 2018-08-26 11:40:00 -0400
categories: Deep_Dive_into_Basic
---

## 참고
> [https://stackoverflow.com/questions/33683217/python-code-type-flags] [1]
> [https://www.slideshare.net/daykim7/pyconkr-2018-deep-dive-into-coroutine-110194978/1] [2]
> Python 기준 버전 3.7.0

## Python Code Object
파이썬의 function 및 모든 코드에는 \_\_code__ 라는 속성이 있다

이 속성은 해당 function 및 모든 코드를 분석하는데 도움이 되는 속성이다

이번 포스팅에서는 해당 \_\_code__ 의 중요한 속성 몇개를 써보려 한다

## \_\_code__ object

> example code
> ```python
> def add_c(a, b):
>    print(a, b)
>    c = int(5)
>    return a + b + c
> ```



> \_\_code__.co_consts
>
해당 코드에서 사용한 상수 값을 모두 모아놓은 속성이다
```python
print(add_c.__code__.co_consts)
>> (None, 5)
```
첫번째 인자가 None 인 이유는 docstring을 작성하지 않았기 때문이다

docstring 을 작성하게 되면 첫번째 인자에 들어가게 된다

또한, function 내부에 function 을 작성하는 경우 co_consts 변수에 추가되게 된다


> \_\_code__.co_flags
```python
print(add_c.__code__.co_flags)
>> 67
```
co_flags 는 해당 코드에 속성으로써, 다음을 통해 확인이 가능하다
```python
import dis
print(dis.COMPILER_FLAG_NAMES)
>> {
       1: 'OPTIMIZED',  # 최적화 여부 
       2: 'NEWLOCALS', 
       4: 'VARARGS',  # *arguments 구문 사용
       8: 'VARKEYWORDS',  # **keywords 구문 사용
       16: 'NESTED', 
       32: 'GENERATOR',  # 제네레이터
       64: 'NOFREE',  # *args, **kwargs 비사용
       128: 'COROUTINE', # 동기 코루틴
       256: 'ITERABLE_COROUTINE',  
       512: 'ASYNC_GENERATOR'  # 비동기 제네레이터
  }
```
와 같이 이해하고 있으며, 단순 해당 코드의 속성이라고 보면 될듯 하다

계산법은 67 과 같이 나오면 and 연산을 통하여 계산하면 된다.
```
67        = 0100 0011
OPTIMIZED = 0000 0001
NEWLOCALS = 0000 0010
NOFREE    = 0100 0000
따라서 해당 값은 OPTIMIZED, NEWLOCALS, NOFREE 이 적용되어 있다
```

> \_\_code__.co_varnames
```python
print(add_c.__code__.co_varnames)
>> ('a', 'b', 'c')
```
코드에서 사용한 변수 이름이 담겨있다

> \_\_code__.co_names
```python
print(add_c.__code__.co_names)
>> ('print', 'int')
```
내부적으로 사용한 함수의 이름이 담겨있다

> \_\_code__.co_code
```python
for byte in bytes(add_c.__code__.co_code):
   print(f'{byte:x} ', end='')
>> 116 0 124 0 124 1 131 2 1 0 116 1 100 1 131 1 125 2 124 0 124 1 23 0 124 2 23 0 83 0 
```
파이썬의 byte 코드가 담겨있으며, 내용은 dis.dis(add_c) 와 같다
순서대로 한글자씩 해석하면 되며,

opcode 는 dis.opmap[116] 과 같이 확인이 가능하다 
```python
116 0  # LOAD_GLOBAL 0
124 0  # LOAD_FAST 0
124 1  # LOAD_FAST 1
131 2  # CALL_FUNCTION 2
...
```
바이트 코드에 관련된 간단한 내용은 [다음 링크] [3] 를 참조하면 된다


[1]: https://stackoverflow.com/questions/33683217/python-code-type-flags
[2]: https://www.slideshare.net/daykim7/pyconkr-2018-deep-dive-into-coroutine-110194978/1
[3]: /deep_dive_into_basic/Python-Byte-codes