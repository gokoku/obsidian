---
type: note
---

#css

---
2022-06-16  09:19

# css. ニューモーフィズム


<div class="rich-link-card-container"><a class="rich-link-card" href="https://twitter.com/mndgn_y/status/1537027495458897921" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">ユイ🌷ママWebデザイナー/クリエイター on Twitter</h1>
		<p class="rich-link-card-description">
		思わず撫でたくなる質感をCSSだけで再現🌷#ニューモーフィズム pic.twitter.com/rKEuVuhMGQ— ユイ🌷ママWebデザイナー/クリエイター (@mndgn_y) June 15, 2022
		</p>
		<p class="rich-link-href">
		https://twitter.com/mndgn_y/status/1537027495458897921
		</p>
	</div>
</a></div>


![[Pasted image 20220616092022.png]]

素晴らしい。

![[Pasted image 20220616092102.png]]

背景と同じ色
#ece5ete なのか

![[Pasted image 20220616092159.png]]

## やってみた

```shell
$ npm init vite@latest
> new-mofizum
$ cd new-mofizum
$ npm i
```

index.html
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
  </head>
  <body>
    <div class="background">
      <div class="box">🦋</div>
      <div class="box">🦋</div>
      <div class="box">🦋</div>
    </div>
    <script type="module" src="/src/main.ts"></script>
  </body>
</html>
```


main.ts
```ts
import './style.css'
```

style.css
```css
body {
  margin: 0;
  padding: 0;
}
.background {
  display: flex;
  justify-content: space-around;
  align-items: center;

  width: 100vw;
  height: 100vh;
  background-color: #ebe5e5;
}
.box {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 200px;
  height: 200px;
  font-size: 5em;
  border-radius: 20px;
  background: linear-gradient(145deg, #fbf5f5, #d4cece);
  box-shadow:  12px 12px 24px #cac5c5,
              -12px -12px 24px #ffffff;
}
```

![[Pasted image 20220616101742.png | 500]]

https://neumorphism.io/#e0e0e0

このツールを使うのがベストだ。
バックの色合いから影の色とか頭でやっても上手くいかない。
計算で正確に出したほうが断然いい。

