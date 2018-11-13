---
title: "Billions of sessions"
date: 2018-11-13 10:40:00 -0400
categories: Programming-Tips
---

## 참고
> [http://docs.likejazz.com/time-wait/#time_wait] [1]
> [https://vincent.bernat.ch/en/blog/2014-tcp-time-wait-state-linux] [2]


## intro
Elasticsearch 를 Python으로 사용하거나, 세션을 많이 사용해야 할 경우 당연하지만 막상 마주하면 당황할 만한 내용을 남깁니다.

E/S 사용시 많은 세션을 맺는 일은 빈번하게 일어나는 일입니다.

보통 python 으로 E/S 를 사용하게 될 경우에 사용량이 적을 경우 단일 요청(index, put) 과 같은 요청을 보내게 됩니다.

하지만 이는 분당 전송수가 65535건이 넘어가게 된다면 위험할수도 있는 선택입니다.

단일 서버에서 E/S에 최대 65535 건의 요청을 보내게 된다면 (혹은 65535 건의 세션을 맺게 된다면) TCP 및 기본 네트워크 구조상 다음과 같은 에러를 내뿜으며 정상적으로 작동하지 않습니다.

> OSError: [Errno 99] Cannot assign requested address

이는 현재 최대로 사용할수 있는 PORT 인 65535개를 모두 사용중이기에 생기는 문제입니다.

다만, index 혹은 put 요청의 경우에는 이미 연결하고 종료된 시점이고, 동시에 65535 개를 요청할일은 없는데 이는 어떻게 된 일일까요?

## TCP time_wait
TCP 가 연결을 끊을 시에는 먼저 TCP 연결 종료를 요청한 쪽에서 해당 세션을 2 MSL([Maximum segment lifetime][3])동안 time_wait 상태로 가지고 있게 됩니다.

![no_support_completion](/assets/img/tcp-state-diagram-v2.png)
표 참고

따라서 별다른 설정을 해주지 않았더라면 60초 가량 세션이 time_wait 으로 남게 됩니다.

그렇기 때문에 60초 내로 각각 세션을 65535 개 맺게 된다면 저런 에러가 발생하여 더이상 프로세스를 사용할수 없게 되는 경우가 발생하게 됩니다.

## solution
그렇기에 개발자는 세션을 맺을 경우에는 신중하게 작업하여야 하며, 무턱대고 Thread 처리를 하기보다는 다른 방안을 만들어야 합니다. 

예를 들자면 requests 를 사용하여 elasticsearch 에 수만은 데이터를 보내게 되는 경우 E/S 의 Bulk insert 같은 다른 방안을 모색하여야 합니다.

<!-- 주석 및 참고 링크입니다. -->
[1]:https://elasticsearch-py.readthedocs.io/en/master/
[2]:https://vincent.bernat.ch/en/blog/2014-tcp-time-wait-state-linux
[3]:https://en.wikipedia.org/wiki/Maximum_segment_lifetime