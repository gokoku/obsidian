#kintone

---
2021-12-08

# Kintone 入門

アプリ作成する。

歯車マークでフォームが構成出来る。

### Form

![[Pasted image 20211208113019.png]]

ここで、フィールドコードに名前を設定すると、js でアクセス出来る。

![[Pasted image 20211208113116.png]]

Field Code がそれ。

![[Pasted image 20211208113142.png]]

### Views

ここでは、View を幾つも持てる。見せる項目を編集できる。

### App Settings

JavaScript and CSS Customization で js や css をアップロードする。

Plug-ins でで JSEdit for kintone をadd すると、ここから js が追加・編集出来る。

![[Pasted image 20211208113720.png]]

このエディタは歯車マークをクリックする。

![[Pasted image 20211208113837.png]]

このJSEditor の下にドキュメントのリンクがある。API DOcs とか便利。

テンプレートで jQuery があるけど、まんまではないと言われる。

Libraries で jQuery を選んでおく。


## JS

sample.js

```js
jQuery.noConflict();
(function($) {
   "use strict";
   kintone.events.on("app.record.index.show", function(e) {
       
       const records = e.records
       records.forEach( record => {
	    console.log(record.class.value)
          console.log(record.hobby.value)
       })
       
   });
})(jQuery);

```

events.on("app.record.index.show") は一覧ページロード時のイベント。

値の取得は、event.records.フイールドコード.value でカラムの一括取得が出来る。

フィールドコードはフォームの設定でつけた名前。