---
title: "About Python docstring"
date: 2018-08-23 11:00:00 -0400
categories: Python PEP
---

## Reference
> [https://www.python.org/dev/peps/pep-0257/] [1]
>
> Python 기준 버전 3.7.0

## Python docstring

#### what is docstring?
docstring은 Python 에서 지원하는 module, function, class, method 의 상태를 정의해놓은 문자열이다

다음과 같은 형태로 사용이 가능하며,

> ```python
> def function(var1: str) -> None:
>    """ function docstring """
>    pass
>```

\_\_doc__ 속성으로 불러올수도 있다

> ```python
> def function(var1: str) -> None:
>     """ function docstring """
>     pass
> ```
> print(function.\_\_doc__)
>
> output >>> function docstring


#### oneline docstring
한줄로만 이루어진 docstring 은 다음과 같이 사용하며, 몇몇 권장하는 규칙들로 이루어져 있다

> ```python
> def function(var1: str) -> None:
>     """ function docstring """
>     pass
> ```

- 한줄의 docstring 이라도 3개짜리 큰따옴표(""") 를 사용하는것이 차후 확장에 용이하다
- 닫는 큰따옴표는 같은 줄에 있는것이 좀더 괜찮게 보이게 된다
- docstring 위 아래로 빈줄이 있게 없어야 한다
- XX를 반환, XX를 계산 과 같은 설명형이 아닌 명령형으로 작성되어야 한다
  - 안좋은 예) XX를 계산하여 반환합니다
  - 좋은 예) XX를 계산 그리고 계산된 값을 반환
- 단순히 function 의 원형을 다시 쓰지 않도록 해야 한다

>ex) bad case
>```python
>def function(a, b):
>    """function(a, b) -> list"""
>```
> 이런 docstring은 buiilt-in 같은 C 로 만들어진 기능을 작성할때만 사용한다.

#### multi-line docstring
여러줄로 이루어진 docstring 은 one-line docstring 과 비슷한 규칙을 사용하고 있다

여러 규칙이 있으나 자주 사용하게 될 규칙을 요약하자면
- multi-line docstring 을 사용시 첫번째 라인은 요약을 작성한다
- 요약 라인 뒤에 한줄의 줄바꿈 뒤에 상세한 설명을 작성한다
- 요약 라인은 여는 큰따옴표와 같은 라인 혹은 다음 라인에 존재할수 있다
- 모든 라인은 첫번째 라인과 같은 띄어쓰기 수준으로 작성되어야 한다
> ```python
> def function(a, b):
>     """ divide a, b and return value
>     
>     Divide a, b
>     if b is 0 then raise Error
>     """
>     ...
>```

와 같은 규칙이 있다

그 외에도 docstring을 작성할 시에 맞춰줘야 하는 규칙들이 있는데

Reference로 써 놓은 사이트에 들어가보면 규칙들을 전부 볼수 있다

[1]: https://www.python.org/dev/peps/pep-0257/