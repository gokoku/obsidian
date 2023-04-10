-  --
type: note
---

#ts

---
2022-10-18  14:15

# ts  TypeScript で jQuery

```shell
$ npm i jquery @types/jquery
```

txconfig.json の追加。

```json
{
  "compilerOptions": {
    .
    .
    .
    "allowSyntheticDefaultImports": true
  }
```

```ts
import $ from 'jquery'

$(document).
```


