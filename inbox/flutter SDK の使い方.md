#flutter 

flutter を github から clone した。

[https://github.com/flutter/flutter.git](https://github.com/flutter/flutter.git)

PATH を通す。

```bash
$ flutter upgrade
$ flutter channel   # git のブランチっぽい
$ flutter channel master  # master branch checkout

$ flutter --version
Flutter 1.22.5
Tools * Dart 2.10.4
```

Dart は brew から uninstall して、Flutter の Dart を使ってる。

---

```bash
$ flutter pub get  # npm install にあたるやつ

$ flutter create my_app
$ cd my_app
$ flutter run

d でターミナルは戻ってくるが、アプリは running
q で quit
```

---

VSCode を使う。

![](image-kn9m9xq3.png)

Run —> Start Debugging

Hot reload で保存で更新が走る。

![[Pasted image 20210525162246.png]]


ここで、端末 emulator を選べる。

---

chrome をクライアントに選べる。

```bash
$ flutter channel beta
$ flutter upgrade
$ flutter config --enable-web
```

vscode で chrome が選択できるようになってた。

![](image-kn9matpi.png)

![](image-kn9mb27c.png)

デバッグ run すると2度目で起動した。

