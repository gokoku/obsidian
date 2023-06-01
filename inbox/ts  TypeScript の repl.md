#ts

---
2021-08-20

# TypeScript の Repl

```shell
$ npm install -g typescript ts-node

$ asdf reshim nodejs

$ ts-node
>
```

* インタフェース
オブジェクトに名前をつけることができるように定義するやつ

```ts
interface Color {
  readonly rgb: string;         # readonly 書き換え不可
  opacity: number;
  name?: string                 # ? 省略可能
}

const turquoise: Color = { rgb: '00afcc', opacity: 1 }
turquoise.name = 'Turqoise Blue'
turquoise.rgb

'00afcc'
```

* インデックス・シグネチャ

```js
interface Status {
  level: number
  [attr: string]: number
}

const myStatus Status = {
  level: 99,
  attack: 999,
  defense: 999
}
```

フレキシブルにプロパティを追加出来る。

* Enum 型

```ts
emun Pet { Cat, Dog, Rabbit }
console.log( Pet.Cat, Pet.Dog, Pet.Rabbit)
0 1 2
```

* リテラル型

```ts
let May: 'Cat' | 'Dog' | 'Rabbit' = 'Cat'

Mary = 'Rabbit'
Maty = 'Parrot'  <--エラー
```

列挙体のように使えるので便利。

* タプル型

```ts
const charattrs: [number, string, boolean] = [1, 'patty', true]
```

レストパラメータが使える。
```ts
const spells: [number, ...string[]] = [7, 'heal', 'sizz', 'snooz']
```

4.2 からの導入とのこと。

API 関数の戻り値として使われる。分割代入で抽出して使う。

```ts
const userAttrs: [number, string, boolean] = [1, 'patty', true]
const [id, username, isAdmin] = userAttrs
```

* Any Unknown Never

any はなんでも受け付ける型。

```ts
let val: any = 100
val = 'buz'
val = null
```

データの型が不明なまま処理を描くとき使う。

```ts
const str = `{"id": 1, "username": "patty"}`
const user =JSON.parse(str)

console.log(user.id, user.address.zipCode)

このとき unknown 型でエラーになるらしいが?
```

