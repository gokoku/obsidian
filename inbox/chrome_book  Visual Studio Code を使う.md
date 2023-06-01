#chrome_book 


![](image-kn6zqkr8.png)

.debパッケージをダウンロードして

```shell
$ sudo apt install ....deb
```

ストアからインストールしたが、動かなかった。
これで Linux 上から起動出来る。

---
## terminal font

これを使いたい。シンボルアイコンが化けないように。

./local/share/fonts/Droid Sans Mono Powerline Nerd Font Complete.otf



フォント名は
```shell
$ fc-list | grep -i nerd
/home/george/.local/share/fonts/Droid Sans Mono Powerline Nerd Font Complete.otf: DroidSansMono Nerd Font:style=Book
```
:以下が名前らしい。

VSCode にこれを書く。

コマンドパレットから setting で

Preferences: Open Settings(JSON) を開く。

```json
"terminal.integrated.fontFamily": "DroidSansMono Nerd Font"
```

これでOK!!
#### ところが
sync してると Mac と違うので、化ける。

 Hack Nerd Font Mono をダウンロードする。
 
 https://github.com/ryanoasis/nerd-fonts/tree/master/patched-fonts/Hack
 
 https://github.com/source-foundry/Hack/releases/download/v3.003/Hack-v3.003-ttf.zip

```shell
$ unzip Hack-v3.003-ttf.zip

$ cd ttf
$ mv *.ttf ~/.local/share/fonts
$ fc-cache -f -v

$ fc-list | grep "Hack"
あるあるOK!!
```
Hack Nerd Font Mono がない。

Github の nerd-fonts-source-code-pro をクリックした。
ここから、 nerd-fonts-source-code-pro-2.1.0.zip をダウンロードした。
同じく .local/share/fonts に ttf を全部移動した。
だめだった。



---
## Flutter で実機がビルドエラー

```shell
FAILURE: Build failed with an exception.

* What sent wrong:
Could not open settings generic class cache for settings file '/home/george/Desktop/my_app/android/settings.gradle' (/home/george/.gradle/caches/6.7/scripts/f0....).
> BUG! exception in phase 'semantic analysis' in source unit '_BuildScript_' Unsupported class file major version 60

* Try
Run with --stacktrace option to get the stack trace. Run with --info or --debug optionto get nore log output. Run with --scan to get full insights.

* Get more help at https://help.gradle.org

BUILD FILED in 8s
Exception: Gradle task assembleDebug failed wight exit code 1
Exited(sigterm)
```

