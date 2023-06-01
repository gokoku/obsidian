#android

---
2021-10-18

# DataBinding

## DataBinding の有効化
app/build.gradle

```java
plugins {
    id 'com.android.application'
    id 'kotlin-android'
    id 'kotlin-kapt'
}

android {
   CompileSdk 29
	   
   dataBinding {
   	enabled true
   }
}
```

## レイアウト XML を DataBinding 対応にする

src/main/res/layout/activity_main.xml

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
		  
		  />
   </androidx.constraintlayout.widget.ConstraintLayout>
	
</layout>
```

layout で包む。

## コードの表示変更

src/main/java/jp.co.people.sample/MainActivity.kt

```kotlin

package jp.co.people.sample

import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import androidx.databinding.DataBindingUtil
import jp.co.people.sample.databinding.ActivityMainBinding

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        val binding: ActivityMainBinding = DataBindingUtil.setContentView(
            this,
            R.layout.activity_main
        )
        binding.textview.text = "Hello DataBinding!"
    }
}
```

