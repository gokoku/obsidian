---
type: note
---

#chrome 

---
2023-03-24  09:11

# chrome  Google Cloud ダッシュボード

https://console.cloud.google.com/projectselector2/home/dashboard?utm_source=Docs_ProjectSelector&hl=ja&_gl=1*90kt3d*_ga*NDAxMDQ1NzE0LjE2Nzk2MTU1MzU.*_ga_NRWSTWS78N*MTY3OTYxNTUzNS4xLjEuMTY3OTYxNTg0MC4wLjAuMA..&pli=1

![[Pasted image 20230324091207.png]]

develop.peoplecorp@gmail.com アカウントでプロジェクト作った。

Google Maps Platform を有効にした。
```
AIzaSyBnWsedfz7i-IgHhTKNpTK_9xpA6IAXVoA
```


```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <script src="https://code.jquery.com/jquery-3.6.1.js" integrity="sha256-3zlB5s2uwoUzrXK3BT7AX3FyvojsraNFxCc2vC/7pNI=" crossorigin="anonymous"></script>
  <script async
      src="https://maps.googleapis.com/maps/api/js?key=AIzaSyBnWsedfz7i-IgHhTKNpTK_9xpA6IAXVoA&callback=initMap">
  </script>
  <title>Map</title>
  <style>
    #map {
      height: 100%;
    }
    html, body {
      height: 100%;
      margin: 0;
      padding: 0;
    }
  </style>
</head>
<body>
<div id="map"></div>
</body>
<script src="script.js"></script>
</html>
```

script.js
```js
let map

function initMap() {
  map = new google.maps.Map(document.getElementById("map"), {
    center: { lat: 39.613109, lng: 141.148595 },
    zoom: 15,
  })
}

window.initMap = initMap
```

![[Pasted image 20230324094717.png]]
