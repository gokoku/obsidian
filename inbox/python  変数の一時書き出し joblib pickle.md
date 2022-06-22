#python

---
2021-12-28

# 変数の一時書き出し joblib と pickle

## joblib

```shell
$ pip3 install joblib
```

```python
import sys
sys.setrecursionlimit(10000) #エラー回避
import joblib


test_list = [1,2,3,4,5]

# 書き出し
joblib.dump(test_list, "temp.txt", compress=3)


# 読み出し
a = joblib.load("temp.txt")

print(a)
```
ファイルサイズが小さいとのこと。

大きな配列で高速になるらしい。

記述がシンプルに見えるなー。


## pickle

```python
import sys
sys.setrecursionlimit(10000) #エラー回避
import pickle

test_list = [1,2,3,4,5]

# 書き出し
with open('temp.txt', 'wb') as f:
   pickle.dump(text_list, f)

# 読み込み
with open('temp.txt', 'rb') as f:
   a = pickle.load(f)
print(1)
```

圧倒的に早いらしい。

でも、ごちゃごちゃしてるように見える。