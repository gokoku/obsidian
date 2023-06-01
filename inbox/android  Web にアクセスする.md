#android

---
2021-10-18

# Web にアクセスする

## ここまでの準備

#android  DataBinding の設定]]

#andorid Fragment を表示する]]


## パーミッションの設定

src/main/AndroidManifest.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="jp.co.people.sample">

    <uses-permission android:name="android.permission.INTERNET" />
    
```

## Retrofit を設定

app/src/main/AdroidManifest.xml

```java
dependencies {
    ...
    implementation 'com.squareup.retrofit2:retrofit:2.7.1'
	}
```

## コルーチンで非同期処理をするようにする

メインスレッドでネットワークにアクセス出来ないようになってるので、コルーチンでするように準備する

app/build.gradle

```java
dependencies {
    ...
    implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-android:1.3.3'
```

## API suspend 関数の設置

app/MastodonApi.kt

```kotlin
package jp.co.people.sample

import okhttp3.ResponseBody
import retrofit2.http.GET

interface MastodonApi {
    @GET("api/v1/timelines/public")
    suspend fun fetchPublicTimeline(): ResponseBody
}
```

## コルーチンの起動

app/MainFragment.kt

```kotlin
package jp.co.people.sample

import android.os.Bundle
import android.view.View
import androidx.databinding.DataBindingUtil
import androidx.fragment.app.Fragment
import jp.co.people.sample.databinding.FragmentMainBinding
import retrofit2.Retrofit
import android.util.Log
import kotlinx.coroutines.CoroutineScope
import kotlinx.coroutines.Dispatchers
import kotlinx.coroutines.launch
import kotlinx.coroutines.withContext

class MainFragment : Fragment(R.layout.fragment_main){

    companion object {
        private val TAG = MainFragment::class.java.simpleName
        private const val API_BASE_URL = "https://androidbook2020.keiji.io"
    }

    private val retrofit = Retrofit.Builder()
        .baseUrl(API_BASE_URL)
        .build()
    private val api = retrofit.create(MastodonApi::class.java)
    private var binding: FragmentMainBinding? = null

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)

        binding = DataBindingUtil.bind(view)
        binding?.button?.setOnClickListener() {
            binding?.button?.text = "clicked"
            CoroutineScope(Dispatchers.IO).launch {
                val response = api.fetchPublicTimeline().string()
                Log.d(TAG, response)
                withContext(Dispatchers.Main) {
                    binding?.textview?.text = response
                }
            }
        }
    }

    override fun onDestroyView() {
        super.onDestroyView()
        binding?.unbind()
    }
}
```

## レイアウト xml

res/layout/fragment_main.xml

```xml
<?xml version="1.0" encoding="utf-8"?>

<layout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools" >

    <androidx.constraintlayout.widget.ConstraintLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:context=".MainActivity">

        <TextView
            android:id="@+id/textview"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            app:layout_constraintBottom_toBottomOf="parent"
            app:layout_constraintLeft_toLeftOf="parent"
            app:layout_constraintRight_toRightOf="parent"
            app:layout_constraintTop_toTopOf="parent" />

        <Button
            android:id="@+id/button"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Hello World!"
            app:layout_constraintBottom_toBottomOf="parent"
            app:layout_constraintLeft_toLeftOf="parent"
            app:layout_constraintRight_toRightOf="parent"
            app:layout_constraintTop_toTopOf="parent" />

    </androidx.constraintlayout.widget.ConstraintLayout>

</layout>
```

## JSON ライブラリの追加

app/build.gradle

```java
dependencies {
    ...
	    
    implementation 'com.squareup.moshi:moshi-kotlin:1.9.2'
    implementation 'com.squareup.retrofit2:converter-moshi:2.7.0'
}
```

## データクラスの作成

なんと、moshi ライブラリは作成するデータクラスのプロパティと JSON Key名を同じくするだけで、
データをパースして入れてくれるという。

app/Account.kt

```kotlin
package jp.co.people.sample

import com.squareup.moshi.Json

data class Account (
    val id: String,
    val username: String,
    @Json(name = "display_name") val displayName: String,
    val url: String
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
    val content: String,
    val account: Account
)
```


## API の調整
app/MastodonApi.kt

```kotlin
package jp.co.people.sample

import retrofit2.http.GET

interface MastodonApi {
    @GET("api/v1/timelines/public")
    suspend fun fetchPublicTimeline(): List<Toot>
}
```

データをToot のリストとして受け取るようにする。


## JSON 表示

app/MainFragment.kt

```kotlin
package jp.co.people.sample

import android.os.Bundle
import android.view.View
import androidx.databinding.DataBindingUtil
import androidx.fragment.app.Fragment
import com.squareup.moshi.Moshi
import com.squareup.moshi.kotlin.reflect.KotlinJsonAdapterFactory
import jp.co.people.sample.databinding.FragmentMainBinding
import retrofit2.Retrofit
import android.util.Log
import kotlinx.coroutines.CoroutineScope
import kotlinx.coroutines.Dispatchers
import kotlinx.coroutines.launch
import kotlinx.coroutines.withContext
import retrofit2.converter.moshi.MoshiConverterFactory

class MainFragment : Fragment(R.layout.fragment_main){

    companion object {
        private val TAG = MainFragment::class.java.simpleName
        private const val API_BASE_URL = "https://androidbook2020.keiji.io"
    }

    private val moshi = Moshi.Builder()
        .add(KotlinJsonAdapterFactory())
        .build()
    private val retrofit = Retrofit.Builder()
        .baseUrl(API_BASE_URL)
        .addConverterFactory(MoshiConverterFactory.create(moshi))
        .build()

    private val api = retrofit.create(MastodonApi::class.java)
    private var binding: FragmentMainBinding? = null

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)

        binding = DataBindingUtil.bind(view)
        binding?.button?.setOnClickListener() {
            binding?.button?.text = "clicked"
            CoroutineScope(Dispatchers.IO).launch {
                val tootList = api.fetchPublicTimeline()
                Log.d(TAG, tootList.map(){it.account.username}.joinToString ("\n"))
                showTootList(tootList)
            }
        }
    }

    private suspend fun showTootList( tootList: List<Toot> ) =
        withContext(Dispatchers.Main) {
            val binding = binding ?: return@withContext
            val accountNameList = tootList.map { it.account.displayName }
            binding.button.text = accountNameList.joinToString("\n")
        }


    override fun onDestroyView() {
        super.onDestroyView()
        binding?.unbind()
    }

}
```
displayName はカラなので、ボタンが伸びるだけだけど。

続き

#android リスト形式でJSONを表示する]]