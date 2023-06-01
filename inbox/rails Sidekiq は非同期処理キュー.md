---
type: note
---

#rails

---
2022-12-02  20:07

# rails Sidekiq は非同期処理キュー

https://dev.icare.jpn.com/dev_cat/sidekiq/

[https://github.com/mperham/sidekiq（公式リポジトリ）](https://github.com/mperham/sidekiq)

例えばプリンタで100ページ分印刷したいときに、印刷が終わるまでブラウザで次のページに行けなかったりキーボードで文字が入力できなかったらすごく不便ですよね。  
コンピュータが処理を行う速度と紙に印刷する速度は、印刷する速度の方が圧倒的に遅いので、100ページ分印刷する処理は「後で処理するリスト」に入れておき、バックグラウンドで並列して実行していきます。

「後で処理するリスト」にはRedisのキューがよく使われます。これから処理していく予定のJobのデータをRedisに永続化することで、Railsプロセスが落ちたとしてもエンキューされたJobのデータが失われることはありません。

うん、わかりやすい。

![](https://d39kau1ie0fw3g.cloudfront.net/wp-content/uploads/2021/11/30230056/sidekiq%E3%81%AE%E8%A8%98%E4%BA%8B%E3%81%AE%E5%9B%B3-600x294.png)

