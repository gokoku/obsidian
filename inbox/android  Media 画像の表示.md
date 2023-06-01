#android

---
2021-10-18

# Media 画像の表示

ここまで、

#android  一方向データバインディングを使う]]


## Media 取得出来るように準備する

app/Media.kt  <--新規

```kotlin
package jp.co.people.sample

import com.squareup.moshi.Json

data class Media (
    val id: String,
    val type: String,
    val url: String,
    @Json(name = "preview_url") val previewUrl: String
    )
```

app/Toot.kt

```kotlin
package jp.co.people.sample

import com.squareup.moshi.Json


data class Toot (
    val id: String,
    @Json(name = "created_at") val createdAt: String,
    val sensitive: Boolean,
    val url: String,
    @Json(name = "media_attachments") val mediaAttachements: List<Media>,
    val content: String,
    val account: Account
) {
    val topMedia: Media?
        get() = mediaAttachements.firstOrNull()
}
```

get はカスタムGetter のようだ。
topMedia で値をプロパティのように取得出来る。

クラスのプロパティの一つ、バッキングフィールドを持たないで、ゲッターで取得する。
セッターもある。この場合はバッキングフィールドを持つ。it みたいに field という暗黙の名前があったりする。


## 画像表示ライブラリの追加

URL から画像を表示する便利なライブラリ glide をインストールする。

```java
dependencies {
    implementation 'com.github.bumptech.glide:glide:4.11.0'
    kapt 'com.github.bumptech.glide:compiler:4.11.0'
```

## BindingAdapter に追加

app/ DataBindingAdapter.kt

```kotlin
package jp.co.people.sample

import android.widget.ImageView
import android.widget.TextView
import androidx.core.text.HtmlCompat
import androidx.databinding.BindingAdapter
import com.bumptech.glide.Glide

@BindingAdapter("spannedContent")
fun TextView.setSpannedString(content: String) {
...

@BindingAdapter("media")
fun ImageView.setMedia(media: Media?) {
    if (media == null) {
        setImageDrawable(null)
        return
    }
    Glide.with(this)
        .load(media.url)
        .into(this)
}
```


## レイアウトの変更

res/layout/ list_item_toot.xml

```xml


        <androidx.appcompat.widget.AppCompatImageView
            android:id="@+id/image"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintTop_toBottomOf="@+id/content"
            app:media="@{toot.topMedia}"
            tools:src="@mipmap/ic_launcher" />

   </androidx.constraintlayout.widget.ConstraintLayout>
</layout>
```

toot.topMedia は Toot の中にある Getter で取得してくる。



次は　#android  表示内容をフィルタリングする]]