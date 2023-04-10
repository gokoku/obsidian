---
type: note
---

#css

---
2022-07-27  17:34

# css 縦リストがするっとはじから出てくるアニメーション

![[Pasted image 20220727173951.png]]

このリストが表示されるときに、スルッと左から出てくるアニメーションを css でつけた。

```css
/* 画像リストのアニメーション */
@keyframes open-parts-list {
  0% {
    opacity: 0;
    transform: translateX(-100px);
  }
  100% {
    opacity: 1;
    transform: translateX(0);
  }
}
#parts-list ul {
  animation: open-parts-list 0.5s ease-in-out;
  ...
}
```

たったこれだけでスルッと可愛いアクションしてくれる!!

