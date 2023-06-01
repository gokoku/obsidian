#js/p5



# raady

```shell
$ p5 generate -b randomwalker
```

sketch.js

```js
var walkers = []

function setup() {
  createCanvas(windowWidth, windowHeight)
  for (var i = 0; i < 50; i++) {
    walkers.push(new RandomWalker())
  }
}

function draw() {
  background(63)
  walkers.forEach((w) => {
    w.move()
    w.draw()
  })
}

class RandomWalker {
  x = 0
  y = 0
  newX = 0
  newY = 0

  constructor() {
    this.newX = this.x = random(width)
    this.newY = this.y = random(height)
  }

  move() {
    this.newX += random(-10, 10)
    this.newY += random(-10, 10)
    this.x = lerp(this.newX, this.x, 0.9)
    this.y = lerp(this.newY, this.y, 0.9)
  }

  draw() {
    fill(31, 127, 255)
    stroke(255)
    ellipse(this.x, this.y, 20, 20)
  }
}

```

lerp でフヨフヨ動くようになった。


