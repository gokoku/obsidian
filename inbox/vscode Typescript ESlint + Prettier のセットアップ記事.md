---
type: note
---

#ts #vscode 

---
2023-04-25  08:44

# typescript ESlint + Prettier のセットアップ記事



一連の記事のようだ。
[[React Hero Setup a new React application with Vite + TypeScript]]


<div class="rich-link-card-container"><a class="rich-link-card" href="https://medium.com/tinyso/react-hero-setup-eslint-for-typescript-react-application-d171df2bb408" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">React Hero: Setup ESlint for TypeScript + React application</h1>
		<p class="rich-link-card-description">
		If you’re working on a Typescript + React project, setting up ESLint + Prettier can help you catch errors early and enforce coding…
		</p>
		<p class="rich-link-href">
		https://medium.com/tinyso/react-hero-setup-eslint-for-typescript-react-application-d171df2bb408
		</p>
	</div>
</a></div>



VScode の extension でセットの仕方を見よう。
と思ったら、プロジェクト毎にセットアップするようになっているので、自前でやるにはゲシゲシとやるものなようだ。

## Install ESLint , Typescript ESLint parser

Vite ですでにセットされているが、やってみる。
```shell
$ npm i eslint --include=dev
$ npm i @typescript-eslint/parser --include=dev
```

## install the required plugins
```shell
$ npm i eslint-plugin-react eslint-plugin-react-hooks @typescript-eslint/eslint-plugin --include=dev
```

.eslintrc.json (.eslintrc.cjs があったので、リネームした)

```json
{
  "env": {
    "browser": true,
    "node": true,
    "es6": true
  },
  "extends": [
    "eslint:recommended",
    "plugin:@typescript-eslint/recommended",
    "plugin:react/recommended",
    "plugin:react-hooks/recommended"
  ],
  "parser": "@typescript-eslint/parser",
  "parserOptions": { "ecmaVersion": "latest", "sourceType": "module" },
  "plugins": ["@typescript-eslint", "react-hooks"],
  "rules": {
    "react/jsx-uses-react": "off",
    "react/react-in-jsx-scope": "off",
    "quotes": ["error", "single"]
  },
  "settings": {
    "react": {
      "version": "detect"
    }
  }
}

```


package.json
```json
{
  "scripts": {
    "lint": "eslint ."
  }
}
```
とあるが、既にセットされているので、そのままにする。


```shell
$ npm run lint
```
"react/jsx-uses-react": "off",
"react/react-in-jsx-scope": "off", がないとerrorが出る。
https://ja.legacy.reactjs.org/blog/2020/09/22/introducing-the-new-jsx-transform.html

target="_blank" でerrorが出る。rel を追加しろと言われるので付ける。
```html
<a href="https://vitejs.dev" target="_blank" rel="noreferrer">
```

```shell
$ npm run lint
```
やっとLinter が通った。





ESlint
![[Pasted image 20230425084701.png]]

settings.json
```js
{  
	"eslint.enable": true,  
	"editor.codeActionsOnSave": {  
		"source.fixAll.eslint": true  
	},  
	"eslint.validate": [  
		"javascript",  
		"typescript",  
		"typescriptreact"  
	]  
}
```


### Prettier setup

```shell
$ npm i prettier --include=dev

$ npm i eslint-config-prettier eslint-plugin-prettier --include=dev

$ touch .prettierrc.json
```

.prettierrc.json
```json
{
  "singleQuote": true,
  "semi": false,
  "tabWidth": 2,
  "trailingComma": "all"
}
```

.eslintrc.json に Prettier を加える。

```json
"extends": [
　　"eslint:recommended",
　　"plugin:@typescript-eslint/recommended",
　　"plugin:react/recommended",
　　"plugin:react-hooks/recommended",
　　"eslint-config-prettier"　　　　　// <-----------this new line

],
```

VSCode settings.json に Prettier configure をセット。

```json
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "prettier.singleQuote": true,
  "prettier.semi": false,
  "prettier.tabWidth": 2,
  "prettier.trailingComma": "all",
  "prettier.bracketSameLine": true,
```


```shell
$ npm run dev
```

http://localhost:5173

![[Pasted image 20230425101437.png]]

Year!!

