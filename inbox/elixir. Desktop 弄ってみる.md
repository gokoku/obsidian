---
type: note
---

#elixir

---
2022-06-13  08:50

# elixir. Desktop


<div class="rich-link-card-container"><a class="rich-link-card" href="https://github.com/elixir-desktop/desktop" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://opengraph.githubassets.com/aa77192d7093a495786b3cbcc3a342ffe87440a85b349c1d1533b3326ec68ce0/elixir-desktop/desktop')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">GitHub - elixir-desktop/desktop: Elixir library to write Windows, macOS, Linux, Android apps with OTP24 & Phoenix.LiveView</h1>
		<p class="rich-link-card-description">
		Elixir library to write Windows, macOS, Linux, Android apps with OTP24 &amp;amp; Phoenix.LiveView - GitHub - elixir-desktop/desktop: Elixir library to write Windows, macOS, Linux, Android apps with...
		</p>
		<p class="rich-link-href">
		https://github.com/elixir-desktop/desktop
		</p>
	</div>
</a></div>


デモを動かす。


<div class="rich-link-card-container"><a class="rich-link-card" href="https://github.com/elixir-desktop/desktop-example-app" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://opengraph.githubassets.com/dd25b30320f6c7ef815f3c18df02ce0c717c0334df98274e5cebb740eff4377c/elixir-desktop/desktop-example-app')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">GitHub - elixir-desktop/desktop-example-app: Elixir Sample App using the Desktop library with LiveView to create a desktop app</h1>
		<p class="rich-link-card-description">
		Elixir Sample App using the Desktop library with LiveView to create a desktop app - GitHub - elixir-desktop/desktop-example-app: Elixir Sample App using the Desktop library with LiveView to create ...
		</p>
		<p class="rich-link-href">
		https://github.com/elixir-desktop/desktop-example-app
		</p>
	</div>
</a></div>


```shell
$ gh repo clone elixir-desktop/desktop-example-app
$ cd desktop-example-app

$ mix deps.get
$ ./run

```

nprogress.css がないと言われる。
node で入れてみる。

```shell
$ npm i nprogress

./run
```

![[Pasted image 20220613101632.png]]

上手くいった。
