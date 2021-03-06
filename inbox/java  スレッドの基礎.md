#java

---
2021-11-05

# Javaのスレッド(Thread)を使いこなすコツを、基礎からしっかり伝授


https://www.bold.ne.jp/engineer-club/java-thread

スレッドクラスは Thread を継承して作る。

### シンプルに

Thread / ThreadSample.java

```java
package Thread;

public class ThreadSample extends Thread {
  public void run() {
    System.out.println("ThreadSample.run()");
  }
}
```

Thread / ThreadExecutor.java

```java
package Thread;

public class ThreadExecutor {
  public static void main(String[] args) {
    ThreadSample t = new ThreadSample();
    t.start();
  }
}
```

```shell
$ javac Thread/ThreadExecutor.java

$ java Thread.ThreadExecutor

ThreadSample.run()
```

### いくつも走らせる

Thread / ThreadSample.java

```java
package Thread;

public class ThreadSample extends Thread {
  public void run() {
    System.out.println("スレッド" + getName() + "で動いてマス");
  }
}

```

Thread / ThreadExecutor.java

```java
package Thread;

public class ThreadExecutor {
  public static void main(String[] args) {
    ThreadSample t1 = new ThreadSample();
    ThreadSample t2 = new ThreadSample();
    ThreadSample t3 = new ThreadSample();
    t1.start();
    t2.start();
    t3.start();
  }
}

```

```shell
$ javac Thread/ThreadExecutor.java
$ java Thread.ThreadExecutor
スレッドThread-2で動いてマス
スレッドThread-0で動いてマス
スレッドThread-1で動いてマス
```

### Runnable とは

スレッドで動かす処理を java.lang.Runnable 実装クラスで作る。

Runnable を Thread クラスのインスタンスで動かす。

Thread / RunnableSample.java

```java
package Thread;

class RunnableSample implements Runnable {
  public void run() {
    String threadName = Thread.currentThread().getName();
    System.out.println("スレッド" + threadName + "が実行されました。");
  }
}

```

Thread / ThreadExecutor.java

```java
package Thread;

public class ThreadExecutor {
  public static void main(String[] args) {
    RunnableSample r = new RunnableSample();
    Thread t = new Thread(r);
    t.start();
  }
}
```

```shell
$ javac Thread/ThreadExecutor.java
$ java Thread.ThreadExecutor

スレッドThread-0が実行されました。
```

### いくつものスレッドで Runnable を呼び出す

Thread / ThreadExecutor.java

```java
package Thread;

public class ThreadExecutor {
  public static void main(String[] args) {
    RunnableSample r = new RunnableSample();
    Thread t1 = new Thread(r);
    Thread t2 = new Thread(r);
    Thread t3 = new Thread(r);
    t1.start();
    t2.start();
    t3.start();
  }
}
```

```shell
$ javac Thread/ThreadExecutor.java
$ java Thread.ThreadExecutor

スレッドThread-2が実行されました。
スレッドThread-0が実行されました。
スレッドThread-1が実行されました。
```

![ The thread calls Runnable](https://www.bold.ne.jp/engineer-club/wp-content/uploads/2020/01/%E3%82%B9%E3%83%AC%E3%83%83%E3%83%89%E3%81%8CRunnable%E3%82%92%E5%91%BC%E3%81%B3%E5%87%BA%E3%81%99.png)

#### スレッドの識別子・名前を取得する

```java
package Thread;

public class ThreadExecutor {
  public static void main(String[] args) {
    Thread t1 = new Thread();
    Thread t2 = new Thread();

    long t1id = t1.getId();
    long t2id = t2.getId();
    String t1name = t1.getName();
    String t2name = t2.getName();

    System.out.println("t1 id: " + t1id + "、名前は" + t1name + "です");
    System.out.println("t2 id: " + t2id + "、名前は" + t2name + "です");
  }
}
```
```shell
❯ javac Thread/ThreadExecutor.java
❯ java Thread.ThreadExecutor
t1 id: 11、名前はThread-0です
t2 id: 12、名前はThread-1です
```

#### 今のスレッドの取得

Thread / RunnableSample.java

```java
package Thread;

class RunnableSample implements Runnable {
  public void run() {
    Thread t = Thread.currentThread();
    System.out.println("スレッドの識別子は" + t.getId() + "名前は" + t.getName() + "が実行されました。");
  }
}

```


Thread / ThreadExecutor.java

```java
package Thread;

public class ThreadExecutor {
  public static void main(String[] args) {
    RunnableSample runnableSample = new RunnableSample();
    Thread t1 = new Thread(runnableSample);
    Thread t2 = new Thread(runnableSample);

    t1.start();
    t2.start();
  }
}

```

```shell
❯ javac Thread/ThreadExecutor.java
❯ java Thread.ThreadExecutor
スレッドの識別子は11名前はThread-0が実行されました。
スレッドの識別子は12名前はThread-1が実行されました。
```

####  今のスレッドを一時停止する

Thread / ThreadSample.java

```java
package Thread;

public class ThreadSample extends Thread {
  public void run() {
    System.out.println("sleep を始めます");
    System.out.println("スレッドの識別子は" + getId() + "名前は" + getName() + "で動いてマス");
    try {
      Thread.sleep(1000);
    } catch (InterruptedException e) {
      System.out.println("sleep を中断しました");
    }
    System.out.println("sleep が終わりました");
  }
}

```

Thread / ThreadExecutor.java

```java
package Thread;

public class ThreadExecutor {
  public static void main(String[] args) {
    ThreadSample threadSample = new ThreadSample();
    threadSample.start();
  }
}
```

sleep 中に interrupt が発生すると中断するらしい。

#### スレッドの処理が終わるのを待つ Thread.join

Thread / ThreadSample.java

```java
package Thread;

public class ThreadSample extends Thread {
  public void run() {
    System.out.println("sleep を始めます");
    try {
      Thread.sleep(1000);
    } catch (InterruptedException e) {
      System.out.println("sleep を中断しました");
    }
    System.out.println("sleep が終わりました");
  }
}
```

Thread / ThreadExecutor.java

```java
package Thread;

public class ThreadExecutor {
  public static void main(String[] args) {
    Thread t = new ThreadSample();
    t.start();

    System.out.println("Join を始めます");

    try {
      t.join(); // 実行中のスレッドを待つ。ここでブロックする。
    } catch (InterruptedException e) {
      e.printStackTrace();
    }
    System.out.println("Join を終了しました");
  }
}
```

```shell
❯ javac Thread/ThreadExecutor.java
❯ java Thread.ThreadExecutor
Join を始めます
sleep を始めます
sleep が終わりました
Join を終了しました
```

![スレッドの終了をjoinで待つ](https://www.bold.ne.jp/engineer-club/wp-content/uploads/2020/01/%E3%82%B9%E3%83%AC%E3%83%83%E3%83%89%E3%81%AE%E7%B5%82%E4%BA%86%E3%82%92join%E3%81%A7%E5%BE%85%E3%81%A4.png)

#### スレッドに割り込み Thread.interrupt

Thread / ThreadSample.java

```java
package Thread;

public class ThreadSample extends Thread {
  public void run() {
    System.out.println("sleep を始めます");
    try {
      Thread.sleep(10000L);
    } catch (InterruptedException e) {
      System.out.println("sleep を中断しました(割り込み入った)");
    }
    System.out.println("sleep が終わりました");
  }
}
```

Thread / ThreadExecutor.java

```java
package Thread;

public class ThreadExecutor {
  public static void main(String[] args) {
    Thread t = new ThreadSample();
    t.start();

    try {
      Thread.sleep(1000);
    } catch (InterruptedException e) {
      e.printStackTrace();
    }
    System.out.println("スレッドに割込み入れます");
    t.interrupt();
    System.out.println("スレッドに割込み入れました");
  }
}
```

```shell
❯ javac Thread/ThreadExecutor.java
❯ java Thread.ThreadExecutor
sleep を始めます
スレッドに割込み入れます
スレッドに割込み入れました
sleep を中断しました(割り込み入った)
sleep が終わりました
```

割込みは処理を中断するためなのだろうか。

しかも、try catche じゃないと即時中断はしないらしい。

![スレッドへinterruptで割り込む](https://www.bold.ne.jp/engineer-club/wp-content/uploads/2020/01/%E3%82%B9%E3%83%AC%E3%83%83%E3%83%89%E3%81%B8interrupt%E3%81%A7%E5%89%B2%E3%82%8A%E8%BE%BC%E3%82%80.png)


#### スレッド内の例外

Thread / ThreadSample.java

```java
package Thread;

public class ThreadSample extends Thread {
  public void run() {
    if(true) {
      throw new RuntimeException("スレッド" + getName() + "で例外が発生しました。");
    }
  }
}
```

Thread / ThreadExecutor.java

```java
package Thread;

public class ThreadExecutor {
  public static void main(String[] args) {
    System.out.println("スレッド実行始め");

    try {
      Thread t = new ThreadSample();
      t.start();
    } catch (Exception e) {
      System.out.println("スレッド実行エラー");
      e.printStackTrace();
    }
    System.out.println("スレッド実行終了");
  }
}
```

```shell
❯ javac Thread/ThreadExecutor.java
❯ java Thread.ThreadExecutor
スレッド実行始め
スレッド実行終了
Exception in thread "Thread-0" java.lang.RuntimeException: スレッドThread-0で例外が発生しました。
	at Thread.ThreadSample.run(ThreadSample.java:6)
```

スレッドからの例外は、main の catch には入らないとのこと。


####  Thread.UncaughtExceptionHandlerで例外の発生を知る

handler を使って捕まえられる。

```java
package Thread;

public class ThreadExecutor {
  public static void main(String[] args) {
    System.out.println("スレッド実行始め");

    Thread.UncaughtExceptionHandler handler = new Thread.UncaughtExceptionHandler() {
      public void uncaughtException(Thread t, Throwable e) {
        System.out.println("例外発生：" + e.getMessage());
      }
    };

    Thread t = new ThreadSample();
    t.setUncaughtExceptionHandler(handler);
    t.start();

    System.out.println("スレッド実行終了");
  }
}
```

```shell
❯ javac Thread/ThreadExecutor.java
❯ java Thread.ThreadExecutor
スレッド実行始め
スレッド実行終了
例外発生：スレッドThread-0で例外が発生しました。
```

捕まえた。

## スレッド関連のトピック

スレッドの処理結果を使いたい。

* ポーリング : Thread/Runnable に処理が終わったか問い合わせ、終わったら結果をもらう。
* コールバック : Thread/Runnable に処理が終わったら呼び出すメソッドを呼んでもらう。
* データ共有オブジェクト


### ポーリング

polling とは、定期的に聞きに行く処理。

Thread / ThreadSample.java

```java
package Thread;

public class ThreadSample extends Thread {
  private int result;
  private boolean finished;

  public void run() {
    for(int i = 0; i <= 10; i++) {
      result += i;
    }
    finished = true;
  }
  public int getResult() {
    return result;
  }
  public boolean isFinished() {
    return finished;
  }
}
```

Thread / ThreadExecutor.java

```java
package Thread;

public class ThreadExecutor {
  public static void main(String[] args) {
    ThreadSample t = new ThreadSample();
    t.start();

    while(true) {
      try {
        Thread.sleep(1000L);
      }catch(InterruptedException e) {
        e.printStackTrace();
      }

      if(t.isFinished()) {
        System.out.println("Thread finished");
        break;
      }
    }
    int result = t.getResult();
    System.out.println("Result: " + result);
  }
}
```

```shell
❯ javac Thread/ThreadExecutor.java
❯ java Thread.ThreadExecutor
Thread finished
Result: 55
```

これは簡単な方法だ。

sleep のところで他の処理かましたりするんだって。

### コールバック

お馴染み、コールバック。

Thread / ThreadSample.java

```java
package Thread;

public class ThreadSample extends Thread {

  private Callback callback;

  public ThreadSample(Callback callback) {
    this.callback = callback;
  }

  public void run() {
    int result = 0;
    for(int i = 0; i <= 10; i++) {
      result += i;
    }
    callback.finished(result);
  }
  static interface Callback {
    void finished(int result);
  }
}
```

Thread / ThreadExecutor.java

```java
package Thread;

public class ThreadExecutor {
  public static void main(String[] args) {
    ThreadSample.Callback callback = new ThreadSample.Callback() {
      public void finished(int result) {
        System.out.println("スレッドでの処理結果は" + result + "です。");
      }
    };
    ThreadSample threadSample = new ThreadSample(callback);
    threadSample.start();
  }
}
```

```shell
❯ javac Thread/ThreadExecutor.java
❯ java Thread.ThreadExecutor
スレッドでの処理結果は55です。
```

### データ共有オブジェクト

Thread / SharedData.java

```java
package Thread;

public class SharedData {
  private Integer result;

  Integer getResult() {
    return result;
  }

  void setResult(Integer result) {
    this.result = result;
  }
}
```

Thread / ThreadSample.java

```java
package Thread;

public class ThreadSample extends Thread {

  private SharedData sharedData;

  public ThreadSample(SharedData sharedData) {
    this.sharedData = sharedData;
  }
  public void run() {
    int result = 0;
    for(int i = 0; i <= 10; i++) {
      result += i;
    }
    sharedData.setResult(result);
  }
}
```

Thread / ThreadExecutor.java

```java
package Thread;

public class ThreadExecutor {
  public static void main(String[] args) {
    SharedData sharedData = new SharedData();
    ThreadSample t = new ThreadSample(sharedData);
    t.start();

    while(true) {
      try {
        Thread.sleep(1000);
      } catch (InterruptedException e) {
        e.printStackTrace();
      }
      if(sharedData.getResult() != null) {
        System.out.println("Thread is finished");
        break;
      }
    }
    System.out.println("結果は" + sharedData.getResult() + "です");
  }
}
```

```shell
❯ javac Thread/ThreadExecutor.java
❯ java Thread.ThreadExecutor
Thread is finished
結果は55です
```

これは排他ロックがないとヤバいやつ。

### main スレッド

main で動いてるのは、main スレッドで動いてるということ。

Thread / ThreadExecutor.java

```java
package Thread;

public class ThreadExecutor {
  public static void main(String[] args) {
    String threadName = Thread.currentThread().getName();

    System.out.println("スレッド名は" + threadName + "です");
  }
}
```

```shell
❯ javac Thread/ThreadExecutor.java
❯ java Thread.ThreadExecutor
スレッド名はmainです
```


## Executor フレームワーク

Thread / RunnableSample.java

```java
package Thread;

class RunnableSample implements Runnable {
  private int result;

  public void run() {
    for(int i=0; i<=10; i++) {
      result += i;
    }
  }

  int getResult() {
    return result;
  }
}

```

Thread / ExecutorServiceTest.java

```java
package Thread;

import java.util.concurrent.Executors;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Future;

public class ExecutorServiceTest {
  public static void main(String[] args) {
    RunnableSample r = new RunnableSample();
    ExecutorService executor = Executors.newSingleThreadExecutor();
    Future future = executor.submit(r);
    try {
      future.get();
    } catch (Exception e) {
      e.printStackTrace();
    }
    executor.shutdown();
    System.out.println("結果は" + r.getResult() + "です");
  }
}
```

```shell
❯ javac Thread/ExecutorServiceTest.java
❯ java Thread.ExecutorServiceTest
結果は55です
```


## Executorフレームワーク(Callable)

Callable を使うと、スレッドの実行結果を外から直に掴めるとのこと。

Thread / CallableSample.java

```java
package Thread;

import java.util.concurrent.Callable;

public class CallableSample implements Callable<Integer> {

    public Integer call() throws Exception {
      int result = 0;
      for(int i=0; i<=10; i++){
        result += i;
      }
      return result;
    }
}

```

 Thread / ExecutorServiceTest.java
 
```java

package Thread;

import java.util.concurrent.Executors;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Future;

public class ExecutorServiceTest {
  public static void main(String[] args) {
    CallableSample c = new CallableSample();
    ExecutorService executor = Executors.newSingleThreadExecutor();
    Future<Integer> future = executor.submit(c);

    try{
      System.out.println("スレッドの処理結果は" + future.get() + "です");
    } catch (Exception e) {
      e.printStackTrace();
      System.out.println("スレッドで例外が発生しました");
      e.getCause().printStackTrace();
    }
    executor.shutdown();
  }
}
```

```shell
❯ javac Thread/ExecutorServiceTest.java
❯ java Thread.ExecutorServiceTest
スレッドの処理結果は55です
```

スレッドを動かす親に例外 throw を投げられるとのこと。

```java
package Thread;

import java.util.concurrent.Callable;

public class CallableSample implements Callable<Integer> {

    public Integer call() throws Exception {
      int result = 0;
      for(int i=0; i<=10; i++){
        result += i;
      }
      if(true) {
        throw new RuntimeException("スレッド" + Thread.currentThread().getName() + "で例外が発生しました。");
      }

      return result;
    }
}

```

```shell
❯ java Thread.ExecutorServiceTest
java.util.concurrent.ExecutionException: java.lang.RuntimeException: スレッドpool-1-thread-1で例外が発生しました。
	at java.base/java.util.concurrent.FutureTask.report(FutureTask.java:122)
	at java.base/java.util.concurrent.FutureTask.get(FutureTask.java:191)
	at Thread.ExecutorServiceTest.main(ExecutorServiceTest.java:14)
Caused by: java.lang.RuntimeException: スレッドpool-1-thread-1で例外が発生しました。
	at Thread.CallableSample.call(CallableSample.java:13)
	at Thread.CallableSample.call(CallableSample.java:5)
	at java.base/java.util.concurrent.FutureTask.run(FutureTask.java:264)
	at java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1128)
	at java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:628)
	at java.base/java.lang.Thread.run(Thread.java:829)
スレッドで例外が発生しました
java.lang.RuntimeException: スレッドpool-1-thread-1で例外が発生しました。
	at Thread.CallableSample.call(CallableSample.java:13)
	at Thread.CallableSample.call(CallableSample.java:5)
	at java.base/java.util.concurrent.FutureTask.run(FutureTask.java:264)
	at java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1128)
	at java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:628)
	at java.base/java.lang.Thread.run(Thread.java:829)
```

こういうことかな。
