#css

---
2021-07-30

# Grid の使い方

[[css   Create 3D Parallax Effect in 5 Minutes – Tilt.js Tutorial]]

こちらからカードデザインで学ぶことにする。

![[Pasted image 20210730104411.png]]

```html
    <div class="card">
      <div class="card-image"></div>
      <div class="card-text">
      </div>
      <div class="card-stats">
        <div class="stat">
        </div>
        <div class="stat border">
        </div>
        <div class="stat">
        </div>
      </div>
    </div>

```

Flex がないと、幅一杯になるので。

```css
body {
  width: 100vw;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}
```

https://qiita.com/kura07/items/e633b35e33e43240d363

Grid コンテナの中に、アイテムがある。


## Grid 分割

グリッドをコラムデザインのように、縦、横を指定する。

```css
grid-template-rows: 210px 210px 80px;  縦方向
grid-template-column: 300px;           横方向
```

![[Pasted image 20210730110028.png]]

1fr は残りとかの幅。


## エリア名を付ける

```css
grid-template-areas: "image" "text" "stats";
```

![[Pasted image 20210730111136.png]]

縦方向 row は、クォートで区切り、横方向 column はスペースで区切る。
形的には Table タグのようなイメージだ。

```css
grid-template-areas:
	"image text"
	"image stats"
```

![[Pasted image 20210730111240.png]]

## エリアとアイテムをつなげる

```css
.card-image {
	grid-area: image;
	border-top-left-radius: 15px;
	border-top-right-radius: 15px;
	background-color: orange;
}

.card-text {
	grid-area: text;
	background-color: 
}
```

アイテムの中にグリッドを作る。

```css
.card-stats {
	grid-area: stats;
	display: grid;
	grid-template-columns: 1fr 1fr 1fr;    横3つ
	grid-template-rows: 1fr;               縦一つ
	
	border-bottom-left-radius: 15px;
	border-bottom-right-radius: 15px;
	background: blueviolet;
}

グリッドの堺を真ん中のアイテムにつける

.card-stats .border {
	border-left: 1px solid white;
	border-right: 1px solid white;
}
```

![[Pasted image 20210730112403.png]]

アイテムの中に flex を設定したりして、要素を調整してる。


