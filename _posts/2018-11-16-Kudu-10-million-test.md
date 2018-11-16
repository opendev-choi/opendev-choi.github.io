---
title: "Kudu 10 million insert test "
date: 2018-11-16 10:40:00 -0400
categories: kudu
---

## intro
회사의 프로젝트로 KUDU 로 이것저것 테스트를 하다가 1000만건을 KUDU 에 insert 할때 걸리는 시간을 측정하는 일을 하게 되었습니다.

다만 보통의 KUDU 설치 후 impala 를 통하여 사용하게 되는데, 이번에는 impala 를 사용하지 않고 KUDU 만 단독으로 사용하여 테스트하게 되었습니다.

특이한 점은 해당 test를 위한 tool 을 만드는 과정에서 있었는데

KUDU 의 특징인지 Python 을 평소에 좋아하던 저는 깔끔하게 정리되어 있는 client 가 없어 여기저기서 예제 소스를 받아 테스트를 하게 되었습니다.

그나마 해당 소스가 가장 잘 정리되어 있던 예제 소스를 먼저 소개하겠습니다.
[https://github.com/cloudera/kudu-examples/tree/master/python/dstat-kudu] [1] -- Python 예제
[https://github.com/cloudera/kudu/tree/master/examples/cpp] [2] -- C++ 예제, 빌드도 간단하고 쉬움

해당 두개 소스에서 보고 Python kudu-python 이란 모듈을 사용하여 테스트를 진행하게 되었습니다.


## in test
다음 파이썬 소스를 통하여 테스트 하였습니다.
```python
import kudu
import datetime

client = kudu.connect('10.1.3.21')
ss = client.new_session()


# INSERT TABLE
def insert_table(table_name: str = 'test_insert') -> None:
    s_time = datetime.datetime.now()
    for idx in range(1_000_000):
        tb = client.table(table_name).new_insert()
        tb["col1"] = idx
        ss.apply(tb)
        if idx % 1_000:
            ss.flush()
        if idx % 50_000 == 0:
            print(f'Job Progress :: {idx}\t|\t{(datetime.datetime.now() - s_time).seconds}')
    e_time = datetime.datetime.now()
    print(f'Job Done :: {(e_time - s_time).seconds}.{(e_time - s_time).microseconds} Sec')

insert_table('test_insert')
```

실행 결과
> Job Progress :: 0	|	0
> Job Progress :: 50000	|	285
>
> KeyboardInterrupt

Python 라이브러리 사용시 생각보다 너무 많은 시간이 걸려 더이상 테스트가 불가능한 상태여서 일단 종료하였고, 겨우 5만건 insert시 285초나 걸리는게 분명 문제가 확실했습니다.

아마 kudu-python 라이브러리 자체가 C로 짜여진 라이브러리에 인터페이스만 맞춘 라이브러리라고 하기 때문에 아마 성능면에서는 많이 뒤떨어 지는 것 같습니다.

성능 향상 시킬수 있는 방법을 찾다 보니 ss.flush() 부분을 자동으로 하게끔 할수 있다 하여 찾아보니 소스에서 flush 를 삭제하고 from kudu.client import FLUSH_AUTO_BACKGROUND 를 추가해주면 된다고 하여 테스트 해보았습니다.

소스는 단순하게 flush 삭제 및 해당 모듈 import 입니다.

실행 결과
> Job Progress :: 0	|	0
> Job Progress :: 50000	|	199
> Job Progress :: 100000	|	407
> Job Progress :: 150000	|	612

확실히 80초정도 줄어든것이 보이지만 아직은 만족스러운 상태는 아닙니다.

혹시나해서 C++ 라이브러리를 사용하여 테스트 해보았습니다.

주요 부분은 다음과 같습니다.

```c++
...

static Status InsertRows(const shared_ptr<KuduTable>& table, int num_rows) {
	shared_ptr<KuduSession> session = table->client()->NewSession();
	KUDU_RETURN_NOT_OK(session->SetFlushMode(KuduSession::MANUAL_FLUSH));
	session->SetTimeoutMillis(5000);

	for (int i = 0; i < num_rows; i++) {
		KuduInsert* insert = table->NewInsert();
		KuduPartialRow* row = insert->mutable_row();
		KUDU_CHECK_OK(row->SetInt32("key", i));
		KUDU_CHECK_OK(row->SetInt32("integer_val", i * 2));
		KUDU_CHECK_OK(row->SetInt32("non_null_with_default", i * 5));
		KUDU_CHECK_OK(session->Apply(insert));
		if (i % 1000 == 0) {
			Status s = session->Flush();
			if (s.ok()) {
				continue;
			}
		}
		if (i % 100000 == 0)
			std::cout << "IDX::" << i << std::endl;
	}
	
	Status s = session->Flush();
	if (s.ok()) {
		return s;
	}

...

	KUDU_CHECK_OK(client->OpenTable(kTableName, &table));
	KUDU_CHECK_OK(InsertRows(table, 10000000));
	KUDU_LOG(INFO) << "Inserted some rows into a table";
```
> example 소스를 많이 참고하였습니다.

역시 C++ 라이브러리를 사용하여서 그런지 속도가 많이 빨라진것을 알수 있었습니다.

3회 테스트시 다음과 같은 성능을 보여줬습니다.

|회차|소요시간|
|--|--|
|1회|0:02:29|
|2회|0:01:50|
|3회|0:02:03|

기존의 가장 빠른 속도인 199초보다 가장 느린 것도 149초로 무려 50초 가량 빨라진것을 알수 있었습니다.

## 결론?
kudu Python 라이브러리는 확실히 c++ 라이브러리보다 꽤 느리다는것을 알수 있었습니다.

다만, 편의성, 익숙한 언어등 성능이 매우 많이 최적화 되어야 하는 경우가 아니라면 kudu python 라이브러리를 쓰는것도 하나의 방법이 될 수 있습니다.

[1]:https://github.com/cloudera/kudu-examples/tree/master/python/dstat-kudu
[2]:https://github.com/cloudera/kudu/tree/master/examples/cpp