#obsidian 

---
[[2022-01-21]]

# Templater の使い方教えてくれ

https://silentvoid13.github.io/Templater/


もうちっとわかりやすいものは?

https://www.takizui.com/%E3%82%A2%E3%83%97%E3%83%AA/Mac/Obsidian+-+Templater

従来との違い

カーソルの位置をコントロール出来るらしい。<- これは早速やってみる。いつもカーソル移動させてダルいと思ってた。

### カーソル位置をコントロールしよう

```ad-tip
<% tp.file.cursor(?) %>

?は数字

```


どーいうこと?

まず、どうやって導入するのか。従来のと exchange する。つまり、Templates を disable にした。

```ad-note
<% tp.file.cursor(1) %>

<% tp.file.cursor(2) %>

<$ tp.file.cursor(3) %>


と Templater ファイルに記述があると、 Opt + TAB で移動する。

```
 hotkey “Templater: Jump to next cursor location”　で設定できるのかな。
 
ここにあった。 設定 -> Hotkeys

![[Pasted image 20220121101335.png]]



### 今日の日付を入れる


```ad-tip

<% tp.date.now("YYY-MM-DD") %>
<% tp.date.now("YYY/MM/DD HH:mm") %>

```

[[2022-01-21]]

2022/01/21  10:21

### タイトル

今まで、ファイル名入れて、またタイトル入れてと怠かった。

これで一発だ。

```ad-tip

<% tp.file.title %>

```


# Daily と inbox 記事だけにしてみる

## Daily

Daily Template は カレンダーから参照させればいい。

Calendar で click したら Daily notes を開くので、こちらでテンプレート名を指定する。

![[Pasted image 20220121104616.png]]

```daily_template
[[daily notes]]
# <% tp.date.now("YYYY-MM-DD") %>

## <% tp.file.cursor(1) %>
```


## inbox 記事

いつもの何でも記事にしてぶっこむやつ。

- cmd + n で新規ノート
- opt + i で inbox template を insert する。
- opt + TAB でカーソルを移動する。

で使うことにしよう。

