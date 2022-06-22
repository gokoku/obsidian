#asdf

anyenv から asdf-vm に乗り換えることにした。

[https://qiita.com/kompiro/items/5d1d631e463d46e7cace](https://qiita.com/kompiro/items/5d1d631e463d46e7cace)

言語、フレームワークまでその数や物凄いの一言。
何でもこれ一本にするため。

  

Linux でも環境を一緒にするので、github から
```shell
$ git clone https://github.com/asdf-vm/asdf.git ~/.asdf
$ cd ~/.asdf

$ git checkout "$(git describe --abbrev=0 --tags)"
```
  

.zshrc
```shell
. $HOME/.asdf/asdf.sh

fpash=(${ASDF\_DIR}/completions $fpath) # 追加した
autoload -Uz compinit
compinit
```
  

update
```shell
$ asdf update


$ asdf plugin-update ruby
```

	```shell
	$ asdf reshim (plugin) # ex. asdf reshim ruby
	```

## node

```shell
$ asdf plugin add nodejs
$ asdf list all nodejs
$ asdf install nodejs 16.2.0
```

You must install GnuPG to verify the authenticity of the downloaded archives before continuing with the install: [https://www.gnupg.org/](https://www.gnupg.org/)

とのことで、これを入れないとinstall 出来ないようだ。
```shell
$ brew install gnupg
```
  

```shell
$ asdf install nodejs 16.2.0
$ asdf list nodejs
$ asdf global nodejs 16.2.0
$ node -v
```
  
## ruby

```shell
$ asdf plugin-add ruby
$ asdf list-all ruby
$ asdf install ruby 3.0.1
$ asdf list ruby
$ asdf global ruby 3.0.1
```
  
```shell
$ exec $SHELL -l
$ ruby -v
```
  

## java

```shell  
$ asdf plugin add java
$ asdf list all java
$ asdf install java adoptopenjdk-15.0.2+7
$ asdf list java

$ asdf global java adoptopenjdk-15.0.2+7

$ javac --version
```
jdk-11 にした。VScode の java extention が動かなかったゆえ

JAVA_HOME をセットする

.zshrc
```shell
  export JAVA\_HOME=$(dirname $(dirname $(asdf which java)))
```  


$(asdf which java) で bin/java まで届くので、dirname で2回剥いた。 つまり、
/Users/george/.asdf/installs/java/adoptopenjdk-15.0.2+7/bin/java が、
/Users/george/.asdf/installs/java/adoptopenjdk-15.0.2+7 になる。

## python

```shell
$ asdf plugin add python
$ asdf list all python
$ asdf install python 3.9.5

# use zlib from xcode sdk と言われる

$ brew reinstall zlib bzip2
```
  

まだ use zlib from xcode sdk と言われる

#### reinstall xtools
```shell
$ sudo rm -rf /Library/Developer/CommandLineTools/
$ xcode-select -—install
$ sudo xcode-select —switch /Library/Developer/CommandLineTools
# OK!!
```


```shell
$ asdf global python 3.9.5
$ exec $SHELL -l
$ python —version
```
  

## elixir

  

[https://thinkingelixir.com/install-elixir-using-asdf/](https://thinkingelixir.com/install-elixir-using-asdf/)

  

Erlang からインストール

実は Erlang をインストール前にライブラリが必要のようだ。

Ubuntu

<div class="rich-link-card-container"><a class="rich-link-card" href="https://qiita.com/MzRyuKa/items/8762ea006ca446e6e422" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://qiita-user-contents.imgix.net/https%3A%2F%2Fcdn.qiita.com%2Fassets%2Fpublic%2Farticle-ogp-background-9f5428127621718a910c8b63951390ad.png?ixlib=rb-4.0.0&w=1200&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTkxNiZ0eHQ9YXNkZiVFMyU4MSVBN0VybGFuZyVFMyU4MiU5MiVFMyU4MiVBNCVFMyU4MyVCMyVFMyU4MiVCOSVFMyU4MyU4OCVFMyU4MyVCQyVFMyU4MyVBQiVFMyU4MSU5OSVFMyU4MiU4QiVFNSVBMCVCNCVFNSU5MCU4OCVFMyU4MSVBRSVFNyU5NSU5OSVFNiU4NCU4RiVFNyU4MiVCOSZ0eHQtY29sb3I9JTIzMjEyMTIxJnR4dC1mb250PUhpcmFnaW5vJTIwU2FucyUyMFc2JnR4dC1zaXplPTU2JnR4dC1jbGlwPWVsbGlwc2lzJnR4dC1hbGlnbj1sZWZ0JTJDdG9wJnM9Y2Q4YTA1OTI0ZmUwMTAyZjVmM2Y4Y2Y1M2U5ZDhjOTA&mark-x=142&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTYxNiZ0eHQ9JTQwTXpSeXVLYSZ0eHQtY29sb3I9JTIzMjEyMTIxJnR4dC1mb250PUhpcmFnaW5vJTIwU2FucyUyMFc2JnR4dC1zaXplPTM2JnR4dC1hbGlnbj1sZWZ0JTJDdG9wJnM9Mzc0YWQ1OGIzZTlkNzM0OGJlZDc5NmZhMDI4M2Y4YjE&blend-x=142&blend-y=491&blend-mode=normal&s=78e3d1f49740299ba76014ec8590bf56')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">asdfでErlangをインストールする場合の留意点 - Qiita</h1>
		<p class="rich-link-card-description">
		Elixirの動作環境をasdfで構築していた際に気づいた点。 最初に結論  Elixirを動作させるためには、事前にErlangのインストールが必要です。 しかし、asdfのインストール手順に書かれている手順でインストールされる...
		</p>
		<p class="rich-link-href">
		https://qiita.com/MzRyuKa/items/8762ea006ca446e6e422
		</p>
	</div>
</a></div>

```shell
$ sudo apt install build-essential autoconf m4
$ sudo apt install libncurses5-dev
$ sudo apt install libwxgtk3.0-gtk3-dev
$ sudo apt install libwxgtk-webview3.0-gtk3-dev
$ sudo apt install libgl1-mesa-dev
$ sudo apt install libglu1-mesa-dev
$ sudo apt install libpng-dev libssh-dev
$ sudo apt install unixodbc-dev
$ sudo apt xsltproc fop
```


OSX
 ```shell
 $ brew install autoconf
 $ brew install wxwidgets
 $ brew install wxidgets
 $ brew install libxslt fop
```
```shell
$ asdf plugin add erlang
$ asdf list all erlang
$ asdf install erlang 24.0
```
けっこうかかったし、引っかかった

  
```shell
$ asdf global erlang 24.0
```

erlang の 24.0 が引っかかる。

```shell
$ brew install unixodbc fop wxwidgets libxslt

$ CPP=cpp EGREP=egrep asdf install erlang latest:24
```

*wx : wxWidgets was not compiled with --enable-webview or wxWebView developer package is not installed, wxWebView will NOT be available

と出るが、ビルド出来てしまうので、まあいっか。

```shell
$ brew install wxmac
```

**ubuntu**


<div class="rich-link-card-container"><a class="rich-link-card" href="https://github.com/asdf-vm/asdf-erlang#before-asdf-install" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://opengraph.githubassets.com/c5957ffa6bf3fa423d3495862ba8b17a5b7be4411ab6d6243d5376f367f3f873/asdf-vm/asdf-erlang')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">GitHub - asdf-vm/asdf-erlang: Erlang plugin for asdf version manager</h1>
		<p class="rich-link-card-description">
		Erlang plugin for asdf version manager. Contribute to asdf-vm/asdf-erlang development by creating an account on GitHub.
		</p>
		<p class="rich-link-href">
		https://github.com/asdf-vm/asdf-erlang#before-asdf-install
		</p>
	</div>
</a></div>

どうもErlang のコンパイルが上手くいってないっぽい
```shell
APPLICATIONS INFORMATION (See: /home/george/.asdf/plugins/erlang/kerl-home/builds/asdf_25.0/otp_build_25.0.log)
 * crypto         : Using OpenSSL 3.0 is not yet recommended for production code.
 * wx             : wxWidgets was not compiled with --enable-webview or wxWebView developer package is not installed, wxWebView will NOT be available
```

```shell
$ sudo apt install libwxgtk-webview3.0-gtk3-dev
```
OpenSSL 3.0 はまだ推奨されてないとのことらしいが、まあいいか。


Elixir をインストール
```shell
$ asdf plugin add elixir
$ asdf list all elixir
$ asdf install elixir 1.12.0
$ asdf global elixir 1.12.0
```  
