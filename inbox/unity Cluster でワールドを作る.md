---
type: note
---

#unity #cluster

---
2022-07-28  13:27

# unity Cluster でワールドを作る

Unity の手始めに cluster のワールドを作ってみたいのだ。

ぴーぷるの VR 会議室を作ってみよう。


まず、基礎から学ぼう。

<div class="rich-link-card-container"><a class="rich-link-card" href="https://creator.cluster.mu/" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://creator.cluster.mu/wp-content/uploads/2021/01/cover-5.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Cluster Creators Guide</h1>
		<p class="rich-link-card-description">
		メタバースのワールドのつくり方を学ぶためのTipsを発信しています。UnityとCluster Creator Kitを使ってスマホからVR、あらゆるデバイスから入ることができる自分だけのワールドをつくろう！
		</p>
		<p class="rich-link-href">
		https://creator.cluster.mu/
		</p>
	</div>
</a></div>

Unity のバージョンをここで選んでダウンロードする。

https://unity3d.com/jp/get-unity/download/archive

2021.3.4f1 が Creator Kit の推奨なので、このページで Hub にDLする。

![[Pasted image 20220728135921.png]]


project をクリックして　OPEN でダウンロードした template を開く。

![[Pasted image 20220728144625.png]]

ダブルクリックする。
する。

![[Pasted image 20220728144658.png]]

すごく時間がかかるけど、待っていれば立ち上がる。



- new sceen を追加する。

Assets --> create --> sceen

![[Pasted image 20220728151251.png]]


Main Camera を削除するとのこと。

- 地面を作る

![[Pasted image 20220728151541.png]]

これが Collider が設定されている Object だから、地面にするらしい。

![[Pasted image 20220728152018.png]]

![[Pasted image 20220728152027.png]]

なんか向きが違う。


- SpawnPoint を作る

![[Pasted image 20220728151503.png]]

Add Component で search

![[Pasted image 20220728152159.png]]

![[Pasted image 20220728152325.png]]

赤い線が向きとのことで、180 度回転させたが、ビデオと違う。

![[Pasted image 20220728152409.png]]

- DespawnHeight を作る

GameObject --> create empty 

![[Pasted image 20220728152452.png]]

![[Pasted image 20220728152525.png]]

地面より下にする。

フライスルーモード : 右クリック + wasd   qe 上下

## Asset Store からダウンロード

まず、失敗する。コンソールで、

![[Pasted image 20220728163253.png]]

Unity Hub を再起動したら Package Maneger で　Sign in がきた。

![[Pasted image 20220728170438.png  | 300]]

Sign in でいけた!!

import で

![[スクリーンショット 2022-07-28 17.11.10.png | 500]]




