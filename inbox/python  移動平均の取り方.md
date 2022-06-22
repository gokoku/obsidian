#python 

---
2021-12-28

#  移動平均

多分だけど、信号の平滑化で、範囲をずらしながら平均とるイメージでは無いだろうか。

それだったらフィルターに使いたい。

https://www.delftstack.com/ja/howto/python/moving-average-python/

```python

```python
import numpy as np
def moving_average(x, w):
    return np.convolve(x, np.ones(w) / w, 'valid')

data = np.array([10,5,8,9,15,22,26,11,15,16,18,7])

print(moving_average(data,4))


```
array([ 8.  ,  9.25, 13.5 , 18.  , 18.5 , 18.5 , 17.  , 15.  , 14.  ])

イメージ通りの動きをしてくれた。

これを一々実装しなくてもいいということか。


### 入力をストリームにしたい

queue に入れて、これの平均をとると移動平均と同じことになるのでは?

```python
from collections import deque

que = deque([1,2,3])
que.popleft()
1
que.append(4)

que
[2,3,4]
```
que の大きさが window というわけだ。

que の最大を制限できるとのこと。これだ。

```python
from collections import deque

que = deque([1,2,3], 3)

que.append(4)
deque([2,3,4], maxlen=3)
que.append(5)
deque([3,4,5], maxlen=3)
```

これと、que の average を取れば出来上がりでは無いだろうか。

```python
import numpy as np
import collections import deque

que = deque([], 3)

que.append(1)
que.append(2)
que.append(3)
np.average(que)
2.0
```
出来た!!!

が、呼ばれるたびに空になるので貯まらない。

joblib がお手軽に永続化できるらしい。




#python  変数の一時書き出し joblib pickle]]

