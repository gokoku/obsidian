#js/ml5 #js/p5



## p5 準備

```shell
$ p5 generate -b glasses
```


sketch.js

```js
let video
let poseNet
let faceImage
let noseX = 0,
  noseY = 0
let eyelX = 0,
  eyelY = 0
let eyerX = 0,
  eyerY = 0

function preload() {
  faceImage = loadImage('images/face.png')
}

function setup() {
  createCanvas(640, 480)
  video = createCapture(VIDEO)
  video.hide()
  let poseNet = ml5.poseNet(video, modelLoaded)
  poseNet.on('pose', gotPoses)
}

function draw() {
  image(video, 0, 0)

  let w = dist(eyerX, eyerY, eyelX, eyerY)

  image(faceImage, eyerX - w, eyerY - (w * 3) / 2, w * 3, w * 4)

  fill(255, 0, 0)
  ellipse(noseX, noseY, w * 0.75)

  fill(255, 255, 255)
  ellipse(eyerX, eyerY, w * 0.5)
  ellipse(eyelX, eyelY, w * 0.5)

  fill(0, 0, 0)
  ellipse(eyerX, eyerY, w * 0.3)
  ellipse(eyelX, eyelY, w * 0.3)
}

function modelLoaded() {
  console.log('Model Loaded!')
}

function gotPoses(poses) {
  if (poses.length > 0) {
    //console.log(JSON.stringify(poses))

    let newNose = poses[0].pose.keypoints[0].position
    let newEyel = poses[0].pose.keypoints[1].position
    let newEyer = poses[0].pose.keypoints[2].position

    noseX = lerp(noseX, newNose.x, 0.2)
    noseY = lerp(noseY, newNose.y, 0.2)

    eyelX = lerp(eyelX, newEyel.x, 0.2)
    eyelY = lerp(eyelY, newEyel.y, 0.2)

    eyerX = lerp(eyerX, newEyer.x, 0.2)
    eyerY = lerp(eyerY, newEyer.y, 0.2)
  }
}

```

images/face.png


