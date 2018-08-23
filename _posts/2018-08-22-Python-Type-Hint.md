---
title: "About Python Type Hint"
date: 2018-08-22 23:25:00 -0400
categories: Python PEP
---

## Reference
> https://www.python.org/dev/peps/pep-0484/
>
> Python 기준 버전 3.7.0
 
## Python Type Hint


Python 3.5 버전부터 지원하기 시작한 기능중 PEP484에 기반한 Type Hint라는 기능이 있다.

> ```python
> blahblah:str = ''
> ```

와 같이 변수에 Type을 참조하게끔 할수 있으나,

실질적으로 해당 변수에 int 형의 데이터를 넣지 못하게 하거나 하는 행위는 할수 없다

말 그대로 해당 변수에 대한 힌트이며, 개발자간 빠른 소스 이해를 돕기 위한 규칙이다

또한, Pycharm 과 같은 IDE 를 사용할때 다음과 같은 코드는 변수 A 에 대한 code completion 을 제공하지 않는다

- ![no_support_completion](/assets/img/no_support_completion.png)

그러나, 이 코드에 Type hint 를 사용한다면 IDE의 도움을 받아 좀더 편하게 코딩할수 있게 된다

- ![no_support_completion](/assets/img/support_completion.png)

이뿐만 아니라 function 의 return 값또한 hint 를 줄수 있는데

> ```python
> def function(var1: str) -> str:
>     return var1
> ```

와 같이 return 값 지정이 가능하며, 이또한 IDE에서 code completion 을 지원하게끔 해준다
