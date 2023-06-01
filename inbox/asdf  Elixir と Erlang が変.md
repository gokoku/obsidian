#asdf #elixir  #erlang

---
2022-01-27  13:47

# asdf  Elixir と Erlang が変

asdf が変わったようだ。それはともあれ、

## Erlang を直す
```shell
$ asdf list all erlang

Plugin erlang's list-all callback script failed with output:
Downloading kerl...
No preset version installed for command curl
Please install a version by running one of the following:

asdf install python 3.9.5

or add one of the following versions in your config file at /Users/george/.tool-versions
python anaconda3-2021.05
chmod: /Users/george/.asdf/plugins/erlang/kerl: No such file or directory
Failed to install kerl
```

何だか全然わからん。

一旦 remove する

```shell
$ asdf plugin remove erlang
$ asdf plugin remove elixir

```

関係なかった。

.asdf/shims/curl が邪魔だったみたいなので、一旦退けて、

```shell
$ asdf list all erlang
...
OK

```
この後もどしたが、install のときも邪魔するので、削除した。
```text
削除したファイル

.asdf/shims/curl
.asdf/shims/curl-config
```
python anaconda3-2021.05 が何だか環境汚染してる?!

```shell
$ asdf install erlang 24.2.1
$ asdf global erlang 24.2.1

$ erl -version
Erlang (SMP,ASYNC_THREADS) (BEAM) emulator version 12.2.1
```
## Elixir を直す

```shell
$ asdf list all elixir

No compatible versions available (elixir )
``````

これも上と同じ。
python anaconda3-2021.05 が入ってると Erlang インストール時に動いて、curl を突っ込んでた。
ので、anaconda をuninstall してからやると、

```shell
$ asdf uninstall python anaconda3-2021.05

$ asdf list all elixir
...
1.13.2

$ asdf install elixir 1.13.2
$ asdf global elixir 1.13.2

$ iex --version
Erlang/OTP 24 [erts-12.2.1] [source] [64-bit] [smp:4:4] [ds:4:4:10] [async-threads:1] [jit]

IEx 1.13.2 (compiled with Erlang/OTP 24)
```

OK!!





