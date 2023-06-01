---
type: note
---

#elixir #asdf

---
2022-09-22  11:42

# elixir. asdf でインストールするとなんか iOSで引っかかるとか言ってたけど解決してるみたい

https://github.com/elixir-desktop/ios-example-app#how-to-build--run


```shell
$ brew install carthage git openssl@1.1

$ asdf install erlang 25.0.4
$ asdf install elixir 1.13.4-otp-25
```

もし、うまくいかなかったら
```shell
$ export DED_LDFLAGS_CONFTEST="-bundle"
$ export KERL_CONFIGURE_OPTIONS="--without-javac --with-ssl=$(brew --prefix openssl@1.1)"
$ asdf install erlang 25.0.4
$ asdf install elixir 1.13.4-otp-25
```

```shell
$ git clone https://github.com/elixirdesktop/ios-example-app.git

$ cd ios-example-app
$ carthage update --use-xcframeworks

error!!
xcrun: error: unable to find utility "xcodebuild", not a developer tool or in PATH
```

Xcode をの preferences の Locations で Command Line Tools を選んでやるとうまくいった。


![[スクリーンショット 2022-09-22 13.24.22.png | 500]]
```shell
$ carthage update --use-xcframeworks
```

todoapp.excodeproj を xcode で起動すると失敗する。
simualtors に tvOS がなかったから build が止まったらしい。


### xcode で simulators に tvOS をインストール

![[Pasted image 20220922135303.png]]

watchOS と tvOS をインストールした。
プロテクトが検証で全然進まないので、強制ダウンさせて、xcode を再起動した。


```shell
$ carthage update --use-xcframeworks
```

Carthage/Build/ZIPFoundation.xcframework が出来た。

![[Pasted image 20220922143642.png | 300]]

凄い!! Elixir で iOS が動いてる。


実機にインストール出来んかった。
