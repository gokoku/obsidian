---
type: note
---

#js/stimulus

---
2022-05-19  16:53

# js Hello, Stimulus



<div class="rich-link-card-container"><a class="rich-link-card" href="https://stimulus.hotwired.dev/handbook/hello-stimulus" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://stimulus.hotwired.dev/assets/favicon-32x32.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Stimulus Handbook</h1>
		<p class="rich-link-card-description">
		
		</p>
		<p class="rich-link-href">
		https://stimulus.hotwired.dev/handbook/hello-stimulus
		</p>
	</div>
</a></div>


```shell
$ git clone https://github.com/hotwired/stimulus-starter.git
$ cd stimulus-starter
$ yarn install
$ yarn start

error

$ npx browserslist@latest --update-db

$ yarn start

error
```

https://github.com/gh640/nextjs-blog-example-ja/issues/49

node v17 で OpenSSL 3.0 を含む形に変わったことで起こってるらしい。

if you hit an 'ERR_OSSL_EVP_UNSUPPORTED' error in your application with Node.js 17,
it's likely that your application or a module you're using is attempting to use an algorithm or key size which is no longer allowed by default with OpenSSL 3.0.
A command-line option, --openssl-legacy-provider, has been added to revert to the legacy provider as a temporary warkaround for these tightened restrictions.

NODE_OPTIONS=--openssl-legacy-provider を付けて実行するようにすればいいらしい。

package.json

```json
  "scripts": {
    "start": "NODE_OPTIONS=--openssl-legacy-provider node server.js"
  },
```

```shell
$ yarn start
```

localhost:9000
![[Pasted image 20220519170804.png]]

public/index.html から始まるらしい。

public/index.html

```html
<div>  
	<input type="text">  
	<button>Greet</button>  
</div>
```

#### Conrollers Bring HTML to Life

src/controllers/hello_controller.js

```js
import { Controller } from "@hotwired/stimulus"

export default class extends Controller {
}
```

### Identifiers Link Controllers With the DOM
public/index.html

```html
<div data-controller="hello">  
	<input type="text">  
	<button>Greet</button>  
</div>
```

### Is This Thing On ?

src/controllers/hello_controller.js

```js
import { Controller } from "@hotwired/stimulus"

export default class extends Controller {
  connect() {
    console.log("Hello, Stimulus!", this.element)
  }
}
```

![[Pasted image 20220519171754.png]]

### Actions Respond to DOM Events

```js
import { Controller } from "@hotwired/stimulus"

export default class extends Controller {
  greet() {
    console.log("Hello, Stimulus!", this.element)
  }
}
```

public/index.html

```html
  <div data-controller="hello">
    <input type="text">
    <button data-action="click->hello#greet">Greet</button>
  </div>
```

### Targets Map Inportant Elements To Controller Properties

public/index.html

```html
  <div data-controller="hello">
    <input data-hello-target="name" type="text">
    <button data-action="click->hello#greet">Greet</button>
  </div>
```

property target に name を add すると自動的に nameTarget property が作られる。

src/controllers/hello_controller.js

```js
import { Controller } from "@hotwired/stimulus"

export default class extends Controller {
  static targets = [ "name" ]
  greet() {
    const element = this.nameTarget
    const name = element.value
    console.log(`Hello, ${name}`)
  }
}
```

![[Pasted image 20220519172605.png]]

### Controllers Simplify Refactoring

src/controllers/hello_controller.js

```js
export default class extends Controller {
  static targets = [ "name" ]
  greet() {
    console.log(`Hello, ${this.name}`)
  }
  get name() {
    return this.nameTarget.value
  }
}
```

getter だ。

