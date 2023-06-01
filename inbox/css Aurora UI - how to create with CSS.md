#css

---
2021-06-16


記事からのキャッチアップ

https://www.albertwalicki.com/aurora-ui-how-to-create 


# Blurred Shapes
The first method is to create three ovals overlaying on each other. Create big ones and position them:

-   first one on the top of the container
-   two in the bottom corners

Then we need to add `filter: blur()` and lower the opacity slightly.


```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div class="wrapper">
    <div class="base one"></div>
    <div class="base two"></div>
    <div class="base three"></div>
  </div>
</body>
</html>
```

```css
.wrapper {
  width: 400px;
  height: 400px;
  background: #f;
  position: relative;
  overflow:hidden;
  border-radius: 40px;
}

.base {
  position: absolute;
  filter: blur(60px);
  opacity: 0.8;
}

.one {
  border-radius: 100%;
  width: 600px;
  height: 600px;
  background-color: #373372;
  left:-50px;
  top:-300px;
  z-index: 3;
}

.two {
  width: 500px;
  height: 800px;
  background-color: [[7C336C]];
  bottom:-30px;
  left:-80px;
}

.three {
  border-radius: 100%;
  width: 450px;
  height: 450px;
  bottom:-80px;
  right:-100px;
  background-color: [[B3588A]];
}
```

![[Pasted image 20210616103436.png]]



# Radial gradient

The second method is to use gradient colours! Instead of solid colours, we can use a radial-gradient to create our effect.

Let's add three radial-gradients from a solid colour to transparent:

-   top left
-   top right
-   bottom left

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div class="wrapper"></div>
</body>
</html>
```

```css
.wrapper {
  width: 400px;
  height: 400px;
  position: relative;
  overflow:hidden;
  border-radius: 40px;
  background-color: #f;
  background-image: 
    radial-gradient(at top left, [[F0ACE0]], transparent),
    radial-gradient(at top right, [[FFA4B2]], transparent),
    radial-gradient(at bottom left, [[A7D3F2]], transparent);
  background-size: 100% 100%;
  background-repeat: no-repeat;
}
```

![[Pasted image 20210616112827.png]]


# Blur an image

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div class="wrapper">
    <img src="ourImg.jpg"/>
  </div>
</body>
</html>
```

```css
.wrapper {

height: 400px;

position: relative;

overflow:hidden;

border-radius: 40px;

}

img {

filter: blur(60px);

}
```
![[Pasted image 20210616113615.png]]

![[Pasted image 20210616113626.png]]


# Animated background

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div class="wrapper">
    <div class="base one"></div>
    <div class="base two"></div>
    <div class="base three"></div>
  </div>
</body>
</html>
```


```css
@keyframes fly {
  100% {
    transform:rotate(1turn) translate(100px) rotate(-1turn);
  }
}

@keyframes flyPlus {
  100% {
    transform:rotate(-1turn) translate(100px) rotate(1turn);
  }
}

.wrapper {
  width: 400px;
  height: 400px;
  background: #f;
  position: relative;
  overflow:hidden;
  border-radius: 40px;
}

.base {
  position: absolute;
  filter: blur(60px);
  opacity: 0.8;
}

.one {
  border-radius: 100%;
  width: 600px;
  height: 600px;
  background-color: #373372;
  left:-50px;
  top:-300px;
  z-index: 3;
  animation: fly 12s linear infinite;
  transform:rotate(0) translate(80px) rotate(0);
}

.two {
  width: 500px;
  height: 800px;
  background-color: [[7C336C]];
  bottom:-30px;
  left:-80px;
}

.three {
  border-radius: 100%;
  width: 450px;
  height: 450px;
  bottom:-80px;
  right:-100px;
  background-color: [[B3588A]];
  apnimation: flyPlus 8s linear infinite;
  transform:rotate(0) translate(100px) rotate(0);
}
```

動く。

![[Pasted image 20210616113149.png]]