#js #web

---
2021-05-28

# ブロックで連結できるオンラインjsエディタ


https://paiza.hatenablog.com/entry/2021/05/26/153000

https://natto.dev/d/78978a59754d46b98155259b8a21eb20

![[Pasted image 20210528091826.png]]

右クリックで add eval
![[Pasted image 20210528092402.png]]

# 使ってみる

岩手の天気情報 json を取得する。

![[Pasted image 20210528092847.png]]
```javascript
fetch('https://www.jma.go.jp/bosai/forecast/data/overview_forecast/030000.json').then(data => data.json())
```

それぞれの JSONデータを抽出。

![[Pasted image 20210528093513.png]]

add eval のオプションで HTML を選択。

![[Pasted image 20210528093545.png]]

線を集められる。

![[Pasted image 20210528093839.png]]

線の値の名前をわかりやすく出来る。

![[Pasted image 20210528093931.png]]

HTMLレイアウトはjs のテンプレートの形で書く。

![[Pasted image 20210528095252.png]]


## パッケージを import する

date を moment いや、luxon でformatする。

add define でcdn で import する。

defineName で as luxon。

![[Pasted image 20210528095854.png]]

format整形する。

![[Pasted image 20210528100613.png]]

```javascript
luxon.DateTime.local().toFormat('yyyy/MM/dd')
```

![[Pasted image 20210528100716.png]]

日付が見やすくなった。

## 書き出し

![[Pasted image 20210528101437.png]]

![[Pasted image 20210528101519.png]]

これはよくわからなかった。動かせなかった。

