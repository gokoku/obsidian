---
type: note
---

#js

---
2023-03-22  10:28

# js mapbox で人工衛星の軌道を投影する

https://qiita.com/yonda_gliese/items/7fe9a9beaf78fd7f1282

```shell
$ npm init vite@latest sat
$ cd sat
$ npm install sgp4
$ npm install tle-validator

$ npm run dev
```

index.html

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link rel="icon" type="image/svg+xml" href="/vite.svg" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Vite App</title>
    <script src='https://api.mapbox.com/mapbox-gl-js/v1.9.1/mapbox-gl.js'></script>
    <link href='https://api.mapbox.com/mapbox-gl-js/v1.9.1/mapbox-gl.css' rel='stylesheet' />
  </head>
  <body>
    <div id="map"></div>
    <div style="margin-top: 20px;">
      <textarea id="tle"></textarea>
    </div>
    <div>
      <button id="add">追加</button>
      <p id="error"></p>
    </div>
    <script type="module" src="/main.js"></script>
  </body>
</html>

```

main.js
```js
import './style.css'
import SGP4 from 'sgp4'
import TLEValidator from 'tle-validator'

const simulateUpdateSecond = 60
const animationDurationMilliSecond = 50
const maxSecond = 3600 * 24

const map = new mapboxgl.Map({
  container: "map",
  center: [139.765, 35.65],
  zoom: 1,
  minZoom: 1,
  maxZoom: 8
})


map.addLayer({
  "id": "base/pale",
  "type": "raster",
  "source": {
    type: "raster",
    tiles: ["https://cyberjapandata.gsi.go.jp/xyz/pale/{z}/{x}/{y}.png"],
    attribution: "<a href='http: //maps.gsi.go.jp/development/ichiran.html'>地理院タイル</a>",
    tileSize: 256,
    minzoom: 2,
    maxzoom: 8
  }
})

map.on('load', function() {
  document.getElementById("add").addEventListener('click', function() {
    document.getElementById("error").innerText = ""
    let texts = document.getElementById("tle").value.split(/\n/)

    function setAnimation(line1, line2) {
      const satRec = SGP4.twoline2rv(line1, line2, SGP4.wgs84())
      const now = new Date()
      const id = now.getTime()
      const datetime = now
      const counterclockwise = getAngularVelocity(datetime).z > 0
      const meanMotion = satRec.no //rad/min
      const orbitPeriod = 2 * Math.PI / (meanMotion / 60) //sec
      const limitSecond = Math.min(maxSecond, orbitPeriod)

      function getPosition(datetime) {
        //https://github.com/joshuaferrara/node-sgp4/blob/master/example.js
        const positionAndVelocity = SGP4.propogate(satRec, datetime.getUTCFullYear(), datetime.getMonth() + 1, datetime.getUTCDate(), datetime.getUTCHours(), datetime.getUTCMinutes(), datetime.getUTCSeconds())
        const gmst = SGP4.gstimeFromDate(datetime.getUTCFullYear(), datetime.getUTCMonth() + 1, datetime.getUTCDate(), datetime.getUTCHours(), datetime.getUTCMinutes(), datetime.getUTCSeconds())
        const geodeticCoodinates = SGP4.eciToGeodetic(positionAndVelocity.position, gmst)
        const longitude = SGP4.degreesLong(geodeticCoodinates.longitude)
        const latitude = SGP4.degreesLat(geodeticCoodinates.latitude)
        console.log("getPosition")
        return [longitude, latitude]
      }

      function getAngularVelocity(datetime) {
        const positionAndVelocity = SGP4.propogate(satRec, datetime.getUTCFullYear(), datetime.getUTCMonth() + 1, datetime.getUTCDate(), datetime.getUTCHours(), datetime.getUTCMinutes(), datetime.getUTCSeconds())
        const position = positionAndVelocity.position
        const velocity = positionAndVelocity.velocity
        const posLength = Math.sqrt(position.x ** 2 + position.y ** 2 + position.z ** 2)
        const velLength = Math.sqrt(velocity.x ** 2 + velocity.y ** 2 + velocity.z ** 2)
        console.log("getAngularVelocity")

        return {
          x: (position.y * velocity.z - position.z * velocity.y) / posLength / velLength,
          y: (position.z * velocity.x - position.x * velocity.z) / posLength / velLength,
          z: (position.x * velocity.y - position.y * velocity.x) / posLength / velLength
        }
      }

      map.addLayer({
        'id': 'line_' + id,
        'type': 'line',
        'source': {
          'type': 'geojson',
          'data': {
            'type': 'FeatureCollection',
            'features': [{
              'type': 'Feature',
              'geometry': {
                'type': 'LineString',
                'coordinates': [
                  [0, 0]
                ]
              }
            }]
          }
        },
        'layout': {
          'line-cap': 'round',
          'line-join': 'round'
        },
        'paint': {
          'line-color': '#ed6498',
          'line-width': 2,
          'line-opacity': 0.8
        }
      })
      map.addLayer({
        'id': 'point_' + id,
        'source': {
          'type': 'geojson',
          'data': {
            'type': 'Point',
            'coordinates': [0, 0]
          }
        },
        'type': 'circle',
        'paint': {
          'circle-radius': 10,
          'circle-color': '#007cbf'
        }
      })

      function animate(timestamp) {
        return function() {
          let datetime = new Date(timestamp)
          let crossingAntiMeridianNumber = 0
          let elapsedSec = 0

          let lines = [
            []
          ]
          let before = getPosition(datetime)
          while(elapsedSec < limitSecond) {
            let lonlat = getPosition(datetime)
            if(counterclockwise) {
              if(lonlat[0] < 0 && before[0] > 0) {
                const lat = (lonlat[1] - before[1]) / (360 + lonlat[0] - before[0]) * (180 - before[0]) + before[1];
                lines[crossingAntiMeridianNumber].push([180, lat])
                lines[++crossingAntiMeridianNumber] = [
                  [-180, lat]
                ]
              }
            } else {
              if (lonlat[0] > 0 && before[0] < 0) {
                const lat = (lonlat[1] - before[1]) / (-360 + lonlat[0] - before[0]) * (-180 - before[0]) + before[1]
                lines[crossingAntiMeridianNumber].push([-180, lat])
                lines[++crossingAntiMeridianNumber] = [
                  [180, lat]
                ]
              }
            }
            lines[crossingAntiMeridianNumber].push(lonlat)
            before = lonlat
            if(elapsedSec < limitSecond / 2)
              setTimeout(function() {
                map.getSource('point_' + id).setData({
                  'type': 'Point',
                  'coordinates': lonlat
                })
              }, 50 * elapsedSec)
            datetime.setSeconds(datetime.getSeconds() + simulateUpdateSecond)
            elapsedSec += simulateUpdateSecond
          }

          function createFeature(line) {
            return {
              'type': 'Feature',
              'geometry': {
                'type': 'LineString',
                'coordinates': line
              }
            }
          }
          let geojson = {
            'type': 'FeatureCollection',
            'features': lines.map(function (line){
              return createFeature(line)
            })
          }
          map.getSource('line_' + id).setData(geojson)
          setTimeout(animate(datetime.getTime()), animationDurationMilliSecond * elapsedSec / 2)
        }
      }
      animate(now.getTime())()
    }
    if(texts.length > 1 && TLEValidator.validateTLE(texts[0].trim(), texts[1].trim())) {
      setAnimation(texts[0].trim(), texts[1].trim())
      document.getElementById("tle").value = ""
    } else {
      document.getElementById("error").innerText = "フォーマットが正しくありません。"
    }
  })
})

```

style.css
```css
#map {
	height: 480px;
}

textarea {
	width: 450px;
	height: 3em;
	line-height: 1.5;
	font-size: 12px;
}

#error {
	font-size: 10px;
}
```


![[Pasted image 20230322135145.png]]

リアルタイムで軌道上をポイントが移動してるらしい。

textarea の中に入れるものは、

https://celestrak.org/NORAD/elements/gp.php?GROUP=beidou&FORMAT=tle

```
1 36828U 10036A   23080.55490306 -.00000153  00000+0  00000+0 0  9990
2 36828  54.2094 173.6975 0035427 188.3547 125.0435  1.00271175 46350
```

これだけ。
ガリレオはこれかな?

![[Pasted image 20230322135346.png]]
https://celestrak.org/NORAD/elements/

Space Stations の中にある ISS とか速い

![[Pasted image 20230322135947.png]]

すげー。