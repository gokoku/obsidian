#js

---
2021-08-20

# Promise をハンドリングする

get-user-the.js

```json
{
	"type": "module"
	"dependencies": {
		"node-fetch": "^2.6.1"
	}
}
```

"type": "module "  が必要。 import を使うなら。

```js
import fetch from 'node-fetch'

const getUser = (userId) =>
  fetch(`https://jsonplaceholder.typicode.com/users/${userId}`).then(
    (response) => {
      if (!response.ok) {
        throw new Error(`${response.status} Error`)
      } else {
        return response.json()
      }
    },
  )

console.log(`--Start--`)

getUser(2)
  .then((user) => {
    console.log(user)
  })
  .catch((error) => {
    console.log(error)
  })
  .finally(() => {
    console.log(`--Completed--`)
  })

```

fetch() 関数も、json() も　Promise オブジェクトを返してくるので、then で受ける。

```shell
$ node get-user-the.js
--Start--
{
  id: 2,
  name: 'Ervin Howell',
  username: 'Antonette',
  email: 'Shanna@melissa.tv',
  address: {
    street: 'Victor Plains',
    suite: 'Suite 879',
    city: 'Wisokyburgh',
    zipcode: '90566-7771',
    geo: { lat: '-43.9509', lng: '-34.4618' }
  },
  phone: '010-692-6593 x09125',
  website: 'anastasia.net',
  company: {
    name: 'Deckow-Crist',
    catchPhrase: 'Proactive didactic contingency',
    bs: 'synergize scalable supply-chains'
  }
}
--Completed--
```

# やっぱり Promise は直感的じゃない

エイシンク・アウェイト
async await を使うと、

```js
import fetch from 'node-fetch'

const getUser = async (userId) => {
  const response = await fetch(
    `https://jsonplaceholder.typicode.com/users/${userId}`,
  )

  if (!response.ok) {
    throw new Error(`${response.status} Error`)
  }

  return response.json()
}

console.log(`--Start--`)

const main = async () => {
  try {
    const user = await getUser(2)
    console.log(user)
  } catch (error) {
    console.error(error)
  } finally {
    console.log('--Completed--')
  }
}

main()
```

```shell
$ node get-user-then.js

--Start--
{
  id: 2,
  name: 'Ervin Howell',
  username: 'Antonette',
  email: 'Shanna@melissa.tv',
  address: {
    street: 'Victor Plains',
    suite: 'Suite 879',
    city: 'Wisokyburgh',
    zipcode: '90566-7771',
    geo: { lat: '-43.9509', lng: '-34.4618' }
  },
  phone: '010-692-6593 x09125',
  website: 'anastasia.net',
  company: {
    name: 'Deckow-Crist',
    catchPhrase: 'Proactive didactic contingency',
    bs: 'synergize scalable supply-chains'
  }
}
--Completed--
```

素晴らしい!!

async をつけると、Promise.resolve() でラップされるとのこと。

await 演算子を付けて呼び出す事ができるようになる。
その非同期関数は Promise が resolve されるか reject されるか返してくる。

だから resolve が成功すると　try 成功して、reject されると error でキャッチされる。

