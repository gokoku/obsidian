#flutter

---
2021-12-23

# カメラを使う

https://qiita.com/konatsu_p/items/eb0e8a7ab62ab9d31315

```shell
$ flutter pub get

$ 
```


VScode でこんなエラーが出た。

[Failed to start DevTools: Dart DevTools exited with code 255](https://stackoverflow.com/questions/70446275/failed-to-start-devtools-dart-devtools-exited-with-code-255)


iOS では、Team を設定してからデバッグRUN走らせることで実機にインストールできるようになる。


## 公式ドキュメント
https://docs.flutter.dev/cookbook/plugins/picture-using-camera

```yaml
dependencies:
  flutter:
    sdk: flutter
  camera:
  path_provider:
  path:
```

Android のバージョンを minSdkVersion 21 にするとのこと。

iOS は ios/Runner/Info.plist にカメラアクセス要求を記述。

```
<key>NSCameraUsageDescription</key>
<string>Explanation on why the camera access is needed.</string>
```

