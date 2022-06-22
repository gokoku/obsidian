#android 

---
2021-10-19

# 実行中のコルーチンをキャンセルする

非同期中にバックキーとかでアプリを少量しても継続するんだって。

だから、止めるようにする必要があるらしい。


ここまでの話。

#android   リストを下に引いて更新する]]

## コードの変更

app/ TootListFragment.kt

```kotlin
    override fun onDestroy() {
        super.onDestroy()
        coroutineScope.cancel()
    }
```

これは lifecycleScope を利用することで必要なくなるとのこと。

## lifecycleScope を利用する

corouchinScope の代わりに lifecycleScope を使う。

app/ TootListFragment.kt

```kotlin
package jp.co.people.sample

import android.os.Bundle
import android.view.View
import androidx.lifecycle.lifecycleScope
import androidx.databinding.DataBindingUtil
import androidx.fragment.app.Fragment
import androidx.recyclerview.widget.LinearLayoutManager
import androidx.recyclerview.widget.RecyclerView
import com.squareup.moshi.Moshi
import com.squareup.moshi.kotlin.reflect.KotlinJsonAdapterFactory
import jp.co.people.sample.databinding.FragmentTootListBinding
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
            it.addOnScrollListener((loadNextScrollListener))
        }

        bindingData.swipeRefreshLayout.setOnRefreshListener {
            tootList.clear()
            loadNext()
        }

        loadNext()
    }

    override fun onDestroyView() {
        super.onDestroyView()
        binding?.unbind()
    }

    private fun loadNext() {
        lifecycleScope.launch {
            isLoading.set(true)
            showProgress()

            val tootListResponse = withContext(Dispatchers.IO) {
                api.fetchPublicTimeline(
                    maxId = tootList.lastOrNull()?.id,
                    onlyMedia = true
                )
            }
            tootList.addAll(tootListResponse.filter { !it.sensitive })

            reloadTootList()

            isLoading.set(false)
            hasNext.set(tootListResponse.isNotEmpty())
            dismissProgress()
        }
    }
    private suspend fun reloadTootList() = withContext(Dispatchers.Main) {
        adapter.notifyDataSetChanged()
    }

    private suspend fun showProgress() = withContext(Dispatchers.Main) {
        binding?.swipeRefreshLayout?.isRefreshing = true
    }

    private suspend fun dismissProgress() = withContext(Dispatchers.Main) {
        binding?.swipeRefreshLayout?.isRefreshing = false
    }
}
```

