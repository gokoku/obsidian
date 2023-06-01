#android 

---
2021-10-19

# リストを下に引いて更新する

ここまでの話。

#android   無限スクロールにする]]


## SwipeRefreshLayout を追加

app/ build.gradle

```java
    implementation 'androidx.swiperefreshlayout:swiperefreshlayout:1.0.0'
```


## レイアウトの変更

res/layout/ fragment_toot_list.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<layout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools" >

    <androidx.constraintlayout.widget.ConstraintLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <androidx.swiperefreshlayout.widget.SwipeRefreshLayout
            android:id="@+id/swipe_refresh_layout"
            android:layout_width="match_parent"
            android:layout_height="match_parent">

            <androidx.recyclerview.widget.RecyclerView
                android:id="@+id/recycler_view"
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:scrollbars="vertical"
                tools:listitem="@layout/list_item_toot" />
            
        </androidx.swiperefreshlayout.widget.SwipeRefreshLayout>
    </androidx.constraintlayout.widget.ConstraintLayout>
</layout>
```


## コードの変更

app/ TootListFragment.kt

```kotlin
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
        coroutineScope.launch {
            isLoading.set(true)
            showProgress()

            val tootListResponse = api.fetchPublicTimeline(
                maxId = tootList.lastOrNull()?.id,
                onlyMedia = true
            )
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

次は　#android   実行中のコルーチンをキャンセルする]]