#flutter  #iphone

iPhone 実機にビルドしようとしてエラー。

```shell
Flutter/Flutter.h が見つからんよ!!

Could not build the precompiled application for the device.

Error launching application on Geore-iPhone12.
```
cocoapods まわりのなんかだったようだが、謎。

```shell
$ rm ios/Flutter/Flutter.podspec
$ flutter clean
```

OK!!

