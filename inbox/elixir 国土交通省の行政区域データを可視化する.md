---
type: note
---

#elixir

---
2023-02-15  18:13

# elixir 国土交通省の行政区域データを可視化する

https://qiita.com/RyoWakabayashi/items/1a12439d2971444ba33b



<div class="rich-link-card-container"><a class="rich-link-card" href="https://qiita.com/RyoWakabayashi/items/1a12439d2971444ba33b" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://qiita-user-contents.imgix.net/https%3A%2F%2Fcdn.qiita.com%2Fassets%2Fpublic%2Farticle-ogp-background-9f5428127621718a910c8b63951390ad.png?ixlib=rb-4.0.0&w=1200&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTkxNiZ0eHQ9RWxpeGlyJTIwTGl2ZWJvb2slMjAlRTMlODElQTclRTUlOUIlQkQlRTUlOUMlOUYlRTQlQkElQTQlRTklODAlOUElRTclOUMlODElRTMlODElQUUlRTglQTElOEMlRTYlOTQlQkYlRTUlOEMlQkElRTUlOUYlOUYlRTMlODMlODclRTMlODMlQkMlRTMlODIlQkYlRTMlODIlOTIlRTUlOEYlQUYlRTglQTYlOTYlRTUlOEMlOTYlRTMlODElOTklRTMlODIlOEIlRUYlQkMlODglRTklODMlQkQlRTklODElOTMlRTUlQkElOUMlRTclOUMlOEMlRTMlODMlQkIlRTUlQjglODIlRTUlOEMlQkElRTclOTQlQkElRTYlOUQlOTElRTMlODElQUUlRTUlQkQlQTIlRTMlODIlOTIlRTUlOUMlQjAlRTUlOUIlQjMlRTQlQjglOEElRTMlODElQUIlRTMlODMlOTclRTMlODMlQUQlRTMlODMlODMlRTMlODMlODglRTMlODElOTklRTMlODIlOEIlRUYlQkMlODkmdHh0LWNvbG9yPSUyMzIxMjEyMSZ0eHQtZm9udD1IaXJhZ2lubyUyMFNhbnMlMjBXNiZ0eHQtc2l6ZT01NiZ0eHQtY2xpcD1lbGxpcHNpcyZ0eHQtYWxpZ249bGVmdCUyQ3RvcCZzPTY2OGNkZDMxOWFiMGE1MTIxZGZhNDY4YWVhZTEyNzY1&mark-x=142&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTYxNiZ0eHQ9JTQwUnlvV2FrYWJheWFzaGkmdHh0LWNvbG9yPSUyMzIxMjEyMSZ0eHQtZm9udD1IaXJhZ2lubyUyMFNhbnMlMjBXNiZ0eHQtc2l6ZT0zNiZ0eHQtYWxpZ249bGVmdCUyQ3RvcCZzPTBhZTk3NDk3MTBjODE5ZTMxODMyMThhMzc2NzNmNGM2&blend-x=142&blend-y=491&blend-mode=normal&s=66451456d5b8ce2e62946c4fcd6940bf')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Elixir Livebook で国土交通省の行政区域データを可視化する（都道府県・市区町村の形を地図上にプロットする） - Qiita</h1>
		<p class="rich-link-card-description">
		はじめに みなさん、都道府県・市区町村の位置と形、分かりますか？ 我が家では日本地図パズルで娘とよく勉強したものです  都道府県・市区町村の形が分かると何が良いのか、、、 都道府県・市区町村の地理情報=座標=緯度経度があれば、ある地...
		</p>
		<p class="rich-link-href">
		https://qiita.com/RyoWakabayashi/items/1a12439d2971444ba33b
		</p>
	</div>
</a></div>


setup
```elixir
Mix.install(
[
  {:nx, "~> 0.4"},
  {:evision, "~> 0.1"},
  {:exla, "~> 0.4"},
  {:geo, "~> 3.4"},
  {:jason, "~> 1.4"},
  {:kino, "~> 0.8"},
  {:kino_maplibre, "~> 0.1.3"}
],
config: [
  nx: [
    default_backend: EXLA.Backend
  ]
]
)
```

国土交通省のGIS を使うらしい。

https://nlftp.mlit.go.jp/index.html

--> https://nlftp.mlit.go.jp/ksj/gml/datalist/KsjTmplt-N03-v3_1.html

![[Pasted image 20230215181717.png]]

livebook にダウンロードデータを設置して

```elixir
Path.absnome("./gtfs-sample/N03-03_GML")
|> File.ls!
```

"N03-22_03_220101.geojson" が Geojson データというものらしい。
https://geojson.org/

```elixir
geojson_data =
	Path.absname("./gtfs-sample/N03-03_GML/N03-22_03_220101.geojson")
	|> File.read!
	|> Jason.decode!
	|> Geo.JSON.decode!
```
![[Pasted image 20230216133313.png]]

```
%Geo.GeometryCollection {
	georetries: [
		%Geo.MultiPolygon {
			coordinates: [
				[
					[{緯度,経度}...],
					[{緯度, 経度}...],
					[{経度, 経度}...],
				]
			],
			srid: nil,
			properties: %{
				"N03_001" => "岩手県",
				"N03_002" => nil,
				"N03_003" => nil,
				"N03_004" => nil,
				"N03_007" => nil
			}
		},
		%Geo.Polygon {
			coordinates: [
				[
					{lat, lon},{lat,lon},....
				]
			],
			strid: nil,
			properties: %{
				"N03_001" => "岩手県",
				"N03_002" => nil,
				"N03_003" => nil,
				"N03_004" => "盛岡市",
				"N03_007" => "03201"
		   }
		},
		%Geo.Polygon {
			cordinates: [[{}.....]],
			srig: nil,
			properties: %{
				"N03_001" => "岩手県",
				"N03_002" => nil,
				"N03_003" => nil,
				"N03_004" => "宮古市",
				"N03_007" => "03202"
			}
		},
		....
	],
	srid: nil,
	properties: %{}
}
```
```
%Geo.GeometryCollection{
	georetries: [
		%Geo.MultiPolygon {
			coordinates: [[[{},{}...],[{},{}...]....]],
			srid: nil,
			properties: %{}
		},
		%Geo.Polygon{
			coordinates: [[{},{}...]],
			srid: nil,
			properties: %{}
		},
		%Geo.Polygon....
	],
	srid: nil,
	properties: %{}
}
```


セルの指定でMapを選んだ。
![[Pasted image 20230216135014.png]]

CENTER に geojson_data の緯度経度を入れて、Source を geojson_data にすると
![[Pasted image 20230216135210.png]]

ZOOM 6 にして、Type をline にすると、
![[Pasted image 20230216135455.png]]

出た!!

cell のコードだけでやってみる。

Qiita の記事の通りにやる。素直にね。
```elixir
prefecture_geojson =
  %Geo.GeometryCollection{
    geometries: [Enum.at(geojson_data.geometries, 0)]
  }
```

Center の値を出す。
```elixir
longitudes =
  prefecture_geojson.geometries
    |> Enum.map(& &1.coordinates)
    |> List.flatten()
    |> Enum.map(& elem(&1, 0))

latitudes =
  prefecture_geojson.geometries
    |> Enum.map(& &1.coordinates)
    |> List.flatten()
    |> Enum.map(& elem(&1, 1))

center =
  {
    (Enum.min(longitudes) + Enum.max(longitudes)) / 2,
    (Enum.min(latitudes) + Enum.max(latitudes)) / 2
  }
```

描画する
```elixir
MapLibre.new(center: center, zoom: 6.8)
  |> MapLibre.add_geo_source("prefecture_geojson", prefecture_geojson)
  |> MapLibre.add_layer(
    id: "prefecture",
    source: "prefecture_geojson",
    type: :fill,
    paint: [fill_color: "#d000ff"]
  )
```

## 市区町村の可視化

https://nlftp.mlit.go.jp/ksj/gml/codelist/AdminAreaCd.html

ここに行政コードの表がある。
![[Pasted image 20230216160227.png]]
おい! 村じゃねーかよ。しかも無い。
![[Pasted image 20230216160704.png]]

```elixir
city_geojson =
  %Geo.GeometryCollection{
    geometries: [Enum.filter(geojson_data.geometries, & &1.properties["N03_007"] == "03201")]
  }
```

中心。
```elixir
get_center = fn geometries ->
  coordinates =
    geometries
    |> Enum.map(& &1.coordinates)
    |> List.flatten()

  lon = Enum.map(coordinates, & elem(&1, 0))
  lat = Enum.map(coordinates, & elem(&1, 1))
  
  {
    (Enum.min(lon) + Enum.max(lon)) / 2,
    (Enum.min(lat) + Enum.max(lat)) / 2
  }
end
center = get_center.(city_geojson.geometries)
```

描画。
```elixir
MapLibre.new(center: center, zoom: 7)
  |> MapLibre.add_geo_source("city_geojson", city_geojson)
  |> MapLibre.add_layer(
    id: "city",
    source: "city_geojson",
    type: :line,
    paint: [line_color: "#ff00aa"]
  )
```

![[Pasted image 20230216161819.png]]

## Evision で画像化

緯度経度の配列をそれぞれ作る。
正規化するのに緯度経度の最小最大を求めるらしい。
```elixir
coordinates =
  city_geojson.geometries
  |> Enum.map(& &1.coordinates)
  |> List.flatten()

lon = Enum.map(coordinates, & elem(&1, 0))
lat = Enum.map(coordinates, & elem(&1, 1))

min_lon = Enum.min(lon)
max_lon = Enum.max(lon)
min_lat = Enum.min(lat)
max_lat = Enum.max(lat)

# 画像の大きさ
{height, width} = {1280, 1280}
```

```elixir
normalized_points =
  city_geojson.geometries
  |> Enum.map(& &1coordinates)
  |> Enum.map(fn coordinate ->
    cordinate
    |> 
  end)
```


