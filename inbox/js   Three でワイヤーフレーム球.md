#js/three 



とりあえずグローバルなコマンドとして用意した。

    $ yarn global add serve
    $ yarn global add browserify

    $ mkdir sphere
    $ cd sphere
    $ yarn init -y
    $ yarn add three

## package.json

```json :package.json
"script": {
  "run": "serve .",
  "build": "browserify index.js > bundle.js"
}
```

## index.html

```html :index.html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Sphere</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <canvas id="canvas"></canvas>
</body>
<script src="bundle.js"></script>
</html>
```

## style.css

```css
body {
  background: #333;
}
```

## index.js

```js :index.js
const THREE = require('three')

window.addEventListener('load', init)

const width = window.innerWidth
const height = window.innerHeight

function init() {
  const renderer = new THREE.WebGLRenderer({
    canvas: document.getElementById('canvas'),
  })

  renderer.setSize(width, height)
  renderer.setClearColor(new THREE.Color('rgb(51, 51, 51)'))

  const scene = new THREE.Scene()

  const camera = new THREE.PerspectiveCamera(40, width / height, 0.1, 10000)
  camera.position.set(0, 0, +1000)

  const sphere = new THREE.SphereGeometry(300, 30, 30)
  const material = new THREE.MeshStandardMaterial({
    color: new THREE.Color('rgb(0, 159, 140)'),
    wireframe: true,
  })

  const mesh = new THREE.Mesh(sphere, material)
  scene.add(mesh)

  const ambientlight = new THREE.AmbientLight(0xffffff, 1)
  scene.add(ambientlight)

  const directionLight = new THREE.DirectionalLight(0xffffff, 1)
  scene.add(directionLight)

  function render() {
    mesh.rotation.x += 0.01
    mesh.rotation.y += 0.01
    renderer.render(scene, camera)
    requestAnimationFrame(render)
  }
  render()
}
```

## build

    $ yarn build

## run

    $ yarn run

## Access to localhsot:5000
