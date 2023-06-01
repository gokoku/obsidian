#flutter 

---
2021-12-23

# Flutter 再入門

```shell
$ flutter create my_app
$ cd my_app

$ flutter run
```

ios で実機インストール失敗する。

```
════════════════════════════════════════════════════════════════════════════════
Building a deployable iOS app requires a selected Development Team with a 
Provisioning Profile. Please ensure that a Development Team is selected by:
  1- Open the Flutter project's Xcode target with
       open ios/Runner.xcworkspace
  2- Select the 'Runner' project in the navigator then the 'Runner' target
     in the project settings
  3- Make sure a 'Development Team' is selected under Signing & Capabilities > Team. 
     You may need to:
         - Log in with your Apple ID in Xcode first
         - Ensure you have a valid unique Bundle ID
         - Register your device with your Apple Developer Account
         - Let Xcode automatically provision a profile for your app
  4- Build or run your project again

For more information, please visit:
  https://flutter.dev/docs/get-started/install/macos#deploy-to-ios-devices

Or run on an iOS simulator without code signing
════════════════════════════════════════════════════════════════════════════════

Error launching application on George-iPhone12.
```

```shell
$ open ios/Runner.xcworkspace/


```

![[Pasted image 20211223171156.png]]

Team をセットして、VScode でデバッグrun したら、動いた!!

一旦通した。

