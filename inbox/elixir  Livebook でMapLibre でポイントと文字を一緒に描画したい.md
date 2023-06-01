---
type: note
---

#elixir

---
2023-02-27  18:16

# elixir  Livebook でMapLibre でポイントと文字を一緒に描画したい

過去1週間で起こった地震のデータを見る記事。




これが Livebook の MapLibre で描く。

```elixir
Mix.install([
  {:explorer, "~> 0.5"},
  {:geo, "~> 3.4"},
  {:kino, "~> 0.8"},
  {:kino_maplibre, "~> 0.1.3"},
  {:download, "~> 0.0.4"}
])
```

```elixir
geo_json =
  "https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_week.geojson"
  |> HTTPoison.get!()
  |> Map.get(:body)
  |> Jason.decode!()
  |> Geo.JSON.decode!()
```

## データの表示

参考になるような記事を探す。

<div class="rich-link-card-container"><a class="rich-link-card" href="https://dockyard.com/blog/2022/11/22/parsing-and-visualizing-sea-current-data-with-elixir-and-nx" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://assets.dockyard.com/images/ocean-currents.jpg')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Parsing and visualizing sea current data with Elixir and Nx - DockYard</h1>
		<p class="rich-link-card-description">
		Exploring NetCDF sea current data and plotting it into an interactive map
		</p>
		<p class="rich-link-href">
		https://dockyard.com/blog/2022/11/22/parsing-and-visualizing-sea-current-data-with-elixir-and-nx
		</p>
	</div>
</a></div>

難しすぎる。

<div class="rich-link-card-container"><a class="rich-link-card" href="https://maplibre.org/maplibre-gl-js-docs/example/change-case-of-labels/" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://static-assets.mapbox.com/branding/social/social-1200x630.v1.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Change the case of labels | MapLibre GL JS Docs</h1>
		<p class="rich-link-card-description">
		Use the upcase and downcase expressions to change the case of labels.
		</p>
		<p class="rich-link-href">
		https://maplibre.org/maplibre-gl-js-docs/example/change-case-of-labels/
		</p>
	</div>
</a></div>

大本すぎて js になってる。

ググる力も最弱なのか、初心者に優しいものどころか、全く望むものにヒットしない。
しょうがない。これらとか色々ヒントにさせてもらいながらごにょっる。

```elixir
alias MapLibre, as: Ml
```

```elixir
Ml.new(
  zoom: 5,
  style: :street,
  center: {-117.6981659, 35.873333, 9.22}
)
|> Ml.add_geo_source("data", geo_json)
|> Ml.add_layer(
  id: "stop",
  source: "data",
  type: :circle,
  paint: [circle_color: "#aa00ff", circle_radius: 5]
  )
|> Ml.add_layer(
  id: "mag",
  type: :symbol,
  source: "data",
  layout: %{
    text_field: [:get, "mag"],
    text_offset: [0, 1.0],
    text_size: 15
  }
)
```

![[Pasted image 20230227183156.png]]

いけましたな!!!

ところで、またやってしまった!!
新規notebook作ってすぐ保存設定しないと次の日、notebook が無くなってるやつ。
でも大丈夫。

![[スクリーンショット 2023-02-28 8.53.27.png]]
オートセーブの日付フォルダーで辿れました、知らなかった。



## プロパティに色が付けられた

```elixir
Ml.new(
  zoom: 2,
  style: :street,
  center: {-117.6981659, 35.873333, 9.22}
)
|> Ml.add_geo_source("data", geo_json)
|> Ml.add_layer(
  id: "stop",
  source: "data",
  type: :circle,
  paint: [circle_color: "#aa00ff", circle_radius: 5]
)
|> Ml.add_layer(
  id: "mag",
  type: :symbol,
  source: "data",
  layout: %{
    text_field: [:get, "mag"],
    text_offset: [0, 1.0],
    text_size: 12,
    text_font: ["Open Sans Bold"],
  },
  paint: [text_color: "#aa0055"]
)
```

paint: %{text_color: "#aa0055"} でもいける。

実は、中々色がつかなかった。

最後にパイプで、
```elixir
|> Kino.MapLibre.new()
```
とやったら俄然動き出した。
付けとくか。

```elixir
Ml.new(
  zoom: 4,
  style: :street,
  center: {141.154484, 39.702053}
)
|> Ml.add_geo_source("data", geo_json)
|> Ml.add_layer(
  id: "stop",
  source: "data",
  type: :circle,
  paint: [circle_color: "#aa00ff", circle_radius: 5]
)
|> Ml.add_layer_below_labels(
  id: "mag",
  type: :symbol,
  source: "data",
  layout: %{
    text_field: [:get, "mag"],
    text_offset: [0, 1.0],
    text_size: 12,
    text_font: ["Open Sans Bold"]
  },
  paint: [text_color: "#aa0055"]
)
|> Ml.add_layer_below_labels(
  id: "sig",
  type: :symbol,
  source: "data",
  layout: %{
    text_field: [:get, "sig"],
    text_offset: [0, -1.0],
    text_size: 12,
    text_font: ["Open Sans Bold"]
  },
  paint: [text_color: "#0033bf"]
)
|> Kino.MapLibre.new()
```

![[Pasted image 20230228120031.png]]

