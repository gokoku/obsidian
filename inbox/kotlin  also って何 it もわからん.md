#kotlin

---
2021-10-14

# also

https://hirauchi-genta.com/kotlin-also/


```kotlin
class User {
  var name: String = ""
  var age: Int = 0
}

val result = User()?.alse {
  it.name = "Tada"
  it.age = 28
}

println("I am ${result.name}, ${result.age} です。")

```

it はラムダの中で、レシーバを表す。 this みたいな感じか??

```kotlin
val result = User()?.alse { user ->
  user.name = "Tada"
  user.age = 28
}
```

こうも書けるとのこと。

