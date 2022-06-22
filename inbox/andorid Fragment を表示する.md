#android 

---
2021-10-18

# Fragment を表示する

## DataBinding の設定

#android  DataBinding の設定]]

## Fragment-KTX の追加

## Fragment レイアウトファイル作成

src/main/res/laout/fragment_main.xml

activity_main.xml の中身を丸ごとコビーする。

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

## Activity レイアウトの変更

src/main/res/layout/activity_main.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.fragment.app.FragmentContainerView
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/fragment_container"
    android:layout_width="match_parent"
    android:layout_height="match_parent" />

```

## Fragment クラスの作成

src/main/java/jp.co.people.sample/MainFragment.kt

```kotlin
package jp.co.people.sample

import android.os.Bundle
import android.view.View
import androidx.databinding.DataBindingUtil
import androidx.fragment.app.Fragment
import jp.co.people.sample.databinding.FragmentMainBinding

class MainFragment : Fragment(R.layout.fragment_main){
    private var binding: FragmentMainBinding? = null

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)

        binding = DataBindingUtil.bind(view)
        binding?.textview?.text = "Hello Fragment"
    }

    override fun onDestroyView() {
        super.onDestroyView()
        binding?.unbind()
    }
}
```


## MainActivity で Fragment を表示

src/main/java/jp.co.people.cample/MainActivity.kt

```kotlin
package jp.co.people.sample

import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        if (savedInstanceState == null) {
            val fragment = MainFragment()
            supportFragmentManager.beginTransaction()
                .add(
                    R.id.fragment_container,
                    fragment,
                    MainFragment::class.java.simpleName
                )
                .commit()
        }
    }
}
```

ここでは、 setContentView(R.layout.activity_main) が復活する。

