---
type: note
---

#ruby/rails 

---
2023-01-19  11:16

# rails  SSLを許さないPuma を通すには

mkcert というものがある。
ローカルでこれを使って puma を起動すると https://localhost:10443 で行くらしい。


<div class="rich-link-card-container"><a class="rich-link-card" href="https://github.com/FiloSottile/mkcert" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://repository-images.githubusercontent.com/138547797/1779e880-6164-11e9-971a-09791e669578')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">GitHub - FiloSottile/mkcert: A simple zero-config tool to make locally trusted development certificates with any names you'd like.</h1>
		<p class="rich-link-card-description">
		A simple zero-config tool to make locally trusted development certificates with any names you&amp;#39;d like. - GitHub - FiloSottile/mkcert: A simple zero-config tool to make locally trusted develo...
		</p>
		<p class="rich-link-href">
		https://github.com/FiloSottile/mkcert
		</p>
	</div>
</a></div>



```shell
$ brew install mkcert


$ mkcert -install
$ mkcert example.com

カレントディレクトリに
example.com-key.pem
example.com.pem
が作られた。
```

pumaの設定をしたが、elixir でUnkown CA で弾かれるので、やめた。



<div class="rich-link-card-container"><a class="rich-link-card" href="https://bokunonikki.net/post/2019/0422_rails_http_paese_error_malformed_request/" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://bokunonikki.net/images/favicon.ico')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Rails,HTTP parse error, malformed requestの対処 - bokunonikki.net</h1>
		<p class="rich-link-card-description">
		
		</p>
		<p class="rich-link-href">
		https://bokunonikki.net/post/2019/0422_rails_http_paese_error_malformed_request/
		</p>
	</div>
</a></div>



