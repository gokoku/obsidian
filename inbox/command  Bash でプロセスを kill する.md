#command 

---
2022-02-02  17:35

# command  Bash でプロセスを kill する

ws2815 を python script で動かしてるのだが、時々ライブラリから帰ってこないで残る時がある。

どんどん並行で動いていくので、ws2815由来のプロセスだけ抽出して kill して片付けたい。

## どうやろうか

pgrep で python3 のプロセスのIDを取得する。

```shell
$ id=$(pgrep python3)

複数の時もある
$ echo $id
5155 5177 5590
```

これを配列に split するには

<div class="rich-link-card-container"><a class="rich-link-card" href="https://www.koikikukan.com/archives/2019/05/09-235555.php" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://www.koikikukan.com/images/2015/linux_logo.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">bashの変数をsplitして配列を作る方法</h1>
		<p class="rich-link-card-description">
		bashの変数をsplitして配列を作る方法を紹介します。
		</p>
		<p class="rich-link-href">
		https://www.koikikukan.com/archives/2019/05/09-235555.php
		</p>
	</div>
</a></div>

文字置換を利用してsplit するらしい。

デミリタがスペースなので、
```shell
$ list=(${id// /})

$ echo ${list[@]}
5155 5177 5590
$ echo ${list[0]}
5155
$ echo ${list[1]}
5177
$ echo ${list[2]}
5590
```

${変数名//置換前文字列/置換後文字列}

これを forループで回す。


<div class="rich-link-card-container"><a class="rich-link-card" href="https://amano41.hatenablog.jp/entry/bash-for-loop-over-array" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">bash の配列を for ループで使う - 知に至る病</h1>
		<p class="rich-link-card-description">
		bash で for ループを使って配列の要素を参照するやり方のメモです。
		</p>
		<p class="rich-link-href">
		https://amano41.hatenablog.jp/entry/bash-for-loop-over-array
		</p>
	</div>
</a></div>

```shell
$ for v in "${list[@]}"
do
	echo $v
done

5155
5177
5590

ちなみにlength は
echo ${#list[@]}
3
```

## code

 全体でこうなった。
```shell:ws_kill.sh
#!/bin/bash
id=$(pgrep python3)
list=(${id// /})

if test -n ${list[0]} ; then
    for v in ${list[@]}
    do
        echo $v
        p=$(ps $v | grep ws2815)
        if test -n "$p" ; then
           sudo kill -9 $v
        fi
    done
fi
sudo /usr/bin/python3 /home/pi/AI/ws2815_led/ws2815_off.py
exit 0
```

pgrep で python3 と付くやつ全部とって、配列にする。

配列を for で回して、ws2815 と付くやつを kill する。
デフォルトで kill すると -15 らしくうまく消えてくれないので、完全強制終了 -9 にした。
"$p" とすると $p の中にスペースが入ってても大丈夫になる。


