
---
type: note
---

#elixir

---
2022-09-27  14:46

# elixir  Livebook を普段使いにしたい

Livebook の立ち上げ方も色々ある。
Docker, brew のデスクトップ

で、こうすることにした。

asdf で入れた elixir でインストールしてルートディレクトリに入って cli で起動する。
ルートディレクトリは、MEGA の中にして、どのマシンでも使えるようにする。

~/MEGA/livebook/ に cd して `livebook server --home .` で起動させる。

ということで、

# setup
```shell
$ mix do local.rebar --force, local.hex --force
$ mix escript.install hex livebook

$ asf reshim elixir


$ cd ~/MEGA?livebook
$ livebook server --home .
```
一度、保存ファイルを設定したら、5sec ごとに autosave してくれるので、いつ閉じても大体いい。

ヨシ!

これは Project 内のモジュールを Livebook で使いたい時にこれでインストールすると良いとのこと。

## 最新のデモは最新のノートで動かす

やっぱ最新のやつで動かしたいよな。

elixir 1.14.0 でないと動かない。

pop-os で
```shell
$ sudo apt install libncurses5-dev automake autoconf
$ asdf pulugin add erlang
$ asdf install erlang 25.1.1
$ asdf global erlang 25.1.1
```

```shell
$ asdf install elixir 1.14.0
$ asdf global elixir 1.14.0
$ asdf reshim elixir


$ git clone https://github.com/livebook-dev/livebook.git
$ cd livebook
$ mix deps.get --only prod

# Run the Livebook server
$ MIX_ENV=prod mix phx.server
```

## brew で install したアプリは最新と同じだった

```shell
$ asdf install elixir 1.14.0
$ asdf global elixir 1.14.0
$ asdf reshim elixir

$ brew install livebook
```

最新のデモが動いた。
これで行きましょう。

![[Pasted image 20220930222507.png|200]]
落とすのも楽。



