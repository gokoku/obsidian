#python 

---
2021-06-21

# Jupyter Lab デバッガの使い方

https://www.youtube.com/watch?v=B3qxUw4NK7Q

デバッガはプリントでバッグの面倒をアップデートしてくれる。

リアルタイムで Break point で中身が見えるように良しなにしてくれる。


```shell
$ pip3 install jupyterlab==3

$ pip3 install xeus-python notebook

$ jupyter lab (開きたいパス)
```

![[Pasted image 20210621093808.png]]

プラグインでデバッガを install する。

![[Pasted image 20210621094707.png]]

![[Pasted image 20210621094730.png]]

コードを打ち込む。

セルを次にしたいときは、shift + return 

これで、このセルを実行もしてくれるようだ。

![[Pasted image 20210621095635.png]]

# Debugger

Xpython を選択して、デバッガを ON にする。

![[Pasted image 20210621095717.png]]

Break point を付ける。

順々にセルを実行すると、Break Point で良しなに値が表示される。

なんにも設定してないのだ。
ただ単に、Break point をクリックしただけ。

![[Pasted image 20210621095955.png]]

実行したら、デバッガの CALLSTACK で次をクリックする。いつものデバッガだ。

![[Pasted image 20210621100631.png]]

Break Point を設定したら、右サイドの CALLSTACK で実行をステップでやったりするようだ。
