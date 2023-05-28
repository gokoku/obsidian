---
type: note
---

#css

---
2023-05-18  14:54

# css Grid 入門

index.html
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
    <div class="item">One</div>
    <div class="item">Two</div>
    <div class="item">Three</div>
    <div class="item">Four</div>
    <div class="item">Five</div>
  </div>
 </body>
</html>
```


style.css
```css
.wrapper {
	display: grid;
	grid-template-columns: repeat(3,1fr);
	grid-auto-rows: 100px;
	column-gap: 10px;
	row-gap: 10px;
	justify-items: center;
	align-items: center;
}
.item {
	background-color: aquamarine;
}
.item:nth-child(1) {
	grid-column: 1;
	grid-row: 1 / 3
}
.item:nth-child(2) {
	grid-column: 2 / 4;
	grid-row: 1;
}
.item:nth-child(5) {
	grid-column: 2;
}
```

![[Pasted image 20230518150728.png]]

