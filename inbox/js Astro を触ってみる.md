	---
type: note
---

#js

---
2022-09-29  09:28

# js Astro を触ってみる

アイランドアーキテクチャのフレームワーク。
なんと、各島々でReact Svelte Vue をどれでも使えるらしい。

https://docs.astro.build/en/getting-started/

## プロジェクト生成
```shell
$ npm create astro@latest

色々聞かれる。プロジェクト名はデフォルトの my-astro-site

TypeScript をセッティングされた


$ npm run dev
```

![[Pasted image 20220929093121.png]]

![[Pasted image 20220929093441.png]]

vscode で extention Astro を入れた。


## React を使ってみる

https://push.tokyo/astro-react/

```shell
$ npx astro add react

-----------
自分でやるには
$ npm i @astrojs/react
$ npm i react react-dom
とやるとのこと
-----------
```


なんだか React の型も入れとくらしい。

```shell
$ npm i -D @types/react
```
`components/Title.tsx` で React Component を作る。

```js
type TitleProps = {
  children: React.ReactNode
}

const Title: React.FC<TitleProps> = ({ children }) => {
  return <h2>{children}</h2>
}

export default Title
```

`pages/index.astro` に component を追加する。

```js
---
import Layout from '../layouts/Layout.astro';
import Card from '../components/Card.astro';
import Title from '../components/Title'

let title = 'Fuga'
---

<Layout title="Welcome to Astro.">
	<main>
		<h1>Welcome to <span class="text-gradient">{title}</span></h1>
		<Title>タイトル</Title>
```
![[Pasted image 20220929132744.png | 500]]

このままだと、js 無しの static な html になってる。
なので、最速なのだったのだ。

動的な js を使うにはどうするのだろうか。


```js
<Button client:load />
```

こうするとインタラクティブになるが、読み込みが格段に遅くなって最速とはならなくなる。

## JSON を事前に読み込んで表示する

youtube でやってたやつ。
react を使うが、js がない。
なので静的サイトの最速になるらしい。

```js
// components/Characters.jsx

import React from 'react'

const Characters = ({characters}) => {
  console.log("characters")
  return (
    <ul>{characters.map(character => {
      return (
        <li>
          {character.name}
          <img width={150} src={character.img_url} alt={`Photo of ${character.name}`} />
        </li>
      )
    })}</ul>
  )
}

export default Characters
```

```js
// pages/characters.astro


---
import Layout from '../layouts/Layout.astro';
import Characters from '../components/Characters'

const characters = await fetch('https://finalspaceapi.com/api/v0/character?limit=5').then(response => response.json() )
---

<Layout title="Welcome to Astro.">
	<main>
		<Characters characters={characters} />
	</main>
</Layout>

```

`http://localhost:3000/characters`

![[Pasted image 20220929180449.png]]


