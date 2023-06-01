#python #python/rx

---
2021-08-19

# Rx

* ストリーム
* ストリーム加工
* スケジューリング


https://dev.classmethod.jp/articles/try-rxpy/

```shell
$ pip3 install rx
```

```python
from rx import range
from rx import operators as op

stream = range(10)

stream.pipe(
  op.filter(lambda x: x % 2 == 0),
  op.count()
).subscribe (
  on_next = print,
  on_completed = lambda: print('Done!')
)
```


```python
import json
from urllib.request import urlopen
from rx import from_
from rx import operators as op
from rx.scheduler import ThreadPoolScheduler

ENDPOINT = "http://zipcloud.ibsnet.co.jp/api"
code_list = [
  "0700001",
  "7070403",
  "9051308",
]

stream = from_(code_list)
pool_scheduler = ThreadPoolScheduler(3)

stream.pipe(
  op.do_action(lambda x: print(f'郵便番号: {x}')),          #郵便番号: 0700001
  op.map(lambda r: f"{ENDPOINT}/search?zipcode={r}"),      #http://zipcloud.ibsnet.co.jp/api/search?zipcode=0700001
  op.observe_on(pool_scheduler),                           #マルチスレッドで実行
  op.map(urlopen),                                         # URLでリクエスト、レスポンス
  op.map(json.load),                                       # {'message': None, 'results': [{'address1': '北海道', 
                        	                             #    'address2': '旭川市', 'address3': '新富１条',
	                                                     #    'kana1': 'ﾎｯｶｲﾄﾞｳ', 'kana2': 'ｱｻﾋｶﾜｼ', 'kana3': 'ｼﾝﾄﾐ1ｼﾞｮｳ',
	                                                     #    'prefcode': '1', 'zipcode': '0700001'}], 'status': 200}
  op.map(lambda r: r["results"][0]["address1"]),           # 北海道
).subscribe (
  #on_next = print
  on_next = lambda x: print(f"都道府県 : {x}")
)

input() #完了待ち
```

```plain
郵便番号: 0700001
郵便番号: 7070403
郵便番号: 9051308
都道府県: 北海道
都道府県: 岡山県
都道府県: 沖縄県
```

ruby でも出来るかな。

