#js/ml5



## p5 を使う

Processing の js 版。

```shell
$ yarn global add p5-manager
```

p5 プロジェクト作成。

```shell
$ p5 generate -b ml5_sample
```

\-b は bundled project

## ml5.js を使う

index.html

```html
<!DOCTYPE html>
<html>
<head>
	<meta charset="UTF-8">
	<meta http-equiv="X-UA-Compatible" content="IE=edge">
	<meta name="viewport" content="width=device-width, initial-scale=1">

	<title>ml5_pong</title>

	<script src="libraries/p5.js"></script>
	<script src="libraries/p5.dom.js"></script>
	<script src="libraries/p5.sound.js"></script>
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
	<script src="https://unpkg.com/ml5@0.1.3/dist/ml5.min.js"></script>
</head>
<body>
</body>
</html>
```

sketch.js

```js
let xBall = Math.floor(Math.random() * 300) + 50
let yBall = 50
const diameter = 50

let vxBall = 5
let vyBall = 5

let xPaddle
let yPaddle
const paddleWidth = 100
const paddleheight = 25

let video
let classifier
let action = 'neutral'

function setup() {
  createCanvas(640, 480)
  xPaddle = width / 2
  yPaddle = height - 100

  const featureExtractor = ml5.featureExtractor('MobileNet', modelLoaded)
  featureExtractor.numClasses = 5

  video = createCapture(VIDEO)
  video.hide()

  classifier = featureExtractor.classification(video, videoReady)

  neutralButton = createButton('neutral')
  neutralButton.mousePressed(function () {
    classifier.addImage('neutral')
    console.log('Added neutral image.')
  })

  leftButton = createButton('left')
  leftButton.mousePressed(function () {
    classifier.addImage('left')
    console.log('Added left image.')
  })

  rightButton = createButton('right')
  rightButton.mousePressed(function () {
    classifier.addImage('right')
    console.log('Added right image.')
  })

  upButton = createButton('up')
  upButton.mousePressed(function () {
    classifier.addImage('up')
    console.log('Added up image.')
  })

  downButton = createButton('down')
  downButton.mousePressed(function () {
    classifier.addImage('down')
    console.log('Added down image.')
  })

  trainButton = createButton('train')
  trainButton.mousePressed(function () {
    classifier.train((loss) => {
      if (loss == null) {
        console.log('Training is complete!!')
        classifier.classify(gotResults)
      } else {
        console.log(loss)
      }
    })
  })
}

function modelLoaded() {
  console.log('Model Loaded!')
}

function videoReady() {
  console.log('The video is ready!')
}

function gotResults(err, results) {
  if (err) {
    console.error(err)
  } else {
    action = results
    console.log(results)
    classifier.classify(gotResults)
  }
}

function draw() {
  push()
  translate(width, 0)
  scale(-1.0, 1.0)
  image(video, 0, 0, width, height)
  pop()

  fill(250, 0, 200)
  noStroke()
  ellipse(xBall, yBall, diameter, diameter)

  if (action === 'left') {
    xBall -= 5
  } else if (action === 'right') {
    xBall += 5
  } else if (action === 'up') {
    yBall -= 5
  } else if (action === 'down') {
    yBall += 5
  }

  if (xBall < diameter / 2) xBall = diameter / 2
  if (xBall > width - diameter) vBall = width - diameter
  if (yBall < diameter / 2) yBall = diameter / 2
  if (yBall > height - diameter) yBall = height - diameter
}

function keyPressed() {
  if (keyCode === LEFT_ARROW) {
    xPaddle -= 50
  } else if (keyCode === RIGHT_ARROW) {
    xPaddle += 50
  }
}

```
