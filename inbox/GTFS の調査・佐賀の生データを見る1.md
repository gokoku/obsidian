---
type: note
---

	#gtfs

---
2023-02-13  14:49

# GTFS の調査・佐賀の生データを見る1

生なopenデータがある。

### 宇野市

http://www3.unobus.co.jp/opendata/

バスロケの csv データが20秒ごとに更新とかある。

静的なダイヤデータ GRFS をダウンロードした。

### 佐賀市

http://opendata.sagabus.info/


# Elixir Livebook を使う

これらは csvデータなので、とりあえず読めるようにしてみるか。

セッティング
```elixir
Mix.install([
	{:explorer, "~> 0.5"},
	{:kino, "~> 0.8"}
])
```
準備
```elixir
alias Explorer.DataFrame
alias Explorer.Series
require Explorer.DataFrame
```


読み出し方
```elixir
Path.absname('./livebookファイルのパスからのデータファイル')
|> DataFrame.from_csv!()
|> Kino.DataTable.new()

# あるいは、てゆーかこっちかな
"./gtfs-sample/saga-current/stop_times.txt"
|> Path.expand(__DIR__)
|> DataFrame.from_csv!()
|> Kino.DataTable.new()


"./gtfs-sample/saga-current/stop_times.txt"
|> Path.absname()
|> DataFrame.from_csv!()
|> Kino.DataTable.new()
```

### calendar_dates.txt

![[Pasted image 20230213151020.png]]


### calendar.txt

![[Pasted image 20230213151144.png]]
右に start_date と end_date がある。謎。

### fare_attributes.txt 
運賃だ。

![[Pasted image 20230213151458.png]]

### fare_rules.txt 
これも運賃のなんか?

![[Pasted image 20230213151552.png]]

### routes.txt
ルートだ
![[Pasted image 20230213151745.png]]
route_type の右へ route_url  route_color   route_text_color   jp_parent_route_id

### shapes.txt

![[Pasted image 20230213151939.png]]

### stop_times.txt
![[Pasted image 20230213152115.png]]
stop_sequence --> stop_headsign --> pickup_type --> drop_off_type --> shape_dist_travel --> timepoint

### stops.txt
素のままでは読めなかった。

```elixir
Mix.install([
	{:explorer, "~> 0.5"},
	{:kino, "~> 0.8"},
	{:csv, "~> 3.0"}
])
```

"parent_station" のカラムで、Int64 でパースしようとして「""」空文字だったので失敗してた。
なので、csv をそのまま読み込んで、DataFrame に出来るような形に加工して DataFrame にする。
文字列として全部パースしてくれるので読み込めた。
その後、適宜 integer に置き換える作戦。

```elixir
stops_data =
Path.absname("./gtfs-sample/saga-current/stops.txt")
|> File.stream!()
|> Enum.map(&(&1 |> String.replace("\uFEFF", "")))
|> CSV.decode!()
|> Enum.to_list()
```

\uFEFF は BOM 邪魔だから取った。
```
[
	["stop_id", "stop_code", "stop_name", "stop_desc", "stop_lat", "stop_lon", "zone_id", "stop_url",
"location_type", "parent_station", "stop_timezone", "wheelchair_boarding"],
	["1001002-01", "", "佐賀駅バスセンター 1番のりば", "", "33.26451", "130.29974",
"1001002-01", "", "0", "1001002", "", ""],
```
header を抽出して、atom にした。
```elixir
csv_header =
stops_data
|> hd
|> Enum.map(&(&1 |> String.to_atom()))
```
```
[:stop_id, :stop_code, :stop_name, :stop_desc, :stop_lat, :stop_lon, :zone_id, :stop_url,
:location_type, :parent_station, :stop_timezone, :wheelchair_boarding]
```
データ部を map 化してDataFrame化出来るような形に加工した。
```elixir
csv_maps =
	stops_data
	|> tl
	|> Enum.map(&(List.zip([csv_header, &1]) |> Enum.into(%{})))
```
```
[
	%{  location_type: "0",
		parent_station: "1001002",
		stop_code: "",
		stop_desc: "",
		stop_id: "1001002-01",
		stop_lat: "33.26451",
		stop_lon: "130.29974",
		stop_name: "佐賀駅バスセンター 1番のりば",
		stop_timezone: "",
		stop_url: "",
		wheelchair_boarding: "",
		zone_id: "1001002-01"
	},
	%{
...
```

これを DataFrame に変換する。
```elixir
stop_df =
	csv_maps
	|> DataFrame.new()
stops_df |> Kino.DataTable.new()
```
![[Pasted image 20230215135116.png]]
location_type と parent_station を数字に置き換える。
parent_station はデータが入ってないところが空文字になっているので、そのまま整数変換すると null が入る。
多分データとして null は良くない気がするので、0 で穴埋めすることにした。
```elixir
location_type = stop_df[:location_type] |> Series.cast(:integer)

parent_station = stops_df[:parent_station] |> Sedirs.cast(:integer)
p_len = length(parent_station) |> Series.to_enum() |> Enum.to_list()

fill_value =
	0
	|> List.duplicate(p_len)
	|> Series.from_list()

stop_df =
	stops_df
	|> DataFrame.put(:location_type, location_type)
	|> DataFrame.put(:parent_station, parent_station)
	|> DataFrame.mutate(parent_station: coalesce(parent_station, ^fill_value))
```
```elixir
stops_df |> Kino.DataTable.new()
```
![[Pasted image 20230215135956.png]]


### stop_times.txt

```elixir
"./gtfs-sample/saga-current/stop_times.txt"
|> Path.absname()
|> DataFrame.from_csv!()
|> Kino.DataTable.new()
```
![[Pasted image 20230215140127.png]]

### translations.txt

```elixir
"./gtfs-sample/saga-current/translations.txt"
|> Path.absname()
|> DataFrame.from_csv!()
|> Kino.DataTable.new()
```
![[Pasted image 20230215140232.png]]

### trips.txt

```elixir
Path.absname("./gtfs-sample/saga-current/trips.txt")
|> DataFrame.from_csv!()
|> Kino.DataTable.new()
```
![[Pasted image 20230215140309.png]]

![[Pasted image 20230215154522.png]]

# Field 定義


<div class="rich-link-card-container"><a class="rich-link-card" href="https://developers.google.com/transit/gtfs/reference?hl=ja#agencytxt" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://www.gstatic.com/devrel-devsite/prod/vd277a93d7226f1fcf53372e6780919bb823bca6ca1c3adbaa8a14ef6554ad67d/developers/images/opengraph/google-blue.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">リファレンス  |  静的な交通機関  |  Google Developers</h1>
		<p class="rich-link-card-description">
		
		</p>
		<p class="rich-link-href">
		https://developers.google.com/transit/gtfs/reference?hl=ja#agencytxt
		</p>
	</div>
</a></div>


### stops.txt

| field name          | type     | explain                                                    |
| ------------------- | -------- | ---------------------------------------------------------- |
| stop_id             | ID       | 停車地、駅、駅の入口を識別する ID を指定します。           |
| stop_code           | text     | 乗客が場所を識別できるような短いテキストまたは番号         |
| stop_name           | text     | 場所の名前。地元の人や旅行者が理解できる名前を使用します。 |
| stop_desc           | text     | 場所の説明                                                 |
| stop_lat            | 緯度     | 場所の緯度                                                 |
| stop_lon            | 軽度     | 場所の軽度                                                 |
| zone_id             | ID       | 停車地の運賃区間を定義します。                             |
| stop_url            | URL      | 場所の web page url                                        |
| location_type       | 列挙型   | 0: 停車地, 1: 駅, 2: 出入口, 3: 汎用ノード, 4: 乗車エリア  |
| parent_station      | ID       | 色々あるので後述                                           |
| stop_timezone       | Timezone | 場所のタイムゾーン                                         |
| wheelchair_boarding | 列挙型   | その場所から車椅子での乗車が可能かどうかを指定します。     |
|                     |          |                                                            |

parent_station : 
`stops.txt` で定義されているさまざまな場所間の階層を定義します。以下のように、親である場所の ID が含まれます。  
• **停車地 / プラットフォーム**（`location_type=0`）: `parent_station` フィールドには駅の ID が含まれます。  
• **駅**（`location_type=1`）: このフィールドは空でなければなりません。  
• **出入口**（`location_type=2`）または**汎用ノード**（`location_type=3`）: `parent_station` フィールドには駅（`location_type=1`）の ID が含まれます。  
• **乗車エリア**（`location_type=4`）: `parent_station` フィールドにはプラットフォームの ID が含まれます。  
  
条件付きで必須:  
• 場所が入口（`location_type=2`）、汎用ノード（`location_type=3`）、または乗車エリア（`location_type=4`）の場合は**必須**です。  
• 停車地 / プラットフォーム（`location_type=0`）の場合は任意です。  
• 駅（`location_type=1`）の場合は使用できません。

### routes.txt

| route_id            | ID       | 経路の識別ID                                                                   |
| ------------------- | -------- | ------------------------------------------------------------------------------ |
| agency_id           | ID       | 指定された経路の交通機関                                                       |
| route_short_name    | text     | 経路の略称                                                                     |
| route_long_name     | text     | 経路の正式名                                                                   |
| route_desc          | text     | 経路の説明                                                                     |
| route_type          | 列挙型   | 後述                                                                           |
| route_url           | url      |                                                                                |
| route_color         | color    | 経路カラー                                                                     |
| route_text_color    | color    | テキストのカラー                                                               |
| route_sort_order    | 整数(正) | 経路の順番                                                                     |
| continuous_pickup   | 列挙型   | 乗客が、交通機関車両の移動経路の途中でその車両に乗車できるかどうかを示します。 |
| continuous_drop_off | 列挙型   | 乗客が、交通機関車両の移動経路の途中でその車両から降車できるかどうかを示します |
|                     |          |                                                                                |

#### route_type :
経路で使用される輸送手段のタイプを指定します。有効なオプションは次のとおりです。  
  
`0` - 路面電車、市街電車、ライトレール。都市圏内を運行する軽量軌道交通または路面電車システム。  
`1` - 地下鉄、メトロ。都市圏内を運行する地下鉄システム。  
`2` - 電車。都市間または長距離の鉄道路線。  
`3` - バス。短距離および長距離のバス路線。  
`4` - フェリー。短距離および長距離の航路。  
`5` - ケーブルトラム。ケーブルが車両の下を通る、路面を走る鉄道車両（例: サンフランシスコのケーブルカー）。  
`6` - 索道（例: リフト、ケーブルカー、ゴンドラ、ロープウェイ）1 本または複数本のケーブルに吊り下げた客車、車両、ゴンドラ、椅子で移動する交通機関。  
`7` - 鋼索鉄道。急斜面用に設計されたすべての鉄道システム。  
`11` - トロリーバス。道路上空の架線から棹状の集電装置で集めた電気を動力として走るバス。  
`12` - モノレール。1 本のレールまたは桁を軌道とする鉄道。

#### continuous_pickup :
乗客が、交通機関車両の移動経路の途中でその車両に乗車できるかどうかを示します。車両の移動経路は、その経路を運行するすべてのルートについて [`shapes.txt`](https://developers.google.com/transit/gtfs/reference?hl=ja#shapestxt) で記述します。有効なオプションは次のとおりです。  
  
`0` - 任意の停車地で乗車可。  
`1` または空 - 任意の停車地で乗車不可。  
`2` - 任意の停車地での乗車を手配するには、交通機関に乗車予約の電話が必要。  
`3` - 任意の停車地での乗車を手配するためには、運転手への事前連絡が必要。  
  
`routes.txt` で定義された任意の停車地での乗車のデフォルトの処理は、[`stop_times.txt`](https://developers.google.com/transit/gtfs/reference?hl=ja#stop_timestxt) でオーバーライドできます。

#### continuous_drop_off :
乗客が、交通機関車両の移動経路の途中でその車両から降車できるかどうかを示します。車両の移動経路は、その経路を運行するすべてのルートについて `shapes.txt` で記述します。有効なオプションは次のとおりです。  
  
`0` - 任意の停車地で降車可。  
`1` または空 - 任意の停車地で降車不可。  
`2` - 任意の停車地での降車を手配するには、交通機関への降車予約が必要。  
`3` - 任意の停車地での降車を手配するには、運転手への事前連絡が必要。  
  
`routes.txt` で定義された任意の停車地での乗車のデフォルトの処理は、`stop_times.txt` でオーバーライドできます。


### trips.txt

| route_id              | routes.route_id     | 経路ID                                                                             |
| --------------------- | ------------------- | ---------------------------------------------------------------------------------- |
| service_id            | calendar.service_id | 1 つまたは複数の経路で、サービスを提供する日付のセットを識別する ID を指定します。 |
| trip_id               | ID                  | ルート識別ID                                                                       |
| trip_headsign         | text                | 乗客に目的地を知らせる行先案内に表示されるテキスト。                               |
| trip_short_name       | text                | 一般公開のテキスト                                                                 |
| direction_id          | 列挙型              | ルールの進行方向の指定                                                             |
| block_id              | ID                  | ルートが属するブロックの識別ID                                                     |
| shape_id              | shapes.shape_id     | このルートの車両移動経路を地図上に描くシェイプを定義します。                       |
| wheelchair_accessible | 列挙型              | 車椅子対応状況を示します。                                                         |
| bikes_allowed         | 列挙型              | 自転車が許可されているかどうかを示します。                                         |
|                       |                     |                                                                                    |

#### direction_id : 列挙型
ルートの進行方向を指定します。このフィールドは経路計算には使用されませんが、時刻表を表示する際にルートの方向を区別するために使われます。有効なオプションは次のとおりです。  
  
`0` - ある方向の便（下り線など）  
`1` - 逆方向の便（上り線など）。

---

例: `trip_headsign` と `direction_id` の両方のフィールドを使用して、一組のルートセットの各方向の便に名前を割り当てることができます。その場合、[`trips.txt`](https://developers.google.com/transit/gtfs/reference?hl=ja#tripstxt) ファイルには、時刻表用に次のレコードを指定できます。  
`trip_id,...,trip_headsign,direction_id`  
`1234,...,Airport,0`  
`1505,...,Downtown,1`

### stop_times.txt

| field name          | type          | explain                                                                                  |
| ------------------- | ------------- | ---------------------------------------------------------------------------------------- |
| trip_id             | trips.trip_id | ルート識別ID                                                                             |
| arrival_time        | 時刻          | 経路を運行する特定のルートに含まれる特定の停車地の到着時刻。                             |
| departure_time      | 時刻          | 経路を運行する特定のルートに含まれる特定の停車地からの出発時刻。                         |
| stop_id             | stops.stop_id | 停車地を識別する ID を指定します。                                                       |
| stop_sequence       | 整数(正)      | 特定のルートの停車地の順序。連続してなくてもいい                                         |
| stop_headsign       | text          | 乗客に目的地を知らせる行先案内に表示されるテキスト trips.trip_headsign を override       |
| pickup_type         | 列挙型        | 乗車方法                                                                                 |
| drop_off_type       | 列挙型        | 降車方法                                                                                 |
| continuous_pickup   | 列挙型        | 乗客が、交通機関車両の移動経路の途中でその車両に乗車できるかどうかを示します。           |
| continuous_drop_off | 列挙型        | 乗客が、交通機関車両の移動経路の途中でその車両から降車できるかどうかを示します。         |
| shape_dist_traveled | Float(正)     | 最初の停車地からこのレコードで指定された停車地まで、シェイプに沿って移動する実際の距離。 |
| timepoint           | 列挙型        | 停車地の到着時刻と出発時刻が車両の正確な運行時刻であるか、概算時刻かつ補間時刻（またはそのいずれか）であるかを指定できます。                                                                                         |

#### pickup_type : 列挙型
乗車方法を示します。有効なオプションは次のとおりです。  
  
`0` または空 - 時刻表記載の乗車地。  
`1` - 乗車不可能。  
`2` - 交通機関に乗車予約の電話が必要。場合により通過、経路変更あり。  
`3` - 運転手への事前連絡が必要。場合により通過、経路変更あり。


#### drop_off_type : 列挙型
降車方法を示します。有効なオプションは次のとおりです。  
  
`0` または空 - 時刻表記載の下車地。  
`1` - 下車不可能。  
`2` - 交通機関に下車予約の電話が必要。場合により通過、経路変更あり。  
`3` - 運転手への事前連絡が必要。場合により通過、経路変更あり。

### calendar.txt

| field name | type   | explain                                                                                                   |
| ---------- | ------ | --------------------------------------------------------------------------------------------------------- |
| service_id | ID     | 1 つまたは複数の経路でサービスを提供する日付のセットを一意に識別する ID を指定します。                    |
| monday     | 列挙型 | `start_date` と `end_date` のフィールドで指定された期間内で、毎週月曜日に運行されるかどうかを指定します。 |
| tuesday    | 列挙型 |                                                                                                           |
| wednesday  | 列挙型 |                                                                                                           |
| thursday   | 列挙型 |                                                                                                           |
| friday     | 列挙型 |                                                                                                           |
| saturday   | 列挙型 |                                                                                                           |
| sunday     | 列挙型 |                                                                                                           |
| start_date | 日付   | サービス提供期間の最初の日。                                                                              |
| end_date   | 日付   | サービス提供期間の最後の日。ここで入力した日付はサービス提供期間に含まれます。                                                                                                          |

#### monday : 列挙型
`   start_date` と `end_date` のフィールドで指定された期間内で、毎週月曜日に運行されるかどうかを指定します。なお、[`calendar_dates.txt`](https://developers.google.com/transit/gtfs/reference?hl=ja#calendar_datestxt) で特定の日付を例外として列挙することができます。有効なオプションは次のとおりです。  
  
`1` - 所定の期間の毎週月曜日に運行されることを示します。  
`0` - 所定の期間の月曜日には運行されないことを示します。


### calendar_dates.txt

推奨される使い方: [`calendar.txt`](https://developers.google.com/transit/gtfs/reference?hl=ja#calendartxt) とともに [`calendar_dates.txt`](https://developers.google.com/transit/gtfs/reference?hl=ja#calendar_datestxt) を使用して、[`calendar.txt`](https://developers.google.com/transit/gtfs/reference?hl=ja#calendartxt) で定義されたデフォルトの運行パターンの例外を定義します。

| field name     | type                | explain                                                                       |
| -------------- | ------------------- | ----------------------------------------------------------------------------- |
| service_id     | calendar.service_id | 運行の例外が 1 つ以上の経路で発生する日付のセットを識別する ID を指定します。 |
| date           | 日付                | サービス例外が発生する日付                                                    |
| exception_type | 列挙型              | `date` フィールドで指定した日付に運行されるかどうかを指定します。             |
|                |                     |                                                                               |

#### exception_type : 列挙型
`date` フィールドで指定した日付に運行されるかどうかを指定します。有効なオプションは次のとおりです。  
  
`1` - 指定された日には、運行が追加されます。  
`2` - 指定された日には、運行が中止されます。

---

_例: 休日に利用できるルートのセットと、休日以外に利用できるルートの別のセットが指定された経路があるとします。その場合、通常の運行スケジュールに対応する `service_id` と、休日スケジュールに対応する別の `service_id` の両方を指定できます。該当する休日には [`calendar_dates.txt`](https://developers.google.com/transit/gtfs/reference?hl=ja#calendar_datestxt) ファイルを使ってその日を休日の `service_id` に追加し、平日の `service_id` スケジュールから除外します。_

### fare_attributes.txt
| field name        | type             | explain                                          |
| ----------------- | ---------------- | ------------------------------------------------ |
| fare_id           | ID               | 運賃クラス識別ID                                 |
| price             | Float            | 運賃の金額                                       |
| currency_type     | 通貨コード       | 運賃の支払い通貨                                 |
| payment_method    | 列挙型           | 運賃を支払うタイミングを示します。               |
| transfers         | 列挙型           | この運賃で許容される乗り換えの回数を指定します。 |
| agency_id         | agency.agency_id | この運賃に関連付ける交通機関の ID を指定します。 |
| transfer_duration | 整数             | 乗り換えの有効期限が切れるまでの時間（秒）                                                 |

#### payment_method : 列挙型
運賃を支払うタイミングを示します。有効なオプションは次のとおりです。  
  
`0` - 乗車後に支払う。  
`1` - 乗車前に支払う。

#### tranfers : 列挙型
この運賃で許容される乗り換えの回数を指定します。通常、必須フィールドには必ず値を指定する必要がありますが、このフィールドは例外的に空にすることができます。有効なオプションは次のとおりです。  
  
`0` - この運賃で乗り換えることは不可能。  
`1` - 1 回の乗り換えが可能。  
`2` - 2 回の乗り換えが可能。  
空 - 無制限に乗り換えることが可能。


### fare_rules.txt

| field name     | type                    | explain                                                                                                                                                                                                |
| -------------- | ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| fare_id        | fare_attributes.fare_id | 運賃クラスを識別する ID                                                                                                                                                                                |
| route_id       | routes.route_id         | 運賃クラスに関連付けられた経路を指定します。                                                                                                                                                           |
| origin_id      | stops.zone_id           | 出発地区間を指定します。運賃クラスに出発地区間が複数ある場合、[`fare_rules.txt`](https://developers.google.com/transit/gtfs/reference?hl=ja#fare_rulestxt) に各 `origin_id` 用のレコードを作成します。 |
| destination_id | stops.zone_id           | 目的地区間を指定します。                                                                                                                                                                               |
| contains_id    | stops.zone_id           | 特定の運賃クラスの使用中に乗客が通過する区間を指定します。                                                                                                                                             |
|                |                         |                                                                                                                                                                                                        |

### shapes.txt
| field name          | type  | explain                                                                                               |
| ------------------- | ----- | ----------------------------------------------------------------------------------------------------- |
| shape_id            | ID    | シェイプID                                                                                            |
| shape_pt_lat        | 緯度  | シェィプポイントの緯度                                                                                |
| shape_pt_lon        | 経度  |                                                                                                       |
| shape_pt_sequence   | 整数  | シェイプ ポイントが連結してシェイプを形成する順序                                                     |
| shape_dist_traveled | Float | 最初のシェイプ ポイントからこのレコードで指定されたポイントまで、シェイプに沿って移動する実際の距離。 |
|                     |       |                                                                                                       |

### frequencies.txt
| field name   | type          | explain                                                                                                   |
| ------------ | ------------- | --------------------------------------------------------------------------------------------------------- |
| trip_id      | trips.trip_id | 運転時隔を適用するルートの ID                                                                             |
| start_time   | 時刻          | 運転時隔を適用するルートの始点となる停車地で、最初の車両が出発する時刻。                                  |
| end_time     | 時刻          | ルートの始点となる停車地で、運行間隔を変更する時刻または運行を停止する時刻。                              |
| headway_secs | 整数          | このルートの同じ駅または停留所からの出発間隔（秒単位）。`start_time` から `end_time` までの運行間隔です。 |
| exact_times  | 列挙型        | ルートのサービスの種類を示します                                                                          |
|              |               |                                                                                                           |

#### exact_times : 列挙型
ルートのサービスの種類を示します。詳しくは、ファイルの説明を参照してください。有効なオプションは次のとおりです。  
  
`0` または空 -運転時隔ベースのルート。  
`1` - 1 日を通して運行間隔が正確に維持される、運行スケジュール ベースのルート。この場合、`end_time` の値は最終希望のルートの `start_time` より大きく、かつ最終希望のルートの `start_time` + `headway_secs` よりも小さい値にする必要があります。


## ブロックと運行日の表示例

| route_id | trip   | service_id                     | block_id | 最初の停車時刻 | 最後の停車時刻 |     |
| -------- | ------ | ------------------------------ | -------- | -------------- | -------------- | --- |
| red      | trip_1 | mon-tues-wed-thurs-fri-sat-sun | red_loop | 22:00:00       | 22:55:00       |     |
| red      | trip_2 | fri-sat-sun                    | red_loop | 23:00:00       | 23:55:00       |     |
| red      | trip_3 | fri-sat                        | red_loop | 24:00:00       | 24:55:00       |     |
| red      | trip_4 | mon-tues-wed-thurs             | red_loop | 20:00:00       | 20:50:00       |     |
| red      | trip_5 | mon-tues-wed-thurs             | red_loop | 21:00:00       | 21:50:00       |     |
|          |        |                                |          |                |                |     |


![[Pasted image 20230215172355.png]]

