---
type: note
---

#js

---
2023-03-24  10:00

# Rapid API を使ってGeoTechnologiesを使って地図


<div class="rich-link-card-container"><a class="rich-link-card" href="https://rapidapi.com/hub" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://rapidapi.com/static-assets/default/logo.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">API Hub - Free Public & Open Rest APIs | Rapid</h1>
		<p class="rich-link-card-description">
		Browse, Test & Connect to 1000s of Public Rest APIs on RapidAPI's API Hub - the world's largest API directory. Sign up today for Free!
		</p>
		<p class="rich-link-href">
		https://rapidapi.com/hub
		</p>
	</div>
</a></div>

MapFanAPI-Map を選んだ。ここからBASIC プランに申し込む。

Test Subscribe をクリックして、申込のボタンをクリックすると、Test Endpoint に変わっていて、実行すれば動く

![[Pasted image 20230324100322.png]]

地図画像取得        と    サイズ指定地図画像取得でAPI が違う
10,000リクエスト/月    1,500リクエスト/月

地図画像取得では lat lon で特定できないっぽい??
取得しても表示が難しかった。
response から png にするやり方がわからなかった。

これはかなり悔しい!







## zoom, x, y --> lon lat の変換が出来るようだ


<div class="rich-link-card-container"><a class="rich-link-card" href="https://wiki.openstreetmap.org/wiki/Slippy_map_tilenames#Python" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://wiki.openstreetmap.org/favicon.ico')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Slippy map tilenames - OpenStreetMap Wiki</h1>
		<p class="rich-link-card-description">
		
		</p>
		<p class="rich-link-href">
		https://wiki.openstreetmap.org/wiki/Slippy_map_tilenames#Python
		</p>
	</div>
</a></div>


```python
import math

def num2deg(xtile, ytile, zoom):
	n = 2.0 ** zoom
	lon_deg = xtile / n * 360.0 - 180.0
	lat_rad = math.atan(math.sinh(math.pi * (1 - 2 * ytile / n)))
	lat_deg = math.degrees(lat_rad)
	return (lat_deg, lon_deg)

num2deg(58211,25806, 16)

(35.68407153314097, 139.7625732421875)
```

```python
import math

def deg2num(lat_deg, lon_deg, zoom):
	lat_rad = math.radians(lat_deg)
	n = 2.0 ** zoom
	xtile = int((lon_deg + 180.0) / 360.0 * n)
	ytile = int((1.0 - math.asinh(math.tan(lat_rad)) / math.pi) / 2.0 * n)
	return (xtile, ytile)

deg2num(39.6131092333334,141.148595233335,16)

(58463, 24902)
```