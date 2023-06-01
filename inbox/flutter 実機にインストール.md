#flutter 

[https://note.com/nbht/n/nd4338701daa5](https://note.com/nbht/n/nd4338701daa5)

```bash
$ flutter build apk    # Android用
$ flutter build ios    # iOS用

どのデバイスにインストールするか確認

$ flutter devices
sdk gphone x86 arm (mobile) • emulator-5554             • android-x86    • Android 11 (API 30) (emulator)
George-iPhone12 (mobile)    • 00008101-000A18DC1081401E • ios            • iOS 14.3
Chrome (web)                • chrome                    • web-javascript • Google Chrome 87.0.4280.141
```

頭文字だけで指定出来るとのこと。

iPhone に入れたかったので、0 で指定してみる。

```bash
$ flutter install -d 0

```

実機にインストールされた　OK!!

