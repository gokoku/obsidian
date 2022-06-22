#ts #draw-library

---
2022-04-14  15:33

# ts draw library 開発中 canvas で好きなフォントに変えたい


<div class="rich-link-card-container"><a class="rich-link-card" href="http://www8.plala.or.jp/p_dolce/site3-1.html" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('http://www8.plala.or.jp/p_dolce/favicon.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">あんずいろapricot×color</h1>
		<p class="rich-link-card-description">
		
		</p>
		<p class="rich-link-href">
		http://www8.plala.or.jp/p_dolce/site3-1.html
		</p>
	</div>
</a></div>



![[スクリーンショット 2022-05-31 17.28.17.png | 300]]

可愛い日本語フォントを指定したい。
どうやるのか。

あんずもじフォントを使いたい。Font Book でインストール済み。

![[Pasted image 20220415171809.png]]


使える名前はここを見る --> PostScript 名 : APJapanesefont

![[Pasted image 20220415171850.png]]


```ts
context.font = "12px APJapanesefont"
```

これで文字を切り替えて使える。

```ts
const fontFamily = {
  gothic: '"ヒラギノ角ゴ Pro W3", "Hiragino Kaku Gothic Pro", "メイリオ", Meiryo, "ＭＳ Ｐゴシック", "MS PGothic", sans-serif',
  mincho: '"ヒラギノ明朝 Pro W3", "Hiragino Mincho Pro", "メイリオ", Meiryo, "ＭＳ Ｐ明朝", "MS PMincho", serif',
  yuru: "APJapanesefont"
}


switch ( property.option.family ) {
  case 'gothic':
    this.family = fontFamily.gothic
    break
  case 'mincho':
    this.family = fontFamily.mincho
    break
	default:
    this.family = fontFamily.yuru
}


this.context.font = `${this.weight} ${this.size}px ${this.family}`

```

![[Pasted image 20220415172513.png]]

