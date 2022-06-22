#command 

---
2021-11-17

# cURL オプション

https://qiita.com/ryuichi1208/items/e4e1b27ff7d54a66dcd9

```shell

# HTTPリクエストを実施し結果を標準出力へ
$ curl http://対象のURL

#コンマや[]を使って範囲指定も出来る
$ curl 'http://{one,two,three}.example.com'
$ curl 'http://[1-3].example.com'

# 実行結果をファイルへ出力
$ curl http://対象のURL > 出力先
$ curl -o 出力先PATH http://対象のURL

# ファイル出力時の進捗状況を非表示にする(エラーも非表示)
$ curl -s -o 出力先PATH http://対象のURL

# 上記でエラーは表示したい場合
$ curl -sS -o 出力先PATH http://対象のURL

# プログレスバーで進捗率を表示
$ curl -# -O http://対象のURL

# SSL接続で証明書エラーをスキップ
$ curl -k https://対象のURL

# URLのファイル名でダウンロード (下記はindex.htmlで保存される)
$ curl -O http://対象のURL/index.html

# プロキシ経由でアクセスする
$ curl -x プロキシサーバ:ポート番号 --proxy-user ユーザ名:パスワード http://対象のURL

# リダイレクトを有効にする
$ curl -L http://対象のURL

# ダウンロードを中断したときに再度ダウンロードを再開するとき
$ curl -C - http://対象のURL

# HTTPメソッドの指定（-X）
$ curl -X PUT http://対象のURL
```

### 主要

```shell
-L -- リダイレクトがあったらリダイレクト先の情報を取る
-s -- 余計な出力をしない
-o -- レスポンスボディの出力先を指定する
```


### デバッグ

```shell
# HTTPレスポンスヘッダーの取得（-I）
$ curl -I http://対象のURL

# 詳細をログ出力（-vもしくは--verbose）
$ curl -v http://対象のURL

# 終了ステータスのみを表示
$ curl -s http://対象のURL -o /dev/null -w '%{http_code}\n'
```