#obsidian 

---
2021-05-27

# コードのバックグラウンドカラーとフォントサイズを変える code

このテーマ真っ黒でお気に入りだが、コードのバックも真っ黒で区別がつくように出来たらなーと思った。

フォントサイズも変えた。

.obsidian/snippets/code.css
```css
.theme-dark pre[class*="language-"] {
  background: #383838;
  font-size: 0.8em;
}
```

### 設定
![[Pasted image 20210527174503.png]]

Apparance --> css snipet で再読み込みすると、加えたcssファイルが見えるようになるので、ON にする。