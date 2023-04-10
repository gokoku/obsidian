---
type: note
---

#gtfs 

---
2023-03-03  09:26

# 矢巾やはばすのGTFS解析

routes.txt

やはばす（矢巾町コミュニティバス）  00312-00101

![[yahaba-gtfs-やはばす 2023-03-02 16.28.00.excalidraw| 900]]


矢巾郵便局前のstop_id が入ってない。

stop_id :  00312-00000000235
stop_id :  00312-00000000262
![[Pasted image 20230303093235.png]]

どの trip_id なのか。
![[やはばす郵便局前gtfs 2023-03-03 09.33.23.excalidraw|700]]

どちらも同じだった。

00312-00007 : 北高田線
00312-00013 : 矢巾医大線


# やはばすのルートだけ取り出してみたら全体がわかった

セッティング
```elixir
Mix.install([
  {:explorer, "~> 0.5"},
  {:kino, "~> 0.8"},
  {:csv, "~> 3.0"},
  {:kino_maplibre, "~> 0.1.7"}
])
```
エリアス
```elixir
alias Explorer.DataFrame, as: Df
alias Explorer.Series, as: Sr
alias MapLibre, as: Ml
require Explorer.DataFrame
```

###  やはばす（矢巾町コミュニティバス）: route_id : 00312-00101 から trip_id を取り出せる。

```elixir
trips_df =
  Path.absname('./gtfs-sample/GTFS_矢巾_20230121/trips.txt')
  |> Df.from_csv!()

yahabus_trips_df =
  trips_df
  |> Df.filter(route_id == "00312-00101")

yahabus_trip_id_list =
  yahabus_trips_df
  |> Df.pull(:trip_id)
  |> Sr.to_list()
```

### stop_times.txt から trip_id で stop_id を取り出せる

```elixir
stop_times_df =
  Path.absname('./gtfs-sample/GTFS_矢巾_20230121/stop_times.txt')
  |> Df.from_csv!()
  
yahabus_stop_id_list =
  yahabus_trip_id_list
  |> Enum.map(fn id ->
    stop_times_df
    |> Df.filter(trip_id == ^id)
    |> Df.pull(:stop_id)
    |> Sr.to_list()
  end)
  |> List.flatten()
  |> Enum.uniq()
```

### stops.txt から stop_id で stop_name, stop_lon, stop_lat を取り出せる。

```elixir
stop_df =
  Path.absname('./gtfs-sample/GTFS_矢巾_20230121/stops.txt')
  |> Df.from_csv!()

yahabus_stop_id_list
  |> Enum.map(fn id ->
  stop_df
  |> Df.filter(stop_id == ^id)
  |> then(fn df ->
    {
      df[:stop_id] |> Sr.to_list() |> Enum.at(0),
      df[:stop_name] |> Sr.to_list() |> Enum.at(0),
      df[:stop_lon] |> Sr.to_list() |> Enum.at(0),
      df[:stop_lat] |> Sr.to_list() |> Enum.at(0)
    }
  end)
end)
```

### stopデータをGeoJSON にする
```elixir
yahabus_stop_geojson =
  %Geo.GeometryCollection{
    geometries:
      yahabus_stop_name_lon_lat_list
      |> Enum.map(fn tpl ->
        %Geo.Point{
          coordinates: {elem(tpl, 2), elem(tpl, 3)},
          properties: %{
            stop_id: elem(tpl, 0),
            stop_name: elem(tpl, 1)
          }
        }
      end)
  }
```



### shape.txt から線路のGeoJSONを作る

trips.txt から route_id : "00312-00101" で shape_id を抽出する。
```elixir
yahabus_trips_id_list =
  trips_df
  |> Df.filter(route_id == "00312-00101")
  |> Df.pull(:shape_id)
  |> Sr.to_list()
  |> Enum.uniq()
```

```elixir
shape_df =
  Path.absname("./gtfs-sample/GTFS_矢巾_20230121/shapes.txt")
  |> Df.from_csv!()

yahabus_shape_df_list =
  yahabus_trips_id_list
  |> Enum.map(fn id ->
    shape_df
    |> Df.filter(shape_id == ^id)
    |> Df.arrange_with(& &1[:shape_pt_sequence])
  end)

yahabus_shape_geojson = %Geo.GeometryCollection{
  geometries:
    yahabus_shape_df_list
    |> Enum.map(fn route ->
      lon = route[:shape_pt_lon] |> Sr.to_list()
      lat = route[:shape_pt_lat] |> Sr.to_list()
      lon_lat = lon |> Enum.zip(lat)
      %Geo.LineString{coordinates: lon_lat}
    end)
}
```



# 描画する
```elixir
Ml.new(
  center: {141.148587, 39.6131268},
  zoom: 15,
  style: :street
)
|> Ml.add_geo_source("line", yahabus_shape_geojson)
|> Ml.add_geo_source("stop", yahabus_stop_geojson)
|> Ml.add_layer(
  id: "line",
  source: "line",
  type: :line,
  paint: [line_color: "#dd00ff", line_width: 2]
)
|> Ml.add_layer(
  id: "stop_point",
  source: "stop",
  type: :circle,
  paint: [circle_color: "#ff0033", circle_radius: 5]
)
|> Ml.add_layer_below_labels(
  id: "stop_name",
  source: "stop",
  type: :symbol,
  layout: %{
    text_field: [:get, "stop_name"],
    text_offset: [0, -2.0],
    text_size: 14,
    text_font: ["Open Sans Bold"]
  },
  paint: [text_color: "#ff0033"]
)
|> Ml.add_layer_below_labels(
  id: "stop_id",
  source: "stop",
  type: :symbol,
  layout: %{
    text_field: [:get, "stop_id"],
    text_offset: [0, 1.0],
    text_size: 12,
    text_font: ["Open Sans Bold"]
  },
  paint: [text_color: "#aa00ff"]
)
```

![[Pasted image 20230303180207.png]]

# データの関係早見表

![[GTFS_table 2023-03-03 18.02.47.excalidraw | 700]]

