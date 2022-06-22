#ts #js 

---
2021-12-06

# 一段上のPromise活用テクニック

https://ics.media/entry/211203/

## モーダル

アンケートとか簡単に出来そう。全てのコールバックは Promise に出来るとのこと。

[[ts Promise でモーダル]]

## Promise で API を確実にキャッシュして通信を抑える

Promise を保持して、何度でも await 出来るんだって。

![データではなく「データを返すPromise」を返す](https://ics.media/entry/211203/images/211203_share_promise.png)

[[ts  Promise で API を確実にキャッシュして通信を効率的にする]]


## Promiseで同時処理数をコントロールする

これは結構入り組んでいる。

Promise は resolve がないと終わらなで待ってるとすると、わかるかな。

[[ts   Promiseで同時処理数をコントロールする]]
