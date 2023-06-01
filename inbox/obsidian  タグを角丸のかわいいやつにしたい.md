#obsidian

---
2021-05-27

# タグを可愛くしたい

https://forum.obsidian.md/t/meta-post-common-css-hacks/1978/13

この人の Hacks がすごい。
けど、html のシンタックスハイライトにも .tag クラスが付いてエライことになったので、a タグに絞った。
```css
a.tag {
  background-color: var(--text-accent);
  border: none;
  color: white;
  font-size: 11px;
  padding: 1px 8px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  margin: 0px 0px;
  cursor: pointer;
  border-radius: 14px;
}
a.tag:hover {
color: white;
background-color: var(--text-accent-hover);
}
a.tag[href^="#n"] {
  background-color: [[4d3ca6]];
}
a.tag[href^="#t"] {
  background-color: red;
}
a.tag[href^="#e"] {
  background-color: green;
}
a.tag[href^="#s"] {
  background-color: orange;
}
```

デフォルトのテーマが一番気に入ったので、これを追加する。

![[Pasted image 20210527163656.png]]

.obsidian/snippets/

CSS snipet フォルダがあるので、この中に tag.css としてこのファイルをコピペした。

![[Pasted image 20210527163803.png]]

これだけで出来てしまった。

---
## 言語ごとにわけてみる

Electron のようなので、devtools を出してみる。
めんどくさいので、言語だけにしよう。


---
## code
.obsidian/snippets/tag.css

```css
a.tag {
  /*background-color: var(--text-accent);*/
  background-color: [[6d53f7]];
  border: none;
  color: white;
  font-size: 12px;
  padding: 1px 8px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  margin: 0px 0px;
  cursor: pointer;
  border-radius: 14px;
  transition: opacity 0.8s;
}
a.tag:hover {
  color: white;
  /*background-color: var(--text-accent-hover);*/
  opacity: 0.6;
}
a.tag[href="#js"] {
  background-color: #168062;
}
a.tag[href="#n"] {
  background-color: [[4d3ca6]];
}
a.tag[href="#css"] {
  background-color: green;
}
a.tag[href="#r"] {
  background-color: [[8e1dbd]];
}
a.tag[href="#a"] {
  background-color: green;
}
a.tag[href="#p"] {
  background-color: [[ab9011]];
}
a.tag[href="#y"] {
  background-color: [[b73535]];
}
a.tag[href="#command"] {
  background-color: [[0465c7]];
}
a.tag[href="#p"] {
  background-color: [[1e76d0]];
}
a.tag[href="#o"] {
  background-color: [[0e7ece]];
}
a.tag[href="#haskell"] {
  background-color: [[c3830e]];
}
a.tag[href="#a"] {
  background-color: [[bd1db9]];
}
a.tag[href="#sass"] {
  background-color: [[c74c1d]];
}
a.tag[href="#r"] {
  background-color: [[d4459b]];
}
```