#blender

---
2022-04-08  09:20

# blender  街が一発で作成ってスゲー!!


<div class="rich-link-card-container"><a class="rich-link-card" href="https://www.youtube.com/watch?v=uk404c43pRY" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://www.youtube.com/embed/uk404c43pRY?feature=oembed')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">【blender】街を一発で作成（Google map + アドオン）</h1>
		<p class="rich-link-card-description">
		ご視聴頂きありがとうございます！Blender GISというアドオンを用いて、google mapをベースにした街のマップを取り込み、立体化します。Blender GIShttps://github.com/domlysz/BlenderGISビルのテクスチャhttps://www.textures.com/do...
		</p>
		<p class="rich-link-href">
		https://www.youtube.com/watch?v=uk404c43pRY
		</p>
	</div>
</a></div>


Blender GIS [https://github.com/domlysz/BlenderGIS](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbHJqS3ZnbTZmMGxWbmZuUjhmdjVha1pvWlVnUXxBQ3Jtc0ttcHM1Q1c1anVEdDRCcVRpU0dDM3lfb0JrVnRzQ2hDXzlSQTR0dFNYQXd2UGdzWW9DYjlBOWE0VkJJT0dsMTJ4OHR3eHBZZmRQLU9zZlA0TUtrMmRaSlhoSE4wbXE0ZjYzdGhWbUlJR3FJVFd4MHNtWQ&q=https%3A%2F%2Fgithub.com%2Fdomlysz%2FBlenderGIS)


ビルのテクスチャ [https://www.textures.com/download/PBR...](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbVN2aS1nazM5TXR0Ykt5V2pvYnFrUjdyOVhRUXxBQ3Jtc0ttdkNlWW9sUmc0UFRHX044WU1ZQWZ1bjdFcEFfcmlqMF9Vb3ZXWEVaT1FSMS04RWhpdTVHVjJxbFFsTmZPUjdRZjJoRHp4eUJBWkF4YlZRVFRRVzlhREt6dWpXOXgyZjdYTHNxRWpuQ0xzejVfZXdXWQ&q=https%3A%2F%2Fwww.textures.com%2Fdownload%2FPBR0537%2F138572) 


▼本動画で主に使用するblenderショートカット一覧 [https://drive.google.com/drive/folder...](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbDBjWmppUG53UFVvMUlZT3RCZGhySUY4ZWV1d3xBQ3Jtc0tsLWI0clN0WVpuNTh2OURMSUR1OC16R2Y4U2VOb1RoLTNEVExLcVRDbVJaN2UxU19VRG1qY1ZQWS1fWGplby02ZndfSHFmTTNpdWRoWjZqTERWOVdyUGYwRXpYTkdDV1loaEpRNlgwU2Z6VTRCMDYzMA&q=https%3A%2F%2Fdrive.google.com%2Fdrive%2Ffolders%2F1GzVBrIHV1lbuo_QO-aV5NyZKe3N2JUJY%3Fusp%3Dsharing)

▼Patreonでご支援して下さると嬉しいです！（月々3ドルより） [https://www.patreon.com/Mdesign_](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbmVXWGpXdUpDNkh5d3lzQjF2SUpEd1Yya3BJQXxBQ3Jtc0tuQjQ0Uk1iVEtEMy1iUF92bTI4OGZGX21LRHJrbkV5c3pYTVZKUEpiWTgtT0ZqY1lwMm5sbjd3NnZId0I5dEpxdTNuZ0ZLWExpajBUNkNwZkxUQ2NTMHAtOGgzSmF3NXBEQlBQVUtzWVVnY3RVTGJCVQ&q=https%3A%2F%2Fwww.patreon.com%2FMdesign_) 

▼操作について カーソル位置に表示されるショートカットコマンドをご参考ください。 ・NUMPAD；テンキー ・OSKEY；macのCommand



Mdesignさんスゲー!!


## やってみる
zip を DL

preference から install で zip のまま選択。
チェック入れると、メニューに出る。

![[Pasted image 20220408092903.png]]
GIS をクリック
![[Pasted image 20220408092929.png]]

Web geodata --> Basemap

![[スクリーンショット 2022-04-08 9.31.59.png]]
OK で地図が出る。

![[スクリーンショット 2022-04-08 9.32.07.png]]

ここで + - キー(テンキー)で拡大縮小、中ボタンで移動。

g キーで goto ダイアログで指定出来る。
![[スクリーンショット 2022-04-08 9.38.12.png]]

Goto --> Harajuku とか
確定前は gキーでZoom level を変えられる。

eキーで確定。

![[スクリーンショット 2022-04-08 9.38.26.png]]

このマップを選択した状態で高さデータを取得する、と言っている。

![[スクリーンショット 2022-04-08 9.39.18.png]]

![[スクリーンショット 2022-04-08 9.39.24.png]]

OSM データとは何だろう。
ここで取得したいデータを複数選択。

![[スクリーンショット 2022-04-08 9.39.39.png]]

ビルと鉄道。OKクリック。

![[スクリーンショット 2022-04-08 9.40.51.png]]

一瞬で街が出来た。

![[スクリーンショット 2022-04-08 9.42.10.png]]

まず、ここまで、この先が凄い。ノード使いまくりーのテクスチャ貼りーの
ライティング自由自在だの。

- [ ] この続きが見たい #todo 


