#css

---
2021-06-11

# メディアクエリの今設定はどーなってる?

https://www.seojuku.com/blog/responsive-mediaquery.html

```css
@media screen and (max-width: 1024px) { }
@media screen and (max-width: 896px) { }
@media screen and (max-width: 480px) { }
```

```css
.container { max-width: 1024px; }
```

![[Pasted image 20210611111313.png]]

```css
メディアクエリなし 【スマートフォン縦】
@media screen and (min-width: 481px) { } 【スマートフォン横】
@media screen and (min-width: 769px) { } 【タブレット縦以上】
```

これで設定してみよう。
