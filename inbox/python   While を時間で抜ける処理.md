#python 

---
2022-03-04  10:37

# python   While を時間で抜ける処理

https://teratail.com/questions/126154

これのベストアンサー。
これいいね。

```python:timer_thread.py
import time
from threading import Thread

cond = True

def f():
	global cond
	time.sleep(1)
	cond = False

thread = Thread(target=f)
thread.start()
while cond:
	print("hoge")
	time.sleep(0.2)
```
```shell
$ python3 timer_thread.py
hoge
hoge
hoge
hoge
hoge

$
```
Thread(target=f) ってなんだろう。
target に指定するのは、動かしたい関数名。
f() を動かしたいので、target=f
そして、タイマーの代わりに裏で動かすのか。

sleep で秒を指定出来るのがいい。




