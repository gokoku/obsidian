#css

---
2021-06-02

# cssセレクタのワイルドカード
https://www.nishi2002.com/6976.html

obsidian の tag に色を付ける設定をした。

```css
a.tag[href="#y"] {
  background-color: [[b73535]];
}
```

obdisian のタグが階層で指定出来ると知り、階層化した。

```
#s 
#c
```

これらもセレクタで拾ってほしい。

```css
a.tag[href*="#y"] {
  background-color: [[b73535]];
}
```

これでOK