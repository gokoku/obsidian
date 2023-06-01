#python #jupyter_notebook


pyenv で annaconda を選んでる状態

```bash
❯ pyenv versions
  system
  3.9.0
* anaconda3-2020.07 (set by /Users/george/.anyenv/envs/pyenv/version)
```

```bash

$ conda install jupyter
$ jupyter notebook
```

[https://qiita.com/ldap2017/items/f014f1c22de269012273](https://qiita.com/ldap2017/items/f014f1c22de269012273)

IPAexフォントをインストール

[https://moji.or.jp/ipafont/ipaex00401/](https://moji.or.jp/ipafont/ipaex00401/)

matplotlib 環境を調べる。

```bash
import matplotlib
print(matplotlib.matplotlib_fname())      <---ここは後で置き換わる
print(matplotlib.rcParams['font.family'])
print(matplotlib.get_configdir())
print(matplotlib.get_cachedir())
```

font family が [sans-serif] になってる。

設定ディレクトリは     ~/.matplotlibrc

ここに matplotlibrc を置く。

```bash
~/.matplotlibrc/matplotlibrc

font.family : IPAexGothic # default Font
```

---

tensorflow と keras をインストールする。

```bash
$ pip3 install tensorflow
$ pip3 install keras

$ python3
>>> import keras
>>> keras.__version__
'2.4.3'

>>> import tesorflow
>>> tensorflow.__version__
'2.3.1'
```

---

# ショートカット short cut


![](image-kndtwrhl.png)

```bash
B セルの追加

Esc + L 行番号

Ctrl + RT  実行

J 下のセルに移動

K 上のセルに移動

Y コードセルにする

1 Markdown セルの h1
2                h2

P ヘルプサーチ

D D セルの削除

A セルの挿入　上
B セルの挿入　下

RT   セルにフォーカス(エディットモード緑　とコマンドモード青 の切り替え) 
```
