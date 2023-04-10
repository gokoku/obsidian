---
type: note
---

#rails #js

---
2022-11-11  16:52

# rails ERBからJavaScriptにhashを渡す方法

https://re-engines.com/2017/08/30/custom-data-attribute/

コントローラでインスタンスを用意する。

```ruby
  @designed_document = DesignedDocument.find(params[:id])
```

erb ビューではカスタムデータAttribute にセットする。
それ経由でJS で値を受け取る。
どうも、パーシャルでもコントローラのインスタンス変数が受け取れるようになってた。

```ruby
<div id="ddDocument" data-designed-document="<%= @designed_document.id %>"


<script>

const docElement = document.querySelector('#ddDocument')
const doc = docElement.getAttribute('data-designed-document')
console.log(doc)

</script>
```

javascript_tag を使わなくても済む。

Hash で渡して JSON で受け取れるようだ。

