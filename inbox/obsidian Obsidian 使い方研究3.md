#obsidian

---
[[2022-04-25]]  09:12

# obsidian Obsidian 使い方研究3

タイトルにタグ名付けるの止めてみよう。Obsidian の検索能力はこれを必要としない。

## obsidian Front matter を使ってみる?

オリジナルのkey name で DataView で抽出することにしよっか。

記事のタイプを分けようと思う。
tag で分類するのはもうプログラム系でごちゃっと使ってるので、これはこれということで。

Database を意識した使い方にしたい。Beyond Evernote

key は、 Front matter 出なくても、記事中に :: を key の後につければOKのようだ。

タイプで分けてみる。

- タイプを記述して後でサクッと見られる的な「チートシート」的なもの。
- データベーステーブル的なもの。
- 抽出する。Viewer 的なもの。最近のマイブームとか
- 普通の記事。ノート。
- 後はその都度 expand する。

```dataview
table file.name as "Title"
where type = "data_sheet"

```






既存の template ファイルに追加する。

ディレクトリを分けるか、Vault を切り替えるか。
## Vault を切り替えてみる
まるで、別ノートを開く感じで、普段の自由自在なノートと切り替わる。

obsidian/english で Vault を作った。中に作れる。
obsidian を選ぶと english フォルダーを作ってくれた。
template も独立して設定できる。

ショートカットも個別に設定、これは面倒臭い。
もはや別アプリ。テーマも独自に出来る。

Vault を切り替えるなら、type 分けが必要ない。

特別な使い方をするためのものとなるようだ。
とりあえず、これはこのまま pendding

## type に合わせてテンプレートを変えれるようにする








