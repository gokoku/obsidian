#android

---
2021-10-18

# 表示内容フィルタリングする

ここまで

#android  Media 画像の表示]]

## API でフィルタリングする

api をクエリ付きで読み込むようにするようだ。

app/ MastodonApi.kt

```kotlin
package jp.co.people.sample

import retrofit2.http.GET
import retrofit2.http.Query

interface MastodonApi {
    @GET("api/v1/timelines/public")
    suspend fun fetchPublicTimeline(
        @Query("only_media") onlyMedia: Boolean = false
    ): List<Toot>
}
```

クエリonly_media として送るパラメータ定義。

app/ TootListFragment.kt

```kotlin
        coroutineScope.launch {
            val tootListResponse = api.fetchPublicTimeline(onlyMedia = true)
            tootList.addAll(tootListResponse)
            reloadTootList()
        }
```

画像添付投稿のみが表示になった。


## コードでフィルタリングする

app/TootListFragment.kt

```kotlin
        coroutineScope.launch {
            val tootListResponse = api.fetchPublicTimeline(onlyMedia = true)
            tootList.addAll(tootListResponse.filter { !it.sensitive })
            reloadTootList()
        }
```

次は　#android   無限スクロールにする]]