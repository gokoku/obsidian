#watson #raspberry_pi 

---
2022-03-24  13:38

# watson  Assistant の設定しよう

### おさらい
Assistant にスキルを追加してカスタマイズする。

タイプごとに一つのスキルをアシスタントに追加出来る。
* アクション・スキル : 会話フローを作成してインターフェースを構成する
* ダイアログ・スキル : 自然言語処理でユーザーの質問と要求を理解して対応する
* 検索スキル : 有料サービスでのみ使える。ユーザー照会して色々出来るみたい。


ダイアログスキル+アクションスキルの場合、ダイアログスキルが使用されて、ダイアログからアクションスキルを呼び出して個々のアクションが処理出来るらしい。

## ダイアログ・スキル

##### インテント
ユーザー入力の目的。ユーザー要求の各タイプに対して定義する。
インテント名には # がつく。
多くのユーザー入力サンプルを用意して、入力サンプルがどのインテントにマップされるかトレーニングすることで、ダイアログ・スキルがインテントを認識出来るようにする。


##### ダイアログ
定義ずみインテントとエンティティを認識した時に、どのように対応するかを定義する会話フローブランチ。

###### エンティティ
質問の微妙な違いを処理出来るようにするには、エンティティを定義してダイアログで参照するようにする。

@が接頭部。

インテントの特定の用語、オブジェクト名?

スキルでエンティティが認識されるようにするには、エンティティ用語の値や同義語、エンティティパターンを提供したり、センテンスの中で典型的に使われるコンテキストを識別したりさせられるらしい。

ダイアログの微調整はノードに戻り、インテントだけでなく、エンティティがあるか検査するノードを追加する。

## エンティティが値を保持したままだ
一度スロットを通したら、おすすめで何にも聞かれず、一択状態になってしまった。
一回聞かれたことをずっと保持してるからだ。

###### 解決した
Youtube で見つけたやり方で同じ現象の対応をみた。


<div class="rich-link-card-container"><a class="rich-link-card" href="https://www.youtube.com/watch?v=rGxjoCLyoM0" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://www.youtube.com/embed/rGxjoCLyoM0?feature=oembed')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">IBM Watson Conversation - Using Slots</h1>
		<p class="rich-link-card-description">
		This video will help you understand how to use IBM Watson Conversation's feature - Slots.For more blog posts on IBM Watson check out cognitivewatson.blogspot...
		</p>
		<p class="rich-link-href">
		https://www.youtube.com/watch?v=rGxjoCLyoM0
		</p>
	</div>
</a></div>

ノードをスロットとマルチコンディションにチェックを入れて、スロットをセットする。

![[Pasted image 20220325131253.png]]

ここで @gender と @age がセットされたままにならないように、この後の処理で、値に NULL を入れれば良い。

![[Pasted image 20220325131652.png]]

NULL が @gender に入ったのか、$gender に入ったのかはよくわからない。
どっちでもないかもしれない。
とりあえずこの context で使用されていた gender の入れ物が空になったようで、

保持されなくなった。

## まとめ

ダイアログのノードで会話パターンを補足して、

「目的別」に Intents にアサインさせて、

最小粒度の「言葉」を Entities で補足するようにして、

ノードの個別の対応をフロー制御に使った。

対応させたいノードに流れが行くように、Intents でインプットパターンを増やして補足出来るように調整する。

あるいは Intents と組み合わせで Entities を使って補足する。

Entities には類義語でぶれを吸収出来るようにする。

ダイアログのノードパターンは、挨拶などの「導入部」、間とか繋ぎとか潤滑剤的な「雑談部」、ズバリ商品を薦める「おすすめ部」にパターン分けをした。




[[watson  遠野AI のダイアログ設計]]









