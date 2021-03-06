#css

---
2021-08-04

# CSSの3Dエフェクトってどんなの？

https://logical-studio.com/develop/web/20201204-20201202-3deffect/

これだけでも、凄いと思ってしまったので入れた。

## 2Dアニメーションボタン

index.html

```html
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=">
  <title></title>
  <link href="style.css" rel="stylesheet">
</head>
<body>
    <div class="container">
        <div class="btn slide-up">
            しゅこ〜
        </div>
    </div>
</body>
</html>
```

style.css

```css
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
}

.btn {
  background-color: [[f6c0d9]];
  color: #f;
  padding: 15px 30px;
  margin: 30px;
  font-size: 16px;
  font-weight: 600;
  text-transform: uppercase;
  cursor: pointer;
  transition: all 0.7s;
  z-index: 1;
}

.btn.slide-up {
  position: relative;
}

.btn.slide-up::before {
  content: "";
  display: inline-block;
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  background-color: [[b7e1f8]];
  transform: rotateX(-90deg);
  transition: transform 0.7s;
  transform-origin: bottom center;
  z-index: -1;
}

.btn.slide-up:hover {
  color: #999;
}

.btn.slide-up:hover::before {
  transform: none;
}
```

最初からXの横軸で90度回転している状態で、立ってて見えないのが、ホバーで解除される感じ。

![[Pasted image 20210804144006.png]]


## 3D エフェクトボタン

### code

index.html

```html
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=">
  <title></title>
  <link href="style.css" rel="stylesheet">
</head>
<body>
    <div class="container">
        <div class="btn effect-3d">
            <span>しゅこ〜</span>
        </div>
    </div>
</body>
</html>
```

style.css

```css
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
}

.btn {
  background-color: [[f6c0d9]];
  color: #f;
  padding: 18px 30px;
  margin: 30px;
  font-size: 16px;
  font-weight: 600;
  text-transform: uppercase;
  cursor: pointer;
  transition: all 0.7s;
  z-index: 1;
}

.btn.effect-3d {
  position: relative;
  z-index: 1;
  transform-style: preserve-3d;
  perspective: 800px;
}

.btn.btn.effect-3d span {
  display: inline-block;
  transform: translateZ(20px);
}
.btn.effect-3d::before {
  content: "";
  display: inline-block;
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  background-color: [[f6c0d9]];
  transform: none;
  transition: transform 0.7s;
  transform-origin: left center;
  opacity: 1;
}
.btn.btn.effect-3d:hover {
  color: #999;
  opacity: 1;
  background-color: [[b7e1f8]];
  perspective: 250px;
}

.btn.btn.effect-3d:hover::before {
  background-color: [[f6c0d9]];
  transform: rotateY(-90deg);
  transition: all 0.7s;
  opacity: 1;
}
```

![[Pasted image 20210804152025.png]]

素晴らしい!! 奥行きがあるアニメーションになってる。

### CSS 3Dの仕組み

#### transform-style: perserve-3d

設定した要素の「子要素を 3D 化する」設定。

#### perspective: 数字

奥行き、カメラの視点。

つまり手前から奥の軸で、値が小さいと近くて、大きいと遠ざかる。

```css
.btn.effect-3d {
	position: relative;
	transform-style: perserve-3d;
	perspective: 800px  /* 奥行きの値 */
}
```

#### 座標軸

座標軸は、z : 手前から奥     x :  左から右     y : 上から下

![](https://develop.logical-studio.com/wp-content/uploads/2020/12/howto3d.jpg)

#### perspective-origin

視点の位置を変更出来るプロパティ。

perspective-origin: 視点の位置( x軸 y軸)  default 50% 50%

値は、%  left  right  top bottom  center とある。

```css
.btn.effect-3d {
  position: relative;
  z-index: 1;
  transform-style: preserve-3d;
  perspective: 800px;
  perspective-origin: -50% 50%;
}
```

半開きみたいなところで止まる。x軸 -50% とは左側面からの視点とのこと。

軸 -50% だと上からの視点になる。

```css
.btn.effect-3d {
  position: relative;
  z-index: 1;
  transform-style: preserve-3d;
  perspective: 800px;
  perspective-origin: 50% -50%;
}
```

![[Pasted image 20210804153658.png]]



## 裏表ひっくり返る

なんて高度なのだ!!

index.html

```html
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="style.css">
    <title></title>
</head>
<body>
    <section>
        <div class="card">
            <div class="box">
                <div class="imgBx">
                    <img src="https://develop.logical-studio.com/wp-content/uploads/2020/12/img01.jpg">
                </div>
                <div class="contentBx"></div>
            </div>
        </div>
    </section>
</body>
</html>
```

style.css

```css
body {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background-color: #333;
}

section {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-wrap: wrap;
  transform-style: preserve-3d;
  width: 1100px;
}

section .card {
  position: relative;
  width: 160px;
  height: 160px;
  margin: 20px;
  transform-style: preserve-3d;
  perspective: 1000px;
}

section .card .box {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  transform-style: preserve-3d;
  transition: 1s ease;
}

section .card:hover .box {
  transform: rotateY(180deg);
}
section .card .box .imgBx {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}

section .card .box .imgBx img {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  backface-visibility: hidden;
}

section .card .box .contentBx {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-image: url(https://develop.logical-studio.com/wp-content/uploads/2020/12/img02.jpg);
  background-size: cover;
  backface-visibility: hidden;
  display: flex;
  justify-content: center;
  align-items: center;
  transform-style: preserve-3d;
  transform: rotateY(180deg);
}

```

![[Pasted image 20210804155409.png]]

くるりと回って

![[Pasted image 20210804155425.png]]

なんて奥深いのだろう。
