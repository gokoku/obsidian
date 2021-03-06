#command #git

---
2021-08-01

# Git 内部コマンド

「アリスとボブの Git 入門レッスン」Git の内部を探りに行く p251 


### index ファイルの中身を見るコマンド。

add すると index ファイルが作られる。

```shell
$ git ls-files -s

100644 4effa19f4f75f846c3229b9dbdbad14eff362f32 0	README
```

| ファイル型 | SHA-1 ハッシュ値                         | ステージ数 | ファイル名 |
| ---------- | ---------------------------------------- | ---------- | ---------- |
| 100644     | feffa19f4f75f846c3229b9dbdbad14eff362f32 | 0          | README     |


### SHA-1 ハッシュ値の求め方

```shell
$ pry
> content = "hello!\n"
> header = "blob #{content.length}\0"  # 最後に 0x00 らしい
> blob_data = header + content
> requre 'digest/sha1'
> sha1_hash = Digest::SHA1.hexdigest(blob_data)
```

ヘッダがタイプとバイト数。

プラス データで SHA-1 ハッシュ値を計算する。

これでハッシュ値がわかる。

```shell
$ echo 'hello!' | git hash-object --stdin
```

### オブジェクトの中身

add するたびに、.git/objects  の中にフォルダとファイルで作られている。

オブジェクトの情報はハッシュ値で見られる。

```shell
$ git cat-file -t 4effa19
blob                      # タイプ

$ git cat-file -s 4effa19
7                         # データのバイト数

$ git cat-file -p 4effa19
hello!                    # データの内容
```

オブジェクトは zlib 圧縮されている。

```shell
$ pry
> require 'zlib'
> content = "hello!\n"
> header = "blob #{content.length}\0"
> blob_text = header + contnt
> zlib_data = Zlib::Deflate.deflate(blob_text, 1)
==> "x\x01K\xCA\xC9OR0g\xC8H\xCD\xC9\xC9W\xE4\x02\x00\"\x1A\x046"     # これがファイルとしてあるらしい

> f = open(".git/objects/4e/013625030ba8dba906f756967f9e9ca394464a")
> blob_data = f.read
> f.close
> zlib_text = Zlib::Inflate.inflate(blob_data)
=> "blob 7\x00hello!\n"
```

-w をつけるとオブジェクトが作られる。

```shell
$ echo 'hello!' | git hash-object -w --stdin
```

![[Pasted image 20210801213259.png]]

## コミットするとどうなる

objects の中身が２つ増える。

コミットされたオブジェクトをリストアップする。

```shell
$ git rev-list --all --objects --format=oneline

5905ce152a30244bf97efa61d63b481667d32b7a 1st commit
34e600920700d3d2d96ff92e3c057a8d24b1f3ed
4effa19f4f75f846c3229b9dbdbad14eff362f32 README
```

増えたのは１つ目が、34e60...

```shell
$ export HASH=34e60; git cat-file -t $HASH; git cat-file -s $HASH; git cat-file -p $HASH

tree
34
100644 blob 4effa19f4f75f846c3229b9dbdbad14eff362f32	README
```

tree オブジェクト、中身は README オブジェクト。

増えた２つ目がコミットオブジェクト、5905c...

```shell
$ export HASH=5905c; git cat-file -t $HASH; git cat-file -s $HASH; git cat-file -p $HASH

commit
171
tree 34e600920700d3d2d96ff92e3c057a8d24b1f3ed
author george <george.6809@gmail.com> 1627821226 +0900
committer george <george.6809@gmail.com> 1627821226 +0900

1st commit
```

タイプが commit で、コミットそのものが入ってる。log と同じ。

tree を指している。

### ディレクトリを掘ってファイルを作って2nd commit

Directory を作って、その中にファィルを作ってコミットすると、treeオブジェクトは、blob と新しく作ったディレクトリ tree を指している。

```shell
$ export HASH=1162ff; git cat-file -t $HASH; git cat-file -s $HASH; git cat-file -p $HASH
tree
65
100644 blob 4effa19f4f75f846c3229b9dbdbad14eff362f32	README
040000 tree f8b62b9efa7e2569a05adc8d958f57cea2637414	ruby
```

新しく作ったディレクトリruby は tree で blob を指している。

```shell
$ export HASH=f8b62; git cat-file -t $HASH; git cat-file -s $HASH; git cat-file -p $HASH

tree
37
100644 blob 01a178ad4d82bd04e8299231ed5b1b8827f9ae82	sample.rb
```

tree コミットオブジェクトをみれば、その状態の復元ができる仕組み。

そして、コミットオブジェクトが、

```shell
$ export HASH=6a4c83; git cat-file -t $HASH; git cat-file -s $HASH; git cat-file -p $HASH
commit
219
tree 1162ffefbf8f91ca7a01a80404af8b301e72dfd7
parent 5905ce152a30244bf97efa61d63b481667d32b7a
author george <george.6809@gmail.com> 1627822223 +0900
committer george <george.6809@gmail.com> 1627822223 +0900

2nd commit

```

保持してる tree オブジェクト、そして parent 情報が増えている。

これで履歴が辿れる。

## タグ、ブランチ、HEAD、チェックアウトの実態

#### タグを追加する

```shell
$ git tag v1.0

$ find .git/refs -type f

.git/refs/tags/v1.0


$ cat .git/refs/tags/v1.0
6a4c83c8e94976104546241945c86d34425b7f97
```

指してるハッシュ値が中身になる。

同じ refs の中の heads にある master も同じ。

```shell
$ cat .git/refs/heads/master
6a4c83c8e94976104546241945c86d34425b7f97
```

#### 新たなコミットをする

```shell
$ echo 'This is inside Git.' >> README
$ git commit -am 'add inside Git'

$ cat .git/refs/heads/master
6d132199613279193efff49ba78522775282c5e8

$ git rev-list --all --objects --format=oneline
6d132199613279193efff49ba78522775282c5e8 add inside Git
...
```

master は最新のコミットのハッシュ値になってる。タグはそのままの値。

このときの HEAD の中身は、今のブランチを指す。

```shell
$ cat .git/HEAD
ref: refs/heads/master
```

#### 新たなブランチをつくってみる

```shell
$ git checkout -b next

$ find .git/refs -type f

.git/refs/heads/next
.git/refs/heads/master
.git/refs/tags/v1.0


$ cat .git/HEAD
ref: refs/heads/next
```

refs/heads に next が追加されて、HEAD の中身が next ブランチを指した。

## オブジェクトを守る仕組み

```shell
$ git reflog
6d13219 (HEAD -> next, master) HEAD@{0}: checkout: moving from master to next
6d13219 (HEAD -> next, master) HEAD@{1}: checkout: moving from master to next
6d13219 (HEAD -> next, master) HEAD@{2}: commit: add inside Git
6a4c83c (tag: v1.0) HEAD@{3}: commit: 2nd commit
5905ce1 HEAD@{4}: commit (initial): 1st commit
```

これは HEAD が過去に参照していたハッシュ値の記録とのこと。

実態は .git/logs にあるテキストファイル。

```shell
$ find .git/logs -type f

.git/logs/HEAD
.git/logs/refs/heads/next
.git/logs/refs/heads/master
```

参照のないオブジェクトを確認。

```shell
$ git fsck

Checking object directories: 100% (256/256), done.
dangling blob ce013625030ba8dba906f756967f9e9ca394464a
```

dangling オブジェクトがそれとのこと。一個ある。





