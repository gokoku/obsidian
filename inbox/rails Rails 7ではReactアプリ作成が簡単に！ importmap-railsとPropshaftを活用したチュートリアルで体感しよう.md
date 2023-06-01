---
type: note
---

#ruby/rails

---
2022-06-30  09:22

# rails Rails 7ではReactアプリ作成が簡単に！ importmap-railsとPropshaftを活用したチュートリアルで体感しよう

https://codezine.jp/article/detail/15973

```shell
$ rails new react_app -a propshaft --skip-hitwire
$ cd react_app

これやらないと importmap.rb が生成されなかった。

$ rails importmap:install

$ rails g controller react hello
```

config/routes.rb

root "react#hello"

### importmap-rails の設定

```shell
$ bin/importmap pin react react-dom/client
```

config/importmap.rb
```ruby
# Pin npm packages by running ./bin/importmap

pin "application", preload: true
pin "react", to: "https://ga.jspm.io/npm:react@18.2.0/index.js"
pin "react-dom/client", to: "https://ga.jspm.io/npm:react-dom@18.2.0/client.js"
pin "process", to: "https://ga.jspm.io/npm:@jspm/core@2.0.0-beta.24/nodelibs/browser/process-production.js"
pin "react-dom", to: "https://ga.jspm.io/npm:react-dom@18.2.0/index.js"
pin "scheduler", to: "https://ga.jspm.io/npm:scheduler@0.23.0/index.js"
```

何これ?
ここに追加。

```ruby
pin_all_from "app/javascript/components", under: "components"
```

これで、app/javascript/components/ にある js が import 出来るようになる。

app/javascript/application.js
```js
import "components/react_hello"
```

app/javascript/components/react_hello.js 追加。

JSX がここでは使えない。それは次のコラムまで。
```js
import React from 'react'
import { createRoot } from 'react-dom/client'

console.log('Hello from react_hello.js')

const Hello = props => (
  React.createElement('div', null, `Hello ${props.name}!!`)
)

Hello.defaultPropps = {
  name: 'World'
}

document.addEventListener('DOMContentLoaded', () => {
  const container = document.getElementById('app')
  const element = React.createElement(Hello, { name: 'React' }, null)
  createRoot(container).render(element)
})
```

![[Pasted image 20220630100204.png]]

### importmap-rails の仕組み

```shell
$ bin/importmap json

{
  "imports": {
    "application": "/assets/application-6934cd9616b5044b68191c8f8c6fe5c2822d1115.js",
    "react": "https://ga.jspm.io/npm:react@18.2.0/index.js",
    "react-dom/client": "https://ga.jspm.io/npm:react-dom@18.2.0/client.js",
    "process": "https://ga.jspm.io/npm:@jspm/core@2.0.0-beta.24/nodelibs/browser/process-production.js",
    "react-dom": "https://ga.jspm.io/npm:react-dom@18.2.0/index.js",
    "scheduler": "https://ga.jspm.io/npm:scheduler@0.23.0/index.js",
    "components/react_hello": "/assets/components/react_hello-7a8aca8f3e97362e9bb88785e99650522448ee6b.js"
  }
}
```

この importmap の取り込みが 
app/views/layouts/application.html

```html
    <%= csp_meta_tag %>

    <%= stylesheet_link_tag "application" %>
    <%= javascript_importmap_tags %>
  </head>
```

ここで取り込まれる仕組み。

### Propshaft の仕組み

マニフェストファイルを使用せず、ファイルを結合せず、圧縮せず。

```shell
$ RAILS_ENV=production rails assets:precompile
```

public/assets にダイジェストを付加したファイルが配置される。
path_to_asset
url_to_asset
stylesheet_link_tag
とか使える。
Sprockets と変わらない。動作が軽い。







