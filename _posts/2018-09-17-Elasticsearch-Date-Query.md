---
title: "Elasticsearch Date Query"
date: 2018-09-17 13:40:00 -0400
categories: elasticsearch
---

## 참고
> [https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-range-query.html][1]
> 
> Elasticsearch 6.4

## Date Query
필드 타입이 Date 인 경우에는 Oracle 에서 흔히 쓰는 INTERVAL X MONTH 같은 쿼리를 Elasticsearch 에서 구현할수 있는 방법입니다.

date가 현재 - 1일 ~ 현재까지의 데이터를 구하는 쿼리

```
GET _search
{
    "query": {
        "range" : {
            "date" : {
                "gte" : "now-1d/d",
                "lt" :  "now/d"
            }
        }
    }
}
```
와 같이 조건을 지정할수 있습니다.

비교

| 속성 | 설명 |
|------|------|
|gt|Greater than|
|gte|Greater than or equal|
|lt|Less then|
|lte|Less than or equal|

날짜 계산은 다음[2] 페이지와 같이 할수 있으며, 자세한 설명은 다음과 같습니다.

date + X 속성 혹은 date - X 속성

|속성|설명|
|----|----|
|y|years|
|M|months|
|w|weeks|
|d|days|
|h|hours|
|H|hours|
|m|minutes|
|s|seconds|

또한 반올림하여 계산할수 있으며 이는 다음과 같습니다.

/d, /y 등 time unit

ex) now-3d/d - 현재일자를 Day 기준으로 반올림하고 3일을 뺀 일자

-다만, now 는 UTC 기준으로 작성되기 때문에 이를 유의하여야 한다!-

[1]:https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-range-query.html
[2]:https://www.elastic.co/guide/en/elasticsearch/reference/current/common-options.html#date-math