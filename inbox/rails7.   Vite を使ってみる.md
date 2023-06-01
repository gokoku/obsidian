---
type: note
---

#ruby/rails #js/vite 

---
2022-05-31  10:39

# rails7. Vite を使ってみる

<div class="rich-link-card-container"><a class="rich-link-card" href="https://zenn.dev/sesere/articles/23a8ef2c54d489" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://res.cloudinary.com/zenn/image/upload/s--Ga7thB_m--/co_rgb:222%2Cg_south_west%2Cl_text:notosansjp-medium.otf_37_bold:sesere%2Cx_203%2Cy_98/c_fit%2Cco_rgb:222%2Cg_north_west%2Cl_text:notosansjp-medium.otf_70_bold:Rails%25E3%2581%25ABvite%25E3%2582%2592%25E5%2585%25A5%25E3%2582%258C%25E3%2581%25A6view%25E3%2582%2592live%2520reload%25E3%2581%2599%25E3%2582%258B%25E6%2589%258B%25E9%25A0%2586%2Cw_1010%2Cx_90%2Cy_100/g_south_west%2Ch_90%2Cl_fetch:aHR0cHM6Ly9yZXMuY2xvdWRpbmFyeS5jb20vemVubi9pbWFnZS9mZXRjaC9zLS16SEJtWTY2Sy0tL2NfbGltaXQlMkNmX2F1dG8lMkNmbF9wcm9ncmVzc2l2ZSUyQ3FfYXV0byUyQ3dfNzAvaHR0cHM6Ly9zdG9yYWdlLmdvb2dsZWFwaXMuY29tL3plbm4tdXNlci11cGxvYWQvYXZhdGFyLzg4NjY3ZDcwMjMuanBlZw==%2Cr_max%2Cw_90%2Cx_87%2Cy_72/v1627274783/default/og-base_z4sxah.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Railsにviteを入れてviewをlive reloadする手順</h1>
		<p class="rich-link-card-description">
		
		</p>
		<p class="rich-link-href">
		https://zenn.dev/sesere/articles/23a8ef2c54d489
		</p>
	</div>
</a></div>




```shell
$ rails new vite-test
$ cd vite-test

$ yarn add tailwind
```

Gemfile

```ruby
gem 'vite_rails'
```

```shell
$ bundle install

$ vite install

Creating binstub
Check that your vite.json configuration file is available in the load path:

	No such file or directory @ rb_sysopen - /Users/george/repository/vite-test/config/vite.json

Creating configuration files
Installing sample files
Installing js dependencies
yarn add v1.22.18
[1/4] Resolving packages...
[2/4] Fetching packages...
[3/4] Linking dependencies...
[4/4] Building fresh packages...
success Saved lockfile.
success Saved 7 new dependencies.
info Direct dependencies
├─ vite-plugin-ruby@3.0.12
└─ vite@2.9.9
info All dependencies
├─ debug@4.3.4
├─ esbuild-darwin-64@0.14.42
├─ esbuild@0.14.42
├─ ms@2.1.2
├─ rollup@2.75.3
├─ vite-plugin-ruby@3.0.12
└─ vite@2.9.9
Done in 4.05s.
Adding files to .gitignore

Vite ⚡️ Ruby successfully installed! 🎉
```


この人はstyleshetts を作って tailwind を入れてる。
多分 Live  素早く反映してくれる。

```
app
└── frontend
    ├── entrypoints
    │   ├── application.js
    │   └── stylesheet.js # NEW!
    └── stylesheets # NEW!

```

 なので、ここではこうしてみる。

```
app
└── javascript
    ├── entrypoints
        ├── application.js

```

frontend ができてなかったけど、javascript の中に entrypoints がある。

```shell
$ bin/dev
```

Vite server が 3036 で起動してる。

localhost:3036/vite-dev

![[Pasted image 20220531130415.png]]

追加されたファイルは
- bin/vite
- vite.config.ts
- config/vite.json

config/vite.json に port が設定してある。
rails でアクセスした時にどう利用できるのかもわからない。


コントローラでページを追加する。

```shell
$ rails g controller draw index
```

http://localhost:3000/draw/index

![[Pasted image 20220531133909.png]]

![[Pasted image 20220531133926.png]]

javascrit/entrypoints/application.js が動いて、Vite が走って console.log が出てる。

```html
    <%= vite_client_tag %>
    <%= vite_javascript_tag 'application' %>
```

これが layout/application.html.erb につけると、全ページに入る。

これを入れたい view ファイルに入れれば、

view/draw/index.hmtl.erb

```html
<%= vite_client_tag %>
<%= vite_javascript_tag 'application' %>

<div>
  <h1 class="font-bold text-4xl">Draw#index</h1>
  <p>Find me in app/views/draw/index.html.erb</p>
</div>
```

このページだけで Vite が入るようになる。

### Typescript でできるか

```shell
$ yarn add -D typescript
```

どうもこれだけでよしなにしてくれるようだ。
後は、拡張子を .ts にするだけだった。

```shell
$ bin/dev
```

Procfile.dev

```
web: bin/rails server -p 3000
css: bin/rails tailwindcss:watch
vite: bin/vite dev
```

ちゃんと vite が動くようになっている。


