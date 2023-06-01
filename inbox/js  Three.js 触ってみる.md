#js/three 

---
2021-09-28

# Three.js 触ってみる

https://ics.media/entry/14771/

```shell
$ npm init vite@latest three
$ cd three
$ npm run dev
```

### カメラ
![[Pasted image 20210928140228.png]]

カメラが移して見えるものをレンダラーが canvas にレンダリングする。


index.html

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link rel="icon" type="image/svg+xml" href="favicon.svg" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Vite App</title>
  </head>
  <body>
    <canvas id="myCanvas"></canvas>
    <script type="module" src="/main.js"></script>
  </body>
</html>
```

main.js
```js
import * as THREE from 'three'

function init() {
  const width = 960
  const height = 540

  // レンダラーを作成
  const renderer = new THREE.WebGLRenderer({
    canvas: document.querySelector('[[myCanvas]]'),
  })
  renderer.setPixelRatio(window.devicePixelRatio)
  renderer.setSize(width, height)

  // シーンを作成	
  const scene = new THREE.Scene()

  // カメラを作成
  //  (画角, アスペクト比, 描画開始距離, 描画終了距離)
  const camera = new THREE.PerspectiveCamera(45, width / height, 1, 10000)
  camera.position.set(0, 0, +1000)

  //　箱を作成
  const geometry = new THREE.BoxGeometry(500, 500, 500)
  const material = new THREE.MeshStandardMaterial({
    color: 0x0000ff,
  })
  const box = new THREE.Mesh(geometry, material)
  scene.add(box)

　// 平行線
  const light = new THREE.DirectionalLight(0xffffff)
  light.intensity = 2 // 光の強さを倍に
  light.position.set(1, 1, 1)
  // シーンに追加
  scene.add(light)

  // 初回実行
  tick()

  function tick() {
    requestAnimationFrame(tick)
	 
    // 箱を回転させる
    box.rotation.x += 0.01
    box.rotation.y += 0.01
	  
    // レンダリング
    renderer.render(scene, camera)
  }
}

init()
```

![[Pasted image 20210928135555.png]]

グリグリ動く。

こんなに多岐にわたる Three.js の世界。

![[Pasted image 20210928135652.png]]

