---
type: note
---

#gtfs

---
2023-02-20  11:04

# GTFS データのまとめ

データ同士の連携が知りたい。

![[gtfs-relationship 2023-02-20 10.34.16.excalidraw|800]]

リファレンス

<div class="rich-link-card-container"><a class="rich-link-card" href="https://developers.google.com/transit/gtfs/reference?hl=ja" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://www.gstatic.com/devrel-devsite/prod/vb06f043a05fab8044a3ccc5b2a77caba73848fbe764e2f874782b493081fa838/developers/images/opengraph/google-blue.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">リファレンス  |  静的な交通機関  |  Google Developers</h1>
		<p class="rich-link-card-description">
		
		</p>
		<p class="rich-link-href">
		https://developers.google.com/transit/gtfs/reference?hl=ja
		</p>
	</div>
</a></div>



を元にして佐賀データを見てみる。使ってないカラムとかはスルーして情報を狭めて実用的にクリアにする。

## routes : 路線のメタ情報、名前など付帯情報


route_id : プライマリーキーと思われる。路線名
agency_id : 会社
route_long_name : 路線名
route_type :  3 (バス)


## shapes : 路線の物理的空間情報

shape_id                    : 識別シェイプ名(同じ名前でグルーピングしてるようだ)
shape_pt_lat             : シェイプポイントの緯度
shape_pt_lon            :  シェイプポイントの経度
shape_at_sequence : シェイプポイントの順番。非連続値OK。進行方向に増加。
shape_dist_travel     : 距離。一つ前のシェイプポイントからこのポイントまでの距離だろうか?

## calendar_dates

service_id         :  サービス(運行?)日付の一意な ID
date                   : このサービス例外が発生する日付
exception_type : 1-指定された日に運行が追加される
                            2-指定された日に運行が中止される

## calendar

service_id  : サービス(運行?)日付の一意な ID
monday     :  1 , 0 のテーブルみたいに見えるディシジョンテーブル? 
tuesday     : 
wednsday : 
thursday   : 
friday        : 
saturday   : 
sunday     : 
start_date : サービス提供の始まり日
end_date  : サービス提供の終わり日

| service_id | monday | tuesday | wednsday | thursday | friday | saturday | sunday |
| ---------- | ------ | ------- | -------- | -------- | ------ | -------- | ------ |
| 1_平日     | 1      | 1       | 1        | 1        | 1      | 0        | 0      |
| 1_土日祝   | 0      | 0       | 0        | 0        | 0      | 1        | 1      |
| 3_平日     | 1      | 1       | 1        | 1        | 1      | 0        | 0      |
| 3_土曜     | 0      | 0       | 0        | 0        | 0      | 1        | 0      |



## stop_times


## stops

location_type : 0-停車地(parent_stationがあるときはプラットフォーム)
                          1-駅
parent_station : location_type=0のときは停留所/プラットフォーム。駅の時は空にすること
stop_id  : 場所のID
stop_lat : 場所の緯度
stop_lon : 場所の経度
stop_name : 場所の名
zone_id :  停車地の運賃区間

場所とは、停車地、駅、駅の入り口



## trips










佐賀県のバス路線の時刻表が引けるよ。

https://www.navitime.co.jp/bus/diagram/
<div class="rich-link-card-container"><a class="rich-link-card" href="https://www.navitime.co.jp/bus/diagram/" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://www.navitime.co.jp/static/pc/l/202302241805/img/common/favicon.ico')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">バス時刻表 - NAVITIME</h1>
		<p class="rich-link-card-description">
		路線バス、高速バス、空港バス、深夜バスの時刻表やバス停の地図、主要駅のバスターミナルの地図も見られます。バスのお出かけ前にもNAVITIMEでチェック！
		</p>
		<p class="rich-link-href">
		https://www.navitime.co.jp/bus/diagram/
		</p>
	</div>
</a></div>

