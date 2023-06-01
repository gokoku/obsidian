---
type: note
---

#flutter

---
2023-06-01  13:04

# flutter  開発環境セッティング

Get the Flutter SDK

https://docs.flutter.dev/get-started/install/macos

asdf で install する

```shell
$ asdf plugin add flutter

# jq がないと言われる npm で入れて上手くいかなかった
$ brew install jq

$ asdf install flutter 3.10.2-stable
$ asdf global flutter latest
```

一旦 asdf plugin remove flutter と消してから add すると上手くいく。

![[Pasted image 20230601132857.png]]

```shell
$ flutter doctor
Androidなんとかする
Xcode なんとかする
```

## android studio setup 
```shell
$ brew install android-studio
```

で Android Studio を立ち上げる。
~/Library/Android/sdk が入る。

Flutter plugin をインストール

Flutter new project をクリックして、

Flutter の設定のところに

![[Pasted image 20230601134633.png]]
```shell
/Users/george/.asdf/installs/flutter/3.10.2-stable
```

Android simulator で Flutter Hello world できた。


https://zenn.dev/imasaka0909/articles/00ebfaf74f9cea



<div class="rich-link-card-container"><a class="rich-link-card" href="https://zenn.dev/imasaka0909/articles/00ebfaf74f9cea" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://res.cloudinary.com/zenn/image/upload/s--cUViFa6N--/c_fit%2Cg_north_west%2Cl_text:notosansjp-medium.otf_55:%25E3%2580%2590Mac%2520M1%25E3%2580%2591%25E3%2580%258Cflutter%2520doctor%25E3%2580%258D%25E5%25AE%259F%25E8%25A1%258C%25E6%2599%2582%25E3%2581%25AE%25E3%2580%258Ccmdline-tools%2520component%2520is%2520m...%2Cw_1010%2Cx_90%2Cy_100/g_south_west%2Cl_text:notosansjp-medium.otf_37:%25E3%2582%25A4%25E3%2583%259E%25E3%2582%25B5%25E3%2582%25AB%2Cx_203%2Cy_98/g_south_west%2Ch_90%2Cl_fetch:aHR0cHM6Ly9saDMuZ29vZ2xldXNlcmNvbnRlbnQuY29tL2EtL0FPaDE0R2pyX3U3QmxSZTJBMnRYRFJCVWdvLVg2QlZmaGRuSnNfZUdZYXZhPXM5Ni1j%2Cr_max%2Cw_90%2Cx_87%2Cy_72/og-base.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">【Mac M1】「flutter doctor」実行時の「cmdline-tools component is missing」の解決法</h1>
		<p class="rich-link-card-description">
		
		</p>
		<p class="rich-link-href">
		https://zenn.dev/imasaka0909/articles/00ebfaf74f9cea
		</p>
	</div>
</a></div>


command line tool をインストール
![[Pasted image 20230601141117.png]]

これで command line tool はセット完了。
```shell
$ flutter doctor --android-licenses
Y
Y
Y....
```

これでandroid関係はオールGreen

## Xcode setting
```shell
$ sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer
$ sudo xcodebuild -runFirstLawnch
```

https://zenn.dev/imasaka0909/articles/cae09eb794340d

No Cocoapods installed.
```shell
$ brew install cocoapods
```

```shell
$ flutter docer

オールGreen!!!
```




# Flutter Hello World

```shell
$ flutter create my_app
$ cd my_app
$ flutter run
```
![[Pasted image 20230601142433.png]]

iOS Simulator で立ち上がった!!

## iPhone 実機

Xcode ->Window-> Devices and Simulators
![[Pasted image 20230601142857.png]]
![[スクリーンショット 2023-06-01 14.30.03.png]]

Connect via Network にチェック入れる。



# VSCode setting

![[Pasted image 20230601142016.png]]
settings では path を設定しない。

https://github.com/oae/asdf-flutter
asdf-flutter に書いてた通りにする。

```shell
export FLUTTER_ROOT="$(asdf where flutter)"
```

![[Pasted image 20230601153847.png]]


通った!!!!

