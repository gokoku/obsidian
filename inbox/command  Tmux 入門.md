#command 

---
2021-12-17

# Tmux

ティーマックス。

ターミナルマルチプレクサ(端末多重化ソフトウェア、仮想ターミナルマネージャー)。


raspberry pi でssh が切られる度にまたつなぐ必要がなくなるのかな。

https://developers.goalist.co.jp/entry/2017/03/09/184319

terminal をパカパカ開くのも何だかなーとも思ってる。
どこに行ったかわからなくなったり、何枚開いてるか忘れたり。

```shell
$ brew install tmux
```

~/.tmux.conf

* 参考
[https://github.com/RVIRUS0817/ansible_Mac/blob/master/roles/homedirectory/files/.tmux.conf](https://github.com/RVIRUS0817/ansible_Mac/blob/master/roles/homedirectory/files/.tmux.conf)


## tmux コマンド

* tmux ls

* tmux a -t name

* tmux new-session -s name

* ウィンドウ系
    * Ctrl-b + c
        * 新規ウインドウ作成

    * Ctrl-b + ,

        * ウインドウの名前の変更

    * Ctrl-b + p

        * ウインドウの切り替え

    * Ctrl-b + n

        * ウインドウの切り替え

    * Ctrl-b + l

        * 最後に操作したウィンドウに切り替え
       
    * Ctrl-b + 数字

        * 番号のウインドウに切り替え


    * Ctrl-b + &

        * ウインドウの削除

    * Ctrl-b + w

        * ウィンドウの一覧と移動

    * Ctrl-b + d

        * セッションのデタッチ(保存して抜ける。ssh切れてもまたセッションを繋げばOK)

    * Ctrl-b + r

        * tmux.conf 再起動

* ペイン系

    * Ctrl-b + q
        
        * ペイン番号表示

    * Ctrl-b + %

        * 垂直分割

    * Ctrl-b + o

        * ペイン移動

    * Ctrl-b + x

        * ペイン分割削除

    * Ctrl-b + {     ||   Ctrl-b + }

        * ペイン場所替え

    * Ctrl-b 押しながらカーソルキー上下

        * ペインサイズ変更

    * Ctrl-b + z

        * ペインを全面表示にするトグル

    * Ctrl-b + \[

        *コピーモード(vim バインドの hjkl で動かし、space で選択、return で戻る)

    * Ctrl-b \]

        * 貼り付け

    * ⌘ + Alt

        * 選択してコピー


* セッション系

    * tmux : セッションの作成
    * tmux ls : セッションの確認
    * tmux attach -t セッション番号 || tmux a  : セッションのアタッチ
    * tmux kill-session {-t セッション番号} : セッションの削除
    * tmux kill-server : 全セッションの削除




## はて、どう使うのだろう

複数の仮想端末を起動して操る。

* tmux サーバ(セッション)

    * tmux起動時に生成
    * セッションを管理するデーモン

* tmux クライアント( pty: 仮想端末)

    * tmux セッションに接続(attach)しているデーモン(pty)

セッション > ウィンドウ(タブのイメージ) > ペイン

https://qiita.com/1000k/items/dedb38e8a959dee40cc3


### セッション開始

```shell
$ tmux      または tmux new-session
```

Ctrl-b %    ペインを分割
Ctrl-b n     次のウィンドウに移動
Ctrl-b ?      ヘルプ

ウィンドウを縦割りして二つのペインにして、片方にヘルプを出して色々試してみる。

![[Pasted image 20211217220215.png]]


### デタッチ

Ctrl-b d

セッションは残ってるので tmux a か tmux attach -t セッション番号

イマイチ、ペインとウィンドウの関係がわからん。

0: 1 windows (created Fri Dec 17 21:59:18 2021) (attached)

セッション0 に window が 1つ　ということか?

window をペインに分けるけど、window は一つのようだ。


### 時計

Ctrl-b t

![[Pasted image 20211217222513.png]]

### tmux のイメージ図

![](http://3.bp.blogspot.com/-e4Sylm_HwDM/UvyWnuU63wI/AAAAAAAAAe0/RHA87XGTwQ8/s640/tmux.png)

リモート先で tmux を立ち上げるのか。

サーバが切れても ssh ログインしてセッションにアタッチしてやるのか。

リモート先で tmux を立ち上げて、他サーバに接続しておくと繋がったままらしい。

