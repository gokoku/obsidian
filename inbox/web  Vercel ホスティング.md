#web #vercel

---
2021-07-07

# Vercel (ヴァーセル)
https://vercel.com/pricing

Next.js を開発してる会社が立ててるらしい。

![[Pasted image 20210707113937.png]]

お一人様 developer なら Free 枠でかなり良さげ。

Vue.js でデプロイ出来るようだ。

template としてフレームワークを使える形。

![[Pasted image 20210707114219.png]]

## Vue.js で入ってみる

![[Pasted image 20210707114440.png]]

GitHub で入る。

![[Pasted image 20210707114626.png]]

install Vercel ってなに?

![[Pasted image 20210707115139.png]]

こっちが Vercel に GitHub からクローンしてデプロイするのか。

![[Pasted image 20210707115313.png]]

成功したみたいだ。

![[Pasted image 20210707115337.png]]

デプロイした。

![[Pasted image 20210707115404.png]]

ダッシュボード

![[Pasted image 20210707115501.png]]

## GitHub はどーなってるんだ?

![[Pasted image 20210707115720.png]]

vue-sample が作られてる。

```shell
$ git clone git@github.com:gokoku/vue-sample.git

$ cd vue-sample
$ npm install

$ npm run serve
```



デプロイしてみる。

```shell
$ vue create
あかん。template が Vue 2だ。


$ npm i -g vercel
$ asdf reshim
$ vercel
```

Vercel に Push したんかな?

https://vercel.com/dashboard

なんかDELETEしたら更新された。

## 既存のプロジェクトをデプロイ

![[Pasted image 20210707130157.png]]

import してみる。

![[Pasted image 20210707130221.png]]

Deploy をクリックしてみる。

![[Pasted image 20210707133834.png]]


Deploy 出来た。

https://myweather-mu.vercel.app/


---

# Free プラン

## 機能

-   HTTPS 対応のカスタムドメイン
-   GitHub・GitLab・BitBucket と連携した継続的デプロイメント
-   高機能なエッジネットワーク
-   無制限の Webサイト と API
-   Node.js や Go を使用したサーバレス関数

## 制限

-   サーバレス関数のサイズ：最大 50 MB
-   サーバレス関数のメモリ：最大 1,024 MB
-   サーバレス関数の同時実行性：最大 1,000
-   サーバレス関数が受け取るペイロードサイズ：最大 5 MB
-   1日あたりのビルド数：100
-   1時間あたりのビルド数：32
-   1時間あたりのフックトリガーからのデプロイ回数：60
-   1日あたりのデプロイ数：100
-   1時間あたりのデプロイ数：100


かなり自由だ。