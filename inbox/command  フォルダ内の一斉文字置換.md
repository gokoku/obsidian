#command

---
2022-01-21  14:52

# command  フォルダ内の一斉文字置換


Obsidian でフォルダ名を変えたので、そのリンクを全部統一して置換したい。
すなわち、

```text

#daily notes]]
#daily notes/daily]]


を、全部こうしたい。

#dialy

```

### grep で抽出してみる。

```shell
$ grep -rE "\[\[.*dialy.*\]\]" *
```

-E: 正規表現だよ
-r: リカーシブにディレクトリをたどる

これで良さそうなので、ファイル名だけを取り出す部分はこれ。

```shell
$ grep -rRl "\[\[.*dialy.*\]\]" *
```

-l : ファイル名だけ取り出す。


### sed で変換を試してみる

1個のファイルで様子をみる。

```shell
$ sed -e 's/\[\[.*daily.*\]\]/\[\[daily\]\]/g' 2021-05-31.md
```

良いようだ。

### xargs で連結する

```shell
$ grep  -rlE "\[\[.*daily.*\]\]" * | xargs sed -i.bak -e 's/\[\[.*daily.*\]\]/\[\[daily\]\]/g'
```

-i : 上書きするが、-i.bak とやると somefile.bak と元のファイルが作られるので安心して試せる。

良いようだったので、.bak を全部削除する。

```shell
$ rm -rf *.bak
```

### ファイル名に space が入ってて弾かれる

https://genzouw.com/entry/2021/03/14/080059/2457/

grep にも null 文字を区切りにするオプションがある。

xargs にも null 文字を区切りにするオプションがある。

```shell
#web を #web にするには

$ grep  -rlE "\[\[web\]\]" * --null | xargs  -0 sed 's/\[\[web\]\]/#web/'
標準出力で見る

良いようだ。

$ grep  -rlE "\[\[web\]\]" * --null | xargs  -0 -i.bak sed 's/\[\[web\]\]/#web/'

うまく行った。

$ rm *.bak
```

一々タグ名を書き換えるのもたるい。けど、ページリンクもあるからうーん。

[a-zA-Z_] で捕まえてそれを sed で指定したい。$1 みたいに。

#### grep で様子をみる

```shell
$ grep -E "\[\#a-zA-Z_]*\]\]" *
```

だめだ。結構タグ以外にあるようだ。
1行目だけしか見ないんですけど、1行目だけみたら中を見ないようにする。

-m1 とすれば良いようだ。

```shell
$ grep -E -m1 "\[\#a-zA-Z_]*\]\]" *
```



#### sed の改良
同じ文字が重複してるのを何とかしたい。

**拡張正規表現(-r)**　を使うと出来る。

```shell
$ sed -r 's/\[\[(php)\]\]/#\1/g' 'php Yii framework.md'
```

これで php php と繰り返さなくても良くなった。

更に、1行だけ置換するようにするには、つまり、1行の中に複数あるものもうまくしたい。

ああ、そうだ、g をとって、正規表現だけで何とかする?

わからんから grep で様子を見て、都度これを繰り返そう。


```shell
$ grep -El -m1 "\[\#a-zA-Z_]*\]\]" --null * | xargs -0 sed -r -i.bak 's/\[\[(css)\]\]/#\1/'
```

この(css) の中を一つ一つ変えては実行。変えては実行する。

#### bak は削除

倍々で溜まってしまうよ。

### タグの階層

```text
#js/vue となってたり

#vue     となってたりするので、その都度対応


's/\[\[(react)\]\]/#js\/\1/'

```

こうすれば、一斉に階層を付けたり取ったり文字操作で出来ることがわかった。




https://github.com/OliverBalfour/obsidian-pandoc/wiki/Installation

### 実践

これでタグ名を探す。
```shell
$ grep -E '\[\[[a-z\|\/_]+\]\]' *

```
いや、このまま全部 `#xxxxx` にしよう


