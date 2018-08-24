---
title: "About Python Byte Codes"
date: 2018-08-43 14:20:00 -0400
categories: Deep_Dive_into_Basic
---

## 참고
> [https://opensource.com/article/18/4/introduction-python-bytecode] [1]
>
> Python 기준 버전 3.7.0
 
## Python Bytecode
Python은 기본적으로 인터프리터 언어지만, 

Python 이 코드를 실행하기 위해서는 Bytecode 로 1차적으로 컴파일 한 뒤 이 Bytecode를 파이썬 인터프리터가 실행을 시키게 된다

이 Bytecode를 알아두게 된다면 Python 코드를 좀더 최적화 시키는데 유용할 것이다

이 포스팅은 참고 링크에서 많은 부분을 번역하여 인용하였다.

## Python Stacks
Python에서는 내부적으로 Stack 을 3개 활용하여 코드를 실행시킨다.

- call stack - Function 들의 실행을 관리하는 메인 스택이다, 다른 속성 없이 Frame(현재 실행되고 있는 함수) 들로 이루어져 있다
- evaluation stack (data stack) - 해당 Function 의 데이터 영역이다
- block stack - 해당 Block(Try, While, if) 의 데이터 영역이다

이중 call stack 은 기존의 c언어에서도 찾아볼수 있는 함수들의 실행을 관리하는 스택이며,

evaluation stack (data stack)은 function 단위의 데이터를 저장하는 스택이고

block stack은 Block 문의 데이터를 저장하는 스택이다

이 위의 스택들은 자신의 영역(Function, Block)을 벗어나면 할당 해제되게끔 되어 있다

## Python Function Call
만약, code ```myfunction(val1, 3)``` 을 호출하게 된다면 내부적으로는 다음과 같은 과정을 거친다

1. LOAD_NAME 명령어를 통하여 myfunction Object를 stack에 넣고
2. LOAD_NAME 명령어를 통하여 val1을 stack에 넣게 된다
3. LOAD_CONST 명령어를 통하여 3을 stack에 넣고
4. CALL_FUNCTION 명령어를 통하여 myfunction 을 실행하게 된다 (만약 **kwargs 와 같은 키워드 인자가 있다면 CALL_FUNCTION_KW 명령어를 실행하게 된다))

이와같이 인자를 해당 함수의 stack에 위에 넣게 되고 CALL_FUNCTION을 통하여 호출하게 되었을때 stack에서 꺼내와서 쓰게 된다

이 과정이 정상적으로 이루어진다면 call stack 에 해당 함수를 Frame(현재 실행되고 있는 함수) 으로 올리게 되며, 해당 영역에 인자로 전달한 변수들을 할당하고, myfunction 안에 있는 Bytecode를 실행하게 된다

## Disassembler of Python Code
만약 이 과정들을 Bytecode 로 보고싶다면 python의 기본 모듈중 하나인 `dis` 모듈을 통하여 Bytecode를 읽어볼수 있다.

> ```python
> import dis
>
> def func_print(msg):
>   print(msg)
>
> dis.dis(func_print)
>
> ======= execute result =======
>  4           0 LOAD_GLOBAL              0  (print)  # print 함수를 불러와 함수 스택에 저장
>              2 LOAD_FAST                0  (msg)    # msg 함수를 불러와 스택에 저장
>              4 CALL_FUNCTION            1           # print 함수를 호출
>              6 POP_TOP                              # 스택에서 함수를 POP
>                                                     # 여기까지가 print 함수 사용부분
>              8 LOAD_CONST               0 (None)    # return 을 위해서 None 을 불러옴
>             10 RETURN_VALUE                         # return
> ```

아까 설명했듯이 함수를 불러온 뒤, 인자를 스택에 저장하고, CALL_FUNCTION을 통하여 호출하여 함수를 실행시키고 있다

## next

이번 포스팅에서는 바이트코드의 기본적인 내용만을 다뤘지만, f-string 과 .format() 함수의 속도 비교등 자세한 사항을 빠른 시일내로 다룰 예정이다


[1]: https://opensource.com/article/18/4/introduction-python-bytecode