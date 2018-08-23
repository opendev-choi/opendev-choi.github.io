---
title: "Python fstring a-Z"
date: 2018-08-23 19:20:00 -0400
categories: Deep Dive into Basic
---

## Reference
> [https://docs.python.org/3.7/reference/lexical_analysis.html#f-strings] [1]
>
> Python 기준 버전 3.7.0
 
## Python fstring
Python 3.6 에 추가된 기능으로써, 기존의 .format 함수를 대채하는 기능이다

기존의 포멧팅 방식으로는 크게 다음 두가지 방식을 볼수 있다

> printf-style String Formatting
> ```python
> "%d" % 234
> ```
>
> str.format() 
> ```
> "{:d}".format()
> ```
>

이와 같은 방식은 다음과 같은 특징을 가지고 있다
- printf-style String Formatting
  - string formatting 시 tuple 에 대한 처리 어려움
  - 가독성이 떨어짐
  - formatting 하는 개수가 다를 경우 TypeError 발생
  - Operand
  - 이항 연산자이기 때문에 여러 인자를 Formatting 시 Tuple 형태로 모아야 함
  
- .format() Style
  - 가독성 printf-style 보다 뛰어남
  - 인자를 사용하면 TypeError 회피 가능
  - 연산자가 아닌 함수형이기에 printf-style formatting 실행 속도가 더 걸림

이와같은 장점과 단점이 있으면서도 더 명확하고 나은 표현식을 위하여 f-string 문법이 나오게 되었다

f-string 문법은 다음과 같이 사용 가능하다
> ```python
> apple_count = 5
> f"I Have {apple_count} Apple!"
>```
>
> \>>> I Have 5 Apple!

string 사이에 {변수명} 를 사용하면 치환되며, 정확한 구문은 다음과 같다

> replacement_field ::=  "{" [field_name] ["!" conversion] [":" format_spec] "}"

하나하나 설명하자면 다음과 같다

첫번째 [field_name] 은 말 그대로 치환될 변수 이름이며,

두번째인  ["!" conversion] 의 경우는 string으로 치환될때 어떤 string으로 처리할지를 결정하는 방법이다

conversion의 종류는 다음과 같다
- "r" | "s" | "a"
- "r" - [repr()] [2] 함수를 사용하여 컨버팅
- "s" - str() 함수를 사용하여 컨버팅
- "a" - ascii() 함수를 사용하여 컨버팅

다음과 같은 포멧으로 치환이 가능하며,

마지막으로 format_spec은 우리가 흔히 쓰는 s, d, 2.f 등의 [포멧팅 문자열] [3]이다

다음과 모든 필드를 체워 다음과 같은 형식으로 변환이 가능하며,
>  ```f"You Have {3!s:2} Stuff!""```

단순 사용시에는 보통 다음과 같이 사용하게 된다
> ```f"You Have {3} Stuff"```


[1]: https://docs.python.org/3.7/reference/lexical_analysis.html#f-strings
[2]: https://wikidocs.net/15132
[3]: https://docs.python.org/3/library/string.html#formatstrings