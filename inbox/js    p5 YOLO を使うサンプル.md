#js/p5 #yolo


<https://himco.jp/2019/01/21/8_2%EF%BC%9Ayolo%E3%81%A8p5-js%E3%81%AB%E3%82%88%E3%82%8B%E3%83%AA%E3%82%A2%E3%83%AB%E3%82%BF%E3%82%A4%E3%83%A0%E7%89%A9%E4%BD%93%E6%A4%9C%E5%87%BA/>

# setting

```shell
$ p5 generate -b p5-sample
$ cd p5-sample
```

ml5.min.js を Library にダウンロードしてみる。

```shell
$ cd libraries
$ wget https://unpkg.com/ml5@0.1.3/dist/ml5.min.js --no-check-certificate
```

どうなんだろう。どうせモデルデータダウンロードするし。

# code

index.html

```html
<!DOCTYPE html>
<html>
<head>
	<meta charset="UTF-8">
	<meta http-equiv="X-UA-Compatible" content="IE=edge">
	<meta name="viewport" content="width=device-width, initial-scale=1">

	<title>yolo_sample</title>

	<script src="libraries/p5.js"></script>
	<script src="libraries/p5.dom.js"></script>
	<script src="libraries/p5.sound.js"></script>
	<script src="https://unpkg.com/ml5@0.1.3/dist/ml5.min.js"></script>
	<script src="sketch.js"></script>

	<style>
		body {
			margin:0;
			padding:0;
			overflow: hidden;
		}
		canvas {
			margin:auto;
		}
	</style>

</head>
<body>
	<p id="status">reading models...</p>
</body>
</html>
```

sketch.js

```js
let video
let yolo
let status
let objects = []

function setup() {
  createCanvas(320, 240)
  video = createCapture(VIDEO)
  video.size(320, 240)

  yolo = ml5.YOLO(video, startDetecting)

  video.hide()
  status = select('#s')
}

function draw() {
  image(video, 0, 0, width, height)
  for (let i = 0; i < objects.length; i++) {
    console.log(JSON.stringify(objects[i]))
    noStroke()
    fill(0, 255, 0)

    text(
      objects[i].className,
      objects[i].x * width + 5,
      objects[i].y * height + 10
    )
    noFill()
    strokeWeight(4)
    stroke(0, 255, 0)
    rect(
      objects[i].x * width,
      objects[i].y * height,
      objects[i].w * width,
      objects[i].h * height
    )
  }
}

function startDetecting() {
  status.html('completed')
  detect()
}

function detect() {
  yolo.detect(function (err, results) {
    objects = results
    detect()
  })
}
```

# カスペルスキーのブロック

ここで、Webカメラのブロックを解除しないとカメラが見つけられない。
