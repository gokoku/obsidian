#ts 

---
2022-02-16  11:38

# ts  Observer パターンの例

https://sbcode.net/typescript/observer/




<div class="rich-link-card-container"><a class="rich-link-card" href="https://sbcode.net/typescript/observer/" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://sbcode.net/typescript/assets/images/favicon.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Observer - Design Patterns in TypeScript</h1>
		<p class="rich-link-card-description">
		
		</p>
		<p class="rich-link-href">
		https://sbcode.net/typescript/observer/
		</p>
	</div>
</a></div>






![[Pasted image 20220216114601.png]]

```shell
$ mkdir observer-pattern
$ cd observer-pattern
$ mkdir src
$ touch src/observer-concept.ts
```

```
├── node_modules
├── package-lock.json
├── package.json
└── src
    └── observer-concept.ts
```

tsconfig.json

```json:tsconfig.json
{
  "compilerOptions": {
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "target": "es6",
    "sourceMap": true,
    "lib": [
      "es6",
      "dom"
    ],
    "noUnusedLocals": true,
    "module": "commonjs"
  }
}
```

package.json

```json:package.json
"scripts": {
    "build": "tsc",
    "dev": "tsc -w"
  },
```

src/observer-concept.ts

```ts
interface IObservable {
  // The Subject Interface

  subscribe(observer: IObserver): void
  // The subscribe method

  unsubscribe(observer: IObserver): void
  // The unsubscribe method

  notify(...args: unknown[]): void
  // The notify method
}

class Subject implements IObservable {
  // The Subject Class
  #observers: Set<IObserver>  // private property
  constructor() {
    this.#observers = new Set()
  }

  subscribe(observer: IObserver): void {
    this.#observers.add(observer)
  }

  unsubscribe(observer: IObserver): void {
    this.#observers.delete(observer)
  }

  notify(...args: unknown[]): void {
    this.#observers.forEach(observer => observer.notify(...args))
  }
}

interface IObserver {
  // A method for the Observer to implement
  notify(...args: unknown[]): void
  // Receive notifications
}

class Observer implements IObserver {
  // The concrete observer class
  #id: number

  constructor(observable: IObservable) {
    this.#id = COUNTER++
    observable.subscribe(this)
  }
  notify(...args: unknown[]) {
    console.log(`Observer_${this.#id} received: ${JSON.stringify(args)}`)
  }
}

// The Client
let COUNTER = 1

const SUBJECT = new Subject()
const OBSERVER_1 = new Observer(SUBJECT)
const OBSERVER_2 = new Observer(SUBJECT)

SUBJECT.notify('First Notification', [1,2,3])

// Unsubscribe OBSERVER_2
SUBJECT.unsubscribe(OBSERVER_2)

SUBJECT.notify('Second Notification', {A: 1, B: 2, C: 3})

```

```shell
$ npm run build
$ node ./dist/observer-concept.js

Observer_1 received: ["First Notification",[1,2,3]]
Observer_2 received: ["First Notification",[1,2,3]]
Observer_1 received: ["Second Notification",{"A":1,"B":2,"C":3}]
```

すげ〜!!

