#java

---
2021-11-05

# パッケージについての基礎

### java ソースをパッケージと同じディレクトリに置く


./Main.java

```java
import test.Sub

public class Main {
  public static void main(String[] args) {
    Sub.hello()
  }
}
```

./test/Sub.java

```java
package test

public class Sub {
  public static void hello() {
    System.out.println("Hello from Sub.")
  }
}
```

```
.
├── Main.java
└── test
    └── Sub.java

```

### コンパイル

```shell
$ javac Main.java
```

何も指定しないと、javaソースと同じ場所に class がコンパイルされる。

```
.
├── Main.class
├── Main.java
└── test
    ├── Sub.class
    └── Sub.java

```

### 実行

```shell
$ java Main

Hello from Sub
```

## まとめ

class ファイルとディレクトリの相対的位置が一緒であれば、アクセスできる。

import を使えばパッケージパスは省略できる。

Android Studio ってなんて難しいんだ?
