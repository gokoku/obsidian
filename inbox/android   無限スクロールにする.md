#android

---
2021-10-18

# 無限スクロールにする

ここまで、

#android  表示内容をフィルタリングする]]

ここからでもいいか。

#android  Media 画像の表示]]


## API で定義する

Mastodon ドキュメントでは max_id 未満の id の Toot を取得するとのこと。

app/ MastodonApi.kt

```kotlin
package jp.co.people.sample

import retrofit2.http.GET
import retrofit2.http.Query

interface MastodonApi {
    @GET("api/v1/timelines/public")
    suspend fun fetchPublicTimeline(
        @Query("max_id") maxId: String? = null,
        @Query("only_media") onlyMedia: Boolean = false
    ): List<Toot>
}
```

## RecyclerView のスクロールイベントを受け取る

app/ TootListFragment.kt

```kotlin
package jp.co.people.sample

import android.os.Bundle
import android.view.View
import androidx.databinding.DataBindingUtil
import androidx.fragment.app.Fragment
import androidx.recyclerview.widget.LinearLayoutManager
import androidx.recyclerview.widget.RecyclerView
import com.squareup.moshi.Moshi
import com.squareup.moshi.kotlin.reflect.KotlinJsonAdapterFactory
import jp.co.people.sample.databinding.FragmentTootListBinding
import kotlinx.coroutines.CoroutineScope
import kotlinx.coroutines.Dispatchers
import kotlinx.coroutines.launch
import kotlinx.coroutines.withContext
import retrofit2.Retrofit
import retrofit2.converter.moshi.MoshiConverterFactory
import java.util.concurrent.atomic.AtomicBoolean

class TootListFragment : Fragment(R.layout.fragment_toot_list) {

    companion object {
        val TAG = TootListFragment::class.java.simpleName
        private const val API_BASE_URL = "https://androidbook2020.keiji.io"
    }

    private var binding: FragmentTootListBinding? = null
    private val moshi = Moshi.Builder()
        .add(KotlinJsonAdapterFactory())
        .build()
    private val retrofit = Retrofit.Builder()
        .baseUrl(API_BASE_URL)
        .addConverterFactory(MoshiConverterFactory.create(moshi))
        .build()
    private val api = retrofit.create(MastodonApi::class.java)

    private val coroutineScope = CoroutineScope(Dispatchers.IO)

    private lateinit var adapter: TootListAdapter
    private lateinit var layoutManager: LinearLayoutManager

    private var isLoading = AtomicBoolean()
    private var hasNext = AtomicBoolean().apply { set(true) }

    private val loadNextScrollListener = object : RecyclerView.OnScrollListener() {
        override fun onScrolled(recyclerView: RecyclerView, dx: Int, dy: Int) {
            super.onScrolled(recyclerView, dx, dy)
            if (isLoading.get() || !hasNext.get()) {
                return
            }
            val visibleItemCount = recyclerView.childCount
            val totalItemCount = layoutManager.itemCount
            val firstVisibleItemPosition = layoutManager.findFirstVisibleItemPosition()

            if ((totalItemCount - visibleItemCount) <= firstVisibleItemPosition) {
                loadNext()
            }
        }
    }

    private val tootList = ArrayList<Toot>()

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)

        adapter = TootListAdapter(layoutInflater, tootList)
        layoutManager = LinearLayoutManager(
            requireContext(),
            LinearLayoutManager.VERTICAL,
            false
        )
        val bindingData: FragmentTootListBinding? = DataBindingUtil.bind(view)

        binding = bindingData ?: return

        bindingData.recyclerView.also {
            it.layoutManager = layoutManager
            it.adapter = adapter
            it.addOnScrollListener(loadNextScrollListener)
        }
        loadNext()
    }

    // coroutinScope.launch が loadNext に移動した
    
    override fun onDestroyView() {
        super.onDestroyView()
        binding?.unbind()
    }

    private fun loadNext() {
        coroutineScope.launch {
            isLoading.set(true)

            val tootListResponse = api.fetchPublicTimeline(
                maxId = tootList.lastOrNull()?.id,
                onlyMedia = true
            )
            tootList.addAll(tootListResponse.filter { !it.sensitive })
            reloadTootList()

            isLoading.set(false)
            hasNext.set(tootListResponse.isNotEmpty())
        }
    }
    private suspend fun reloadTootList() = withContext(Dispatchers.Main) {
        adapter.notifyDataSetChanged()
    }
}
```

次は #android   リストを下に引いて更新する]]