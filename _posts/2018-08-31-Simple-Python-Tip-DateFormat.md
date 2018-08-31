---
title: "Python Simple Tip DateFormat"
date: 2018-08-28 13:20:00 -0400
categories: Simple-Online-Tip
---

## 참고
> [https://stackoverflow.com/questions/2158347/how-do-i-turn-a-python-datetime-into-a-string-with-readable-format-date] [1]
> 
> Python 기준 버전 3.7.0

## DateTime Format
python 에서 Datetime을 사용할떄 YYYY-MM-DD 와 같은 형태로 포멧팅할때 다음과 같은 코드를 쓰면 좀더 쉽게 사용이 가능하다.
```python
>>> import datetime
>>> f'DateTime! {datetime.datetime.now():%Y-%m-%d}'
'DateTime! 2018-08-31'  # strftime 포멧과 동일!
```

[1]: https://stackoverflow.com/questions/2158347/how-do-i-turn-a-python-datetime-into-a-string-with-readable-format-date