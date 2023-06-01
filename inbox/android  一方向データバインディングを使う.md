#android

---
2021-10-18

# 一方向データバインディングを使う

ここまでの話。

#android リスト形式でJSONを表示する]]

DataBinding の機能で 一方向データバインディングを使うと、表示情報をレイアウトXMLでコントロール
出来るという。

## レイアウトXML に一方向データバインディングを適用

res/layout/list_item_toot.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<layout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools" >

    <data>
        <variable
            name="toot"
            type="jp.co.people.sample.Toot" />
    </data>

    <androidx.constraintlayout.widget.ConstraintLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        tools:context=".MainActivity">

        <TextView
	　　　...
            android:text="@{toot.account.username}"  <--- 追加
    	      .../>
        <TextView
   	
            ...  
            android:text="@{toot.content}"   <--- 追加
            ... />

    </androidx.constraintlayout.widget.ConstraintLayout>
</layout>
```
	
## DataBinding オブジェクトにデータ設定

app/TootListAdapter.kt

```kotlin
    class ViewHolder (
        private val binding: ListItemTootBinding
        ) : RecyclerView.ViewHolder(binding.root) {
            fun bind(toot: Toot) {
                binding.toot = toot
            }
        }
```

bindingするデータを親である toot を渡す。
xml 側で取捨選択出来るようになる。


## BindingAdapter を利用する

content が HTML のままで、パースするものがないらしい。

Spanned 型に変換して、BindingAdapter を使って、View メソッドと XML 属性を結び付けるとのこと。

TextView にメソッドを追加して、加工処理を追加している。

app/DataBindingAdapter.kt

```kotlin

package jp.co.people.sample

import android.widget.TextView
import androidx.core.text.HtmlCompat
import androidx.databinding.BindingAdapter

@BindingAdapter("spannedContent")
fun TextView.setSpannedString(content: String) {
    text = HtmlCompat.fromHtml(
        content,
        HtmlCompat.FROM_HTML_MODE_COMPACT
    )
}
```

res/layout/list_item_toot.xml

```xml
        <TextView
            android:id="@+id/content"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
			
		android:text="@{toot.content}"             <---削除して
            app:spannedContent="@{toot.content}"       <---追加
   
　　　　　　　....

            />
```

content が HTML としてレンダーされて表示された。

次は、#android  Media 画像の表示]]

