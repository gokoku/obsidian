#java 

![](image-kn9nkml2.png)

```bash
@Override
protected void onCreate(Bundle savedInstanceState) {
	〜省略〜
	BackgroundTask backgroundTask = new BackgroundTask();  // (1)
	ExecutorService executorService  = Executors.newSingleThreadExecutor();  // (2)
	executorService.submit(backgroundTask);  // (3)
}

private class BackgroundTask implements Runnable {  // (4)
	@Override
	public void run() {  // (5)
		Log.i("Async-BackgroundTask", "ここに非同期処理を記述する");  // (6)
	}
}
```

戻って来たあとの処理が ピュアJava では難しく、Android 独自実装があるらしい。

Handler 

```bash
private class BackgroundTask implements Runnable {
	private final Handler _handler;  // (1)
	public BackgroundTask(Handler handler) {  // (2)
		_handler = handler;  // (2)
	}
	@Override
	public void run() {
		Log.i("Async-BackgroundTask", "ここに非同期処理を記述する");
		PostExecutor postExecutor = new PostExecutor();
		_handler.post(postExecutor);  // (3)
	}
}

private class PostExecutor implements Runnable {  // (4)
	@Override
	public void run() {
		Log.i("Async-PostExecutor", "ここにUIスレッドで行いたい処理を記述する");  // (5)
	}
}
```

謎 Handler オブジェクト。スレッド間の通信オブジェクトにすぎないとのこと。

Looper

```bash
@Override
protected void onCreate(Bundle savedInstanceState) {
	〜省略〜
	Looper mainLooper = Looper.getMainLooper();  // (1)
	Handler handler = HandlerCompat.createAsync(mainLooper);  // (2)
	BackgroundTask backgroundTask = new BackgroundTask(handler);  // (3)
	ExecutorService executorService  = Executors.newSingleThreadExecutor();
	executorService.submit(backgroundTask);
}

private class BackgroundTask implements Runnable {
	private final Handler _handler;
	public BackgroundTask(Handler handler) {
		_handler = handler;
	}
	@Override
	public void run() {
		Log.i("Async-BackgroundTask", "ここに非同期処理を記述する");
		PostExecutor postExecutor = new PostExecutor();
		_handler.post(postExecutor);
	}
}

private class PostExecutor implements Runnable {
	@Override
	public void run() {
		Log.i("Async-PostExecutor", "ここにUIスレッドで行いたい処理を記述する");
	}
}
```

ここにおいて別スレッドに渡し、メインスレッドに戻って処理するループが完成するようだ。

Android のMainActivity.java

```bash
@Override
protected void onCreate(Bundle savedInstanceState) {
	〜省略〜
	asyncExecute();
}

@UiThread  // (1)
public void asyncExecute() {  // (2)
	Looper mainLooper = Looper.getMainLooper();
	Handler handler = HandlerCompat.createAsync(mainLooper);
	BackgroundTask backgroundTask = new BackgroundTask(handler);
	ExecutorService executorService  = Executors.newSingleThreadExecutor();
	executorService.submit(backgroundTask);
}

private class BackgroundTask implements Runnable {
	private final Handler _handler;
	public BackgroundTask(Handler handler) {
		_handler = handler;
	}
	@WorkerThread  // (3)
	@Override
	public void run() {
		Log.i("Async-BackgroundTask", "ここに非同期処理を記述する");
		PostExecutor postExecutor = new PostExecutor();
		_handler.post(postExecutor);
	}
}

private class PostExecutor implements Runnable {
	@UiThread  // (4)
	@Override
	public void run() {
		Log.i("Async-PostExecutor", "ここにUIスレッドで行いたい処理を記述する");
	}
}
```

@UiThread アノテーションでここがUIスレッドで実行されますよとコンパイラに確認してもらえる。
