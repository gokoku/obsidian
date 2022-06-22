#chrome

# 9 Hidden Features of Chrome DevTools

https://javascript.plainenglish.io/9-hidden-features-of-chrome-devtools-78856b2a96de

面白そうなやつをピックアップ

### 5. Detecting Unused Code

![](2021-05-11-9-57-30-kojbrlfb.png)

Coverrage をクリック。

![](image-kojbtbbz.png)

赤が使われてないコードらしい。

しかも、グラフをクリックすると、ソースでその箇所が見られる。

![](image-kojc11ro.png)

### 9. Breakpoints

右上のメニューをクリックして、

![](image-koje1zgo.png)

Show media queries を選ぶ。

設定されたメディアクエリーのレスポンシブ対応の様子が上のバーで一目瞭然。

![](image-koje4v8u.png)

バーをクリックするとその大きさの幅になる。

![](image-kojeopn9.png)
どのファィルで設定してあるかわかる。



バーが3つあるのはなんだろう。

![](image-kojeq203.png)

一本のバーの箇所でも、複数のファイルで定義されてるし。

* Blue : ~px以下            : max-width
* Green : ~px以上 ~px 以下   : min-width and max-width
* オレンジ : ~px以上          : min-width

のようだ。

```scss
@media screen and (max-width: 300px) {
  #wrap {
    color: blue;
  }
}
@media screen and (min-width: 400px) and (max-width: 800px) {
  #wrap {
    color: green;
  }
}
@media screen and (min-width: 1000px) {
  #wrap {
    color: orange;
  }
}
```

![](image-kojfajx4.png)
