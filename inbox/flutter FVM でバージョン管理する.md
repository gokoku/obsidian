---
type: note
---

#flutter

---
2022-04-30  21:03

# flutter FVM でバージョン管理する

<div class="rich-link-card-container"><a class="rich-link-card" href="https://fvm.app/" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://fvm.app/img/share-image.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">fvm | Flutter Version Management</h1>
		<p class="rich-link-card-description">
		A simple CLI to manage Flutter SDK versions.
		</p>
		<p class="rich-link-href">
		https://fvm.app/
		</p>
	</div>
</a></div>


<div class="rich-link-card-container"><a class="rich-link-card" href="https://zenn.dev/riscait/articles/flutter-version-management" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://res.cloudinary.com/zenn/image/upload/s--Io1cJaFC--/co_rgb:222%2Cg_south_west%2Cl_text:notosansjp-medium.otf_37_bold:%25E6%259D%2591%25E6%259D%25BE%25E9%25BE%258D%25E4%25B9%258B%25E4%25BB%258B%2Cx_203%2Cy_98/c_fit%2Cco_rgb:222%2Cg_north_west%2Cl_text:notosansjp-medium.otf_70_bold:FVM%25E3%2581%25A7Flutter%2520SDK%25E3%2581%25AE%25E3%2583%2590%25E3%2583%25BC%25E3%2582%25B8%25E3%2583%25A7%25E3%2583%25B3%25E3%2582%2592%25E3%2583%2597%25E3%2583%25AD%25E3%2582%25B8%25E3%2582%25A7%25E3%2582%25AF%25E3%2583%2588%25E6%25AF%258E%25E3%2581%25AB%25E7%25AE%25A1%25E7%2590%2586%25E3%2581%2599%25E3%2582%258B%2Cw_1010%2Cx_90%2Cy_100/g_south_west%2Ch_90%2Cl_fetch:aHR0cHM6Ly9yZXMuY2xvdWRpbmFyeS5jb20vemVubi9pbWFnZS9mZXRjaC9zLS1VNkpQbk1PNy0tL2NfbGltaXQlMkNmX2F1dG8lMkNmbF9wcm9ncmVzc2l2ZSUyQ3FfYXV0byUyQ3dfNzAvaHR0cHM6Ly9zdG9yYWdlLmdvb2dsZWFwaXMuY29tL3plbm4tdXNlci11cGxvYWQvYXZhdGFyLzAyNzY0MzlkYzYuanBlZw==%2Cr_max%2Cw_90%2Cx_87%2Cy_72/v1627274783/default/og-base_z4sxah.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">FVMでFlutter SDKのバージョンをプロジェクト毎に管理する</h1>
		<p class="rich-link-card-description">
		
		</p>
		<p class="rich-link-href">
		https://zenn.dev/riscait/articles/flutter-version-management
		</p>
	</div>
</a></div>



#### iMac
dart が brew で入ってたので、削除した。
flutter sdk に dart は入ってるので、よく分からなくなるかもということで uninstallしてからの、
```shell
$ brew tap leoafarias/fvm
$ brew isntall fvm
```

Ubuntu系
[[utuntu  Linuxbrew を install する]]
で一緒。

### path を通す
これで FVM を使えるようにするとのこと。

```
export PATH=$HOME/.pub-cache/bin:$PATH
```

```shell
FVMのパージョンを上げる

$ dart pub global activate fvm
```

```shell

$ fvm releases

# 2.10.5 stable とあるから install してみる

$ fvm install 2.10.5

$ fvm use 2.10.5 --force  # mac は force 入れないとダメだった。

$ fvm list

2.10.5 (active)



$ fvm flutter --version
Flutter 2.10.5
```

## flutter コマンドを使えるようにする

fvm flutter とやらなきゃ flutter が使えないので、global 設定すると flutter と打っても使えるらしい。

```shell
$ fvm global stable
```

.zshrc
```
export PATH=$PATH:$HOME/fvm/default/bin
```

これで、普通に flutter でも良く成った。
```shell
$ flutter doctor
```

android toolchain でワーニング
```shell
$ ~/Library/Android/sdk/tools/bin/sdkmanager --install "commandline-tools;latest"
```

sdkmanager コマンドでは上手くいかなかった。

Android Studio から Preferences→Appearance & Behavior —>System Settings —> Android SDK —> SDK Tools でCommand-line tools を入れる。

(Command-line tools が出なかった。apply をクリックしたら入ってきた)

![[Untitled.png]]

.zshrc
```
export PATH=$PATH:$ANDROID_HOME/cmdline-tools/latest/bin
```
多分、sdk/tools/bin の sdkmanager とかは古い。
cmdline-tools/の方を先に見つけるようなパスをセットすればいいのかな。
あるいは重なってるコマンドを片付けるか。

```shell
$ flutter doctor --android-licenses

$ flutter doctor

Doctor summary (to see all details, run flutter doctor -v):
[✓] Flutter (Channel stable, 2.10.5, on macOS 12.3.1 21E258 darwin-x64, locale ja-JP)
[✓] Android toolchain - develop for Android devices (Android SDK version 31.0.0)
[✓] Xcode - develop for iOS and macOS (Xcode 13.3.1)
[✓] Chrome - develop for the web
[✓] Android Studio (version 2021.1)
[✓] VS Code (version 1.66.2)
[✓] VS Code (version 1.45.0)
[✓] VS Code (version 1.45.0)
[✓] VS Code (version 1.50.0)
[✓] Connected device (1 available)
[✓] HTTP Host Availability

• No issues found!
```

やりましたな。

## VSCode で 使う

.gitignore に追記
```
.fvm/flutter_sdk
```

settings.json

```json
{
	...
  "dart.flutterSdkPath": ".fmv/flutter_sdk",
  ...
}
```

