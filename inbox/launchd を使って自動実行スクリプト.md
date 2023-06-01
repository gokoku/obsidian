---
type: note
---

#command #osx

---
2022-06-02  15:41

# launchd を使って自動実行スクリプト

launch用 plist ファイルの置き場。

![[Pasted image 20220602163755.png]]

## 登録の仕方
```shell
$ launchctl load ~/Library/LaunchAgents/my_script.plist

$ launchctl start ジョブラベル
```

## 停止の仕方
```shell
$ sudo launchctl unload ~/Library/LaunchAgents/my_script.plist
```

### ジョブ一覧
```shell
$ launchctl list
```

### デバッグ
コンソールで system.log の中からファイル名で検索( ex. my_sciprt.plist)

標準エラー出力でデバッグが捗る。

```xml
<key>StandardOutPath</key>
<string>/tmp/obc.log</string>
<key>StandardErrorPath</key>
<string>/tmp/obc.error</string>
```

```shell
$ tail -f -n 100 /tmp/obc.log
```

