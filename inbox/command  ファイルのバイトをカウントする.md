#command 

---
2022-02-03  16:11

# command  ファイルのバイトをカウントする

wc -c でバイトカウントが出来る。

これから数字だけ抽出する。

```shell
$ wc -c voice.wav
44 voice.wav

$ echo $(wc -c voice.wav | cut -f 1 -d ' ')
44
```
デミリタを -d で設定 スペースに。

フィールドの 1番目で -f 1


## code
バイトが 100 未満かそれ以外かが知りたいスクリプト。

```shell:little_size_check.sh
#!/bin/bash

FILES='/home/pi/AI/watson'

limit=100
if [ $# == 1 ]; then
  limit=$1
fi

count=$(wc -c $FILES/voice.wav | cut -f 1 -d ' ')
if [ $count -lt $limit ]; then    # <
  echo -n "$limit 未満"
else
  echo -n "$limit 以上"
fi
```

test の代わりに \[\] を使う。


```shell
$ ./little_size_check.sh
100 未満

$ ./little_size_check.sh 1000
1000 未満
```




