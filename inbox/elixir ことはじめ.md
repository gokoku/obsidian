#elixir 


https://elixir-lang.jp/install.html


![](image-kn9mq069.png)

Mac にインストール

```shell
$ brew update
$ brew install elixir


Man pages can be found in:
  /usr/local/opt/erlang/lib/erlang/man

Access them with `erl -man`, or add this directory to MANPATH.
```

erl -man って使わないからいいかな。


anyenv に exenv がある。
これが elixir の rbenv とのこと。

## Debian 系
Erlang をビルドするところから。
```shell
$ sudo apt install build-essential libncurses5-dev libssl-dev openssl
$ sudo apt install libncursesw5-dev
$ sudo apt install autoconf
$ sudo apt install unixodbc xsltproc
$ sudo apt install libwxgtk3.0
$ sudo apt install erlang-jinterface
$ sudo apt install libxml2-utils
```

kerl を使うところからか?
https://qiita.com/tatsuya6502/items/f15da8ea6e793c5038a2
```shell
$ curl -O https://raw.githubusercontent.com/kerl/kerl/master/kerl
$ chmod a+x kerl
$ sudo mv kerl /usr/local/bin/
$ kerl list releases
$ kerl update releases
23.3

$ kerl build 23.3 23.3
$ kerl install 23.3 ~/.anyenv/envs/erlenv/releases/23.3

$ erlenv rehash
$ erlenv releases
$ erlenv 23.3

$ erlenv release
23.3
```


それとも、ソースダウンロードからか?
https://qiita.com/nownabe/items/67a27ddd23652676ffee
```shell
$ wget http://www.erlang.org/download/otp_src_18.2.1.tar.gz
$ tar zxf otp_src_18.2.1.tar.gz
$ cd otp_src_18.2.1
$ ./configure --prefix=$HOME/.anyenv/envs/erlenv/releases/18.2.1
$ make -j 4
$ make install
$ erlenv global 18.2.1
$ erlenv rehash

$ erl # 確認
```




```shell
$ anyenv install exenv
$ exec $SHELL -l
$ exenv install --list
$ exenv　install 1.11.3

$ exenv global 1.11.3
```

---
config をいじって iex の表示をカスタマイズ出来る。

~/.iex.exs
```ruby
IEx.configure colors: [evalu_result: [:cyan, :bright] ]
```

---
iex でロード
```shell
$ iex
> c "somefile.exs"
```

リコンパイル
```shell
$ iex -S mix
> recompile()
```