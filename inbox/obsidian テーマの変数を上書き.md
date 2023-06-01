#obsidian 

---
2022-04-14  12:15

# obsidian テーマの変数を上書き

![[obsidian  アイコンの色をも少しなんとかしたかった]]


## checklist plugin の文字サイズがデカすぎるので小さくしたい
![[Pasted image 20220414121658.png | 200]]

developer tools で見てみると、var(--checklist-contentFontSize) をいじればいいようだ。

![[Pasted image 20220414122009.png]]

```css:.obsidian/snippets/checklist.css
:root {

    --checklist-contentFontSize: 0.7em;
}
:root .svelte-129fg97 {
    --checklist-headerFontSize: 0.6em;
}

```

![[Pasted image 20220414125727.png | 200]]


## calender の今日をなんとかしたい

色は変えれたが、weight が細くなってたのを太く。
クリックして active にするとバックの塗り潰しが他と変わらなくなって分かりにくかったので、今日のところだけ色を帰る。

```css
.today {
    --color-text-today: #ff068e;
    font-weight: 800;
}
.today.active {
    --interactive-accent: #dd039e;
}
```

![[Pasted image 20220417131450.png]]

ホバーの時も

## code のバックとフォントサイズ

![[obsidian コードのバックグラウンドを区別出来るようにしたい]]
