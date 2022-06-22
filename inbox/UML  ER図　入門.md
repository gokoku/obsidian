#uml

---
2022-04-15  11:32

# UML  ER図　入門


<div class="rich-link-card-container"><a class="rich-link-card" href="https://products.sint.co.jp/ober/blog/create-er-diagram" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://products.sint.co.jp/hubfs/shutterstock_572934004%20%281%29.jpg#keepProtocol')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">ER図とは？書き方やテクニックをわかりやすく解説</h1>
		<p class="rich-link-card-description">
		最近ではデータベースがクラウドサービス化されたことでデータベースを意識しない開発も可能となったことから、若手のエンジニアの中には「ER図を知らない」という方もいるのではないでしょうか？はじめてER図を書く方向けに、ER図の概要や書き方、テクニックについてご紹介します。
		</p>
		<p class="rich-link-href">
		https://products.sint.co.jp/ober/blog/create-er-diagram
		</p>
	</div>
</a></div>

## ER図のデータモデル
* 概念モデル
* 論理モデル
* 物理モデル


#### 概念モデル
- エンティティ　もの
	- イベントエンティティ
		- マスターテーブルになる Entity
	- リソースエンティティ
		- トランザクションテーブルになるEntity
		- トランザクション管理する
- リレーションシップ

完成
![[Pasted image 20220415130226.png |400]]

向き、ショップ(主語)から商品(目的語)の方向

依存リレーションシップ
- 実線
- 親Entity が無いと子Entity も存在しない関係

非依存リレーションシップ
- 点線
- 親がいなくても子はいられてる関係

多対多リレーションシップ

カーディナリ

#### 論理モデル

アトリビュートの追加

![[Pasted image 20220415131018.png|400]]

カーディナリ(多重度)

「片方のエンティティ１レコードに対し、もう一方エンティティが何レコードになるのか」
![[Pasted image 20220415131226.png]]

#### 物理モデル

仲介Entity を設けて多対多リレーションを表すとのこと、
てことは中間テーブル? 

![[Pasted image 20220415131523.png |400]]

角丸が中間テーブルに当たるやつか。

![[Pasted image 20220415131628.png]]


### 正規化
ER図の設計テクニックに正規化が位置するのか。

- 第一正規化
	- ![[Pasted image 20220415132239.png]]
	- 繰り返し項目を取る
- 第二正規化
	- ![[Pasted image 20220415132334.png]]
	- 従属するアトリビュートを見つけて、別Entity に分割する。
- 第三正規化
	- ![[Pasted image 20220415132648.png]]
	- 主キー属性が無い項目を見つけて、別 Entity に分割する。





## IE記法
https://it-koala.com/entity-relationship-diagram-1897

<div class="rich-link-card-container"><a class="rich-link-card" href="https://it-koala.com/entity-relationship-diagram-1897" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://it-koala.com/wp-content/uploads/2016/09/shutterstock_189001583.jpg')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">若手プログラマー必読！５分で理解できるER図の書き方５ステップ</h1>
		<p class="rich-link-card-description">
		この記事では、ER図の基礎知識から５つの作成ステップまで、若手エンジニアが抑えておくべきER図の全知識をどの記事よりも分かりやすく解説します。この記事を参考にER図を書いてみましょう。
		</p>
		<p class="rich-link-href">
		https://it-koala.com/entity-relationship-diagram-1897
		</p>
	</div>
</a></div>

![[Pasted image 20220415133232.png]]

- 依存リレーションの書き方
	- ![[Pasted image 20220415133502.png]]
	- 依存する子となるEntity は角丸四角で書く。
- 非依存リレーションの書き方
	- ![[Pasted image 20220415133544.png]]
- 1対1の関係
	- ![[Pasted image 20220415134432.png |400]]
- 1対多の関係(曖昧なので下のどれかに定義する)
	- ![[Pasted image 20220415134128.png|400]]
	- 1対0以上の関係
		- ![[Pasted image 20220415134058.png|400]]
	- 1対1以上の関係
		- ![[Pasted image 20220415134306.png |400]]
- 多対多の関係
	- ![[Pasted image 20220415134459.png |400]]
- 0または1対多の関係
	- ![[Pasted image 20220415134542.png |400]]

### Entityをマスター系とトランザクション系に分ける

名詞っぽい、静的な、物的データがマスタ系?
動詞っぽい、動的な、アクション履歴系がトランザクション系?

