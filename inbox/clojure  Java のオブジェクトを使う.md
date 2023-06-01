---
type: note
---

#clojure 

---
2023-05-02  13:04

# clojure  Java のオブジェクトを使う

```
$ lein repl
> (def rnd (new java.util.Random))
> (. rnd nextInt)
```

何がどうなってるのか?
そもそも、java の Random の使い方を知らない。

### そもそものjava入門
クラス名と同じファイル名をつける。

HelloWorld.java
```java
public class HelloWorld {
  public static void main(String[] args) {
    System.out.println("Hello World!");
  }
}
```

```shell
$ javac HelloWorld.java
$ java HelloWorld
```

### java で random オブジェクトを使う

```java
import java.util.Random;

public class RandomSample {
  Random rnd = new Random();
  int randomNumber = rnd.nextInt(bound:10);
  System.out.println(randomNumber)
}
```

```shell
$ javac RandomSample.java
$ java RandomSample
8
```

### clojure で random オブジェクトを使うagain

```shell
$ lein repl
> (def rnd (new java.util.Random))
> (. rnd nextInt 10)
2
```
ドットはメソッドの呼び出し。
因みに nextInt は nextInt() と引数を取る nextInt() がどちらも定義されているらしい。


なんじゃいこりゃ!!
Object を new してメソッド使うだけかい!!

```
> (. Math PI)
```

これが何故普通に使えるのかは、Clojure は自動的に java.lang を import してるとのこと。

java.util.Random とやる代わりにこうする。
```clojure
(import '(java.util Random Locale)
		'(java.text MessageFormat))

```

# URI オブジェクトを使う

## java

URIExample.java
```java
import java.net.URI;
import java.net.URISyntaxException;

public class URIExample {
  public static void main(String[] args) {
    URI url;
    try {
      url = new URI(str: "https://www.example.com/");
      System.out.println("Scheme: " + url.getScheme());
      System.out.println("Host: " + url.getHost());
      System.out.println("Path: " + url.getPath());
    } catch (URISyntaxException e) {
	  e.printStackTrace();
    }
  }
}
```

```shell
$ javac URIExample.java
$ java URIExample
Scheme: https
Host: www.example.com
Path: /
```

## clojure
```clojure
(import '(java.net URI))

(def url (new URI "https://www.example.com/"))
(. url getScheme)
"https"
(. url getHost)
"www.example.com"
(. url getPath)
"/"
```

なんじゃいこりゃ！

# ってゆーかChatGPTに聞きながらやったらものすごくコミットささる

こんな時は? この場合は? 他では? とか聞くとググればめんどくさい事もサラッとまとめて教えてくれるので、学びがどんどん入り込んでコミット力半端ないことになることがわかった!!!!!!!!!!!!!


