#kotlin #android 

---
2021-10-13

# クラス名を文字列で取得するためにこうするようだ


android で MyClass::class.java.simpleName と何度も見かけて?となった。

```kotlin
 val TAG = ToolListFragment::class.java.simpleName
```

これはクラス名を文字列として取得するものだとさ。

https://maku77.github.io/kotlin/misc/class-name.html


