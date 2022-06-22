#zsh

---
2022-04-01  20:02

# zsh  基本の設定

```shell
autoload -U compinit
compinit
setopt auto_pushd
setopt auto_cd
setopt correct
setopt cdable_vars
```

```shell
$ cd - 　　　　リターンでひとつ前に戻る

$ cd -       ここでtabすると候補が出る
```

```shell
$ ls **/README    # 再起的に探索して README ファイルを探し当てる

$ ls sample<50->    # sample50, sample51, sample52 にマッチ

$ ls *.(c|h)       # *.c *.h どちらにもマッチ
```

```shell
$ who > file1 > file2  # who の出力を file1 と file2 に

$ > test.txt   # cat > test.txt と一緒、

$ < test.txt   # more test.txt と一緒

$$ >> test.txt # 標準入力から追記
```


