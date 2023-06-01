#java

---
2021-11-05

# Java: Threadおさらい

https://qiita.com/e99h2121/items/8eb3e031529083901485

```shell
$ mkdir playground
```

playground/ThreadUser.java

```java
package playground;

public class ThreadUser {

    public static void main(String[] args) {
        SimpleThread t = new SimpleThread();
        t.start();

        SimpleThread2 t2 = new SimpleThread2();
        Thread t2c = new Thread(t2); 
        t2c.start();

        System.out.println("joinを始めます");

        try {
            t.join(); // スレッドでの処理が終わるまで、ここでブロックされる
            t2c.join();
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }

        System.out.println("joinが終わりました");
    }
}
```

playground/SimpleThread.java

```java
package playground;

public class SimpleThread extends Thread {
    public void run() {
        for (int i = 0; i < 10; i++) {
            try {
                Thread.sleep(10000L); // 10秒停止する
                System.out.println("スレッドで動いています:" + (i+1) +"回目");
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }
    }
}
```


playground/SimpleThread2.java

```java
package playground;

public class SimpleThread2 implements Runnable {
    public void run() {

      Thread t = Thread.currentThread();
      long id = t.getId();
      String name = t.getName();
      System.out.println("スレッドの識別子は" + id + "、名前は" + name + "です。");
      for(int i=0; i<10; i++) {
        try {
          Thread.sleep(10000L);
          System.out.println("スレッド2で動いています :" + (i+1) + "回目");
        } catch(InterruptedException e) {
          System.out.println("Interrupted");
          Thread.currentThread().interrupt();
        }
    }
  }
}

```

#### コンパイル & 実行

```shell
$ javac playground/ThreadUser.java


$ java playground.ThreadUser
```

ThreadUser には main が入っているので動く。

実行には、パッケージで指定できるのか。バッケージからパスを解析するのだな。へー

