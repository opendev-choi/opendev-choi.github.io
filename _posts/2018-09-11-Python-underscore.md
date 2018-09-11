---
title: "Python underscore"
date: 2018-09-11 11:00:00 -0400
categories: Deep_Dive_into_Basic
---


## 참고
> [https://hackernoon.com/understanding-the-underscore-of-python-309d1a029edc][1]
> 
> Python 기준 버전 3.6.6

## Python underscore
파이썬에서는 _ 을 사용한 변수명 작명, \_\_main__ 과 같은 특이한 변수들을 사용할때 보통 underscore('_') 를 사용하여 표현하고는 합니다.

이런 underscore 를 쓰는 부분을 좀더 찾아봤습니다.

## Using Interpreter
인터프리터의 마지막 실행값을 표현할때 사용하곤 합니다.

```python
>>> 10 
10 
>>> _ 
10 
>>> _ * 3 
30 
>>> _ * 20 
600
```

다음과 같이 마지막 실행값을 저장하는 변수로써 _ 를 사용하게 됩니다.

## Ignore Values
for 문에서 iteration 을 할때 실제로 값이 중요하지 않다던가, 튜플을 순회할때 첫번째 값이 중요하지 않다고 생각할때 asdf, temp 와 같은 변수명 대신 _ 을 사용하게 됩니다.

```python
# unpacking 할때 값을 무시하고 싶을 경우
x, _, y = (1, 2, 3) # x = 1, y = 3 
# unpacking 할때 중간 값을 무시하고 싶을 경우, "Extended Unpacking" 라고 불리우며 3.x 버전에서만 지원합니다.
x, *_, y = (1, 2, 3, 4, 5) # x = 1, y = 5  
# index 값을 무시하고 싶을 경우
# ex) 다음과 같이 단순 x 번 함수를 호출해주는 경우
for _ in range(10):     
    do_something()  
# tuple 에서 첫번째 값이 필요하지 않을 경우
for _, val in list_of_tuple:
    do_something(val)
```

## 변수, 함수명 작명에서 규칙으로 지정된 경우
Python 의 Style Guide 인 [PEP8] [2]에 지정된 명명 규칙으로 사용됩니다.
- \_single\_leading\_underscore
  - 일종의 private 과 비슷한 사용법입니다.
  - 파이썬은 private 가 없기 때문에 강제로 사용은 할수 있지만 개발자가 외부에서는 사용하지 않는 함수를 만들때 사용됩니다.
  - from A import * 시에는 A 에서 _ 으로 시작되는 함수, 클래스는 불러오지 않습니다.
- single\_trailing\_underscore\_
  - Python의 int, str, list 와 같은 예약어를 피하기 위하여 사용합니다.
  - ex) ```list_ = []```
- \_\_double_leading_underscore
  - mangling 에 사용됩니다.
  - 같은 이름의 함수를 오버라이딩 했을 경우 상위 클래스의 값에 접근하기 위하여 사용됩니다.
- \_\_double_leading_and_trailing_underscore__ 
  - "magic" 오브젝트 혹은 유저 namespace 에 존재하는 속성을 의미합니다.
  - \_\_init__ 혹은 \_\_name__ 과 같은 속성입니다.
  - 유저가 새로 만들어서 쓰지 않습니다. 오로지 문서에 나온 값만 사용합니다.

## As Internationalization(i18n)/Localization(l10n) functions
gettext 와 같은 GNU gettext 에서 사용됩니다.
```python
# see official docs : https://docs.python.org/3/library/gettext.html
import gettext
gettext.bindtextdomain('myapplication','/path/to/my/language/directory')
gettext.textdomain('myapplication')
_ = gettext.gettext

# ...
print(_('This is a translatable string.'))
```

## separate the digits
1000 단위의 구분자를 읽기 쉽게 하기 위해 다음과 같이 1,000 으로 표시할 경우처럼 코드 에서 사용할때 _ 를 사용합니다.
10진수, 16진수 모두 사용 가능합니다.
```python
billion = 1_000_000_000
print(f'5 billion!!! {billion * 5}')
```


[1]:https://hackernoon.com/understanding-the-underscore-of-python-309d1a029edc
[2]:https://www.python.org/dev/peps/pep-0008/