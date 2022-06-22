#js/react 

---
2021-07-09

# Building a Next.js shopping cart app

https://blog.logrocket.com/building-a-next-js-shopping-cart-app/

![Building A Next.js Shopping Cart App](https://blog.logrocket.com/wp-content/uploads/2021/07/Building-Next-js-shopping-cart-app.png)


やれるとこまでやってみよう。

https://github.com/itsnitinr/nextjs-shopping-cart

```shell
$ npx create-next-app shopping-cart
```

css module を使ってる。

なんと!! わかりやすくて、デザインも保っていて、しかも最後まで行けた!!

redux まで網羅してる。

API の Router の仕組みにノックアウト。当て画像素材が画像サービスサイト imgur


-   Setting up a Next.js project with `create-next-app`
-   The routing system in Next.js
-   [Styling with CSS modules](https://blog.logrocket.com/a-deep-dive-into-css-modules/)
-   Image optimization with the `<Image>` component
-   Integrating Redux Toolkit for global state management
-   Static generation and server-side rendering
-   Next.js API routes
-   Data fetching with `getStaticProps()`, `getStaticPaths`, and `getServerSideProps()`



## 全体構成

```text

|-- node_modules
|-- package.json
|-- package-lock.json
|
|-- pages
|   |-- api
|   |   |-- hello.js
|   |   |-- [category].js
|   |   |
|   |   |-- products
|   |       |-- [category].js
|   |       |-- data.json
|   |       |-- index.js
|   |
|   |-- _app.js
|   |-- index.js
|   |-- cart.js
|   |-- shop.js
|
|-- components
|   |-- CategoryCard.js
|   |-- Footer.js
|   |-- Navbar.js
|   |-- ProductCard.js
|
|-- redux
|   |-- cart.slice.js
|   |-- store.js
|
|-- styles
    |-- globals.css
    |-- Home.module.css
    |-- CartPage.module.css
    |-- CategoryCard.module.css
    |-- Footer.module.css
    |-- Navbar.module.css
    |-- ProductCard.module.css
    |-- ShopPage.module.css
```

![[Pasted image 20210709172336.png]]

![[Pasted image 20210709172351.png]]

![[Pasted image 20210709172410.png]]


# Page
### index.js

page/index.js

```js
import CategoryCard from '../components/CategoryCard'
import styles from '../styles/Home.module.css'

const HomePage = () => {
  return (
    <main className={styles.container}>
      <div className={styles.small}>
        <CategoryCard image='https://imgur.com/uKQqsuA.png' name='Xbox' />
        <CategoryCard image='https://imgur.com/3Y1DLYC.png' name='PS5' />
        <CategoryCard image='https://imgur.com/Dm212HS.png' name='Switch' />
      </div>
      <div className={styles.large}>
        <CategoryCard image='https://imgur.com/qb6IW1f.png' name='PC' />
        <CategoryCard
          image='https://imgur.com/HsUfuRU.png'
          name='Accessories'
        />
      </div>
    </main>
  )
}
export default HomePage

```

styles/Home.module.css
```css
.container {
  padding: 0 2rem;
}

.small {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
}
.large {
  display: grid;
  grid-template-columns: 1fr 1fr;
}
```

### _app.js
pages/_app.js

```js
import { Provider } from 'react-redux'
import Navbar from '../components/Navbar'
import Footer from '../components/Footer'
import store from '../redux/store'
import '../styles/globals.css'

function MyApp({ Component, pageProps }) {
  return (
    <Provider store={store}>
      <div className='wrapper'>
        <Navbar />
        <Component {...pageProps} />
        <Footer />
      </div>
    </Provider>
  )
}

export default MyApp
```

styles/globals.css
```css
@import url('https://fonts.googleapis.com/css2?family=Open+Sans:wght@300;400;600;700&display=swap');

*, *==before, *==after {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Open Sans', sans-serif;
}

.wrapper {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
main {
  padding: 2rem;
}
```

### shop.js
pages/shop.js
```js
import ProductCard from '../components/ProductCard'
import styles from '../styles/ShopPage.module.css'
import { getProducts } from './api/products/index'

const ShopPage = ({ products }) => {
  return (
    <div className={styles.container}>
      <h1 className={styles.title}>All Results</h1>
      <div className={styles.cards}>
        {products.map((product) => (
          <ProductCard key={product.id} product={product} />
        ))}
      </div>
    </div>
  )
}

export default ShopPage

export async function getStaticProps() {
  const products = await getProducts()
  return { props: { products } }
}
```

styles/ShopPage.module.css
```css
.title {
  font-size: 2rem;
  text-transform: uppercase;
  margin: 0 1rem 1rem;
}
.container {
  padding: 0 2rem;
  margin-bottom: 2rem;
}
.cards {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: 2.5rem 1rem;
  place-items: center;
}
```

### cart.js
pages/cart.js
```js
import Image from 'next/image'
import { useSelector, useDispatch } from 'react-redux'
import {
  incrementQuantity,
  decrementQuantity,
  removeFromCart,
} from '../redux/cart.slice'

import styles from '../styles/CartPage.module.css'

const CartPage = () => {
  const cart = useSelector((state) => state.cart)
  const dispatch = useDispatch()

  const getTotalPrice = () => {
    return cart.reduce(
      (accumulator, item) => accumulator + item.quantity * item.price,
      0,
    )
  }

  return (
    <div className={styles.container}>
      {cart.length === 0 ? (
        <h1>Your Cart is Empty!</h1>
      ) : (
        <>
          <div className={styles.header}>
            <div>Image</div>
            <div>Product</div>
            <div>Price</div>
            <div>Quantity</div>
            <div>Actions</div>
            <div>Total Price</div>
          </div>
          {cart.map((item) => (
            <div className={styles.body}>
              <div className={styles.image}>
                <Image src={item.image} height='90' width='65' />
              </div>
              <p>{item.product}</p>
              <p>$ {item.price}</p>
              <p>{item.quantity}</p>
              <div className={styles.buttons}>
                <button onClick={() => dispatch(incrementQuantity(item.id))}>
                  +
                </button>
                <button onClick={() => dispatch(decrementQuantity(item.id))}>
                  -
                </button>
                <button onClick={() => dispatch(removeFromCart(item.id))}>
                  x
                </button>
              </div>
              <p>$ {item.quantity * item.price}</p>
            </div>
          ))}
          <h2>Grand Total: $ {getTotalPrice()}</h2>
        </>
      )}
    </div>
  )
}

export default CartPage
```

styles/CartPage.modules.css
```css
.container {
  padding: 2rem;
  text-align: center;
}
.header {
  margin-top: 2rem;
  display: flex;
  justify-content: space-between;
}
.header div {
  flex: 1;
  text-align: center;
  font-size: 1rem;
  font-weight: bold;
  padding-bottom: 0.5rem;
  text-transform: uppercase;
  border-bottom: 2px solid black;
  margin-bottom: 2rem;
}
.body {
  display: flex;
  justify-content: space-between;
  align-items: center;
  text-align: center;
  margin-bottom: 1rem;
}
.body > * {
  flex: 1;
}
.image {
  width: 100px;
}
.buttons > * {
  width: 25px;
  height: 30px;
  background-color: black;
  color: white;
  border: none;
  margin: 0.5rem;
  font-size: 1rem;
}
```


### category/\[category\].js

```js
import { useRouter } from 'next/router'
import ProductCard from '../../components/ProductCard'
import styles from '../../styles/ShopPage.module.css'
import { getProductsByCategory } from '../api/products/[category]'

const CategoryPage = ({ products }) => {
  const router = useRouter()

  return (
    <div className={styles.container}>
      <h1 className={styles.title}>Results for {router.query.category}</h1>
      <div className={styles.cards}>
        {products.map((product) => (
          <ProductCard key={product.id} product={product} />
        ))}
      </div>
    </div>
  )
}

export default CategoryPage

export async function getServerSideProps(ctx) {
  const category = ctx.query.category
  const products = await getProductsByCategory(category)
  return { props: { products } }
}

```

# Component
### Navbar.js

components/Navbar.js
```js
import Link from 'next/link'
import { useSelector } from 'react-redux'
import styles from '../styles/Navbar.module.css'

const Navbar = () => {
  const cart = useSelector((state) => state.cart)

  const getItemsCount = () => {
    return cart.reduce((accumulator, item) => accumulator + item.quantity, 0)
  }

  return (
    <nav className={styles.navbar}>
      <h6 className={styles.logo}>GameKart</h6>
      <ul className={styles.links}>
        <li className={styles.navlink}>
          <Link href='/'>Home</Link>
        </li>
        <li className={styles.navlink}>
          <Link href='/shop'>Shop</Link>
        </li>
        <li className={styles.navlink}>
          <Link href='/cart'>
            <p>Cart ({getItemsCount()})</p>
          </Link>
        </li>
      </ul>
    </nav>
  )
}

export default Navbar
```

styles/Navbar.module.css
```css
.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 2rem;
}
.logo {
  font-size: 1.2rem;
  font-weight: 600;
  text-transform: uppercase;
}
.links {
  display: flex;
}
.navlink {
  list-style: none;
  margin: 0 0.75rem;
  text-transform: uppercase;
}
.navlink a {
  text-decoration: none;
  color: black;
}
.navlink a:hover {
  color: [[f9826c]];
}
```

### Footer.js

components/Footer.js
```js
import styles from '../styles/Footer.module.css'

const Footer = () => {
  return (
    <footer className={styles.footer}>
      Copyright <span className={styles.brand}>GamesKart</span>{' '}
      {new Date().getFullYear()}
    </footer>
  )
}

export default Footer
```

styles/Footer.modules.css
```css
.footer {
  padding: 1rem 0;
  color: black;
  text-align: center;
}
.brand {
  color: [[f9826c]];
}
```

### CategoryCard.js

components/CategoryCard.js
```js
import Link from 'next/link'
import Image from 'next/image'
import styles from '../styles/CategoryCard.module.css'

const CategoryCard = ({ image, name }) => {
  return (
    <div className={styles.card}>
      <Image className={styles.image} src={image} height={700} width={1300} />
      <Link href={`/category/${name.toLowerCase()}`}>
        <div className={styles.info}>
          <h3>{name}</h3>
          <p>SHOP NOW</p>
        </div>
      </Link>
    </div>
  )
}

export default CategoryCard
```

styles/CategoryCard.module.css
```css
.card {
  margin: 0.5rem;
  flex: 1 1 auto;
  position: relative;
}
.image {
  object-fit: cover;
  border: 2px solid black;
  transition: all 5s cubic-bezier(0.14, 0.96, 0.91, 0.6);
}
.info {
  position: absolute;
  top: 50%;
  left: 50%;
  background: white;
  padding: 1.5rem;
  text-align: center;
  transform: translate(-50%, -50%);
  opacity: 0.8;
  border: 1px solid black;
  cursor: pointer;
}
.card:hover .image {
  transform: scale(1.2);
}
.card:hover .info {
  opacity: 0.9;
}
```

### ProductCard.js
components/ProductCard.js
```js
import Image from 'next/image'
import { useDispatch } from 'react-redux'
import { addToCart } from '../redux/cart.slice'
import styles from '../styles/ProductCard.module.css'

const ProductCard = ({ product }) => {
  const dispatch = useDispatch()

  return (
    <div className={styles}>
      <Image src={product.image} height={300} width={220} />
      <h4 className={styles.tile}>{product.product}</h4>
      <h5 className={styles.category}>{product.category}</h5>
      <p>$ {product.price}</p>
      <button
        onClick={() => dispatch(addToCart(product))}
        className={styles.button}>
        Add to Cart
      </button>
    </div>
  )
}

export default ProductCard
```

styles/ProductCard.module.css
```css
.card {
  display: flex;
  flex-direction: column;
}
.title {
  font-size: 1rem;
  font-weight: 600;
}
.category {
  font-size: 0.8rem;
  text-transform: uppercase;
}
.button {
  width: 100%;
  margin-top: 0.5rem;
  padding: 0.75rem 0;
  background: transparent;
  text-transform: uppercase;
  border: 2px solid black;
  cursor: pointer;
}

.button:hover {
  background: black;
  color: white;
}
```

# Adding domain

next.config.js
```js
module.exports = {
  reactStrictMode: true,
  images: {
    domains: ['imgur.com'],
  },
}
```

# Next.js API routes
### api/products/index.js
pages/api/products/index.js
```js
import data from './data.json'

export function getProducts() {
  return data
}

export default function handler(req, res) {
  if (req.method !== 'GET') {
    res.setHeader('Allow', ['GET'])
    res.status(405).json({ message: `Method ${req.method} is not allowed` })
  } else {
    const products = getProducts()
    res.status(200).json(products)
  }
}

```

### api/products/\[category\].js
pages/api/products/\[category\].js
```js
import data from './data.json'

export function getProductsByCategory(category) {
  const products = data.filter((product) => product.category == category)
  return products
}

export default function handler(req, res) {
  if (req.method !== 'GET') {
    res.setHeader('Allow', ['GET'])
    res.status(405).json({ message: `Method ${req.method} is not allowed` })
  } else {
    const products = getProductsByCategory(req.query.category)
    res.status(200).json(products)
  }
}

```

### api/products/data.js
pages/api/products/data.json
```json
[
  {
    "id": 1,
    "product": "Cyberpunk 2077",
    "category": "xbox",
    "image": "https://imgur.com/3CF1UhY.png",
    "price": 36.49
  },
  {
    "id": 2,
    "product": "Grand Theft Auto 5",
    "category": "xbox",
    "image": "https://imgur.com/BqNWnDB.png",
    "price": 21.99
  },
  {
    "id": 3,
    "product": "Minecraft",
    "category": "xbox",
    "image": "https://imgur.com/LXnUnd2.png",
    "price": 49.99
  },
  {
    "id": 4,
    "product": "PUBG",
    "category": "xbox",
    "image": "https://imgur.com/Ondg3Jn.png",
    "price": 5.09
  },
  {
    "id": 5,
    "product": "FIFA 21",
    "category": "xbox",
    "image": "https://imgur.com/AzT9YMP.png",
    "price": 17.49
  },
  {
    "id": 6,
    "product": "Battlefield 5",
    "category": "xbox",
    "image": "https://imgur.com/X3MQNVs.png",
    "price": 29.35
  },
  {
    "id": 7,
    "product": "Watch Dogs 2",
    "category": "xbox",
    "image": "https://imgur.com/v3lqCEb.png",
    "price": 18.99
  },
  {
    "id": 8,
    "product": "Fortnite",
    "category": "ps5",
    "image": "https://imgur.com/3lTxDpl.png",
    "price": 29.99
  },
  {
    "id": 9,
    "product": "Call of Duty: Black Ops",
    "category": "ps5",
    "image": "https://imgur.com/4GvUw3G.png",
    "price": 69.99
  },
  {
    "id": 10,
    "product": "NBA2K21 Next Generation",
    "category": "ps5",
    "image": "https://imgur.com/Mxjvkws.png",
    "price": 69.99
  },
  {
    "id": 11,
    "product": "Spider-Man Miles Morales",
    "category": "ps5",
    "image": "https://imgur.com/guV5cUF.png",
    "price": 29.99
  },
  {
    "id": 12,
    "product": "Resident Evil Village",
    "category": "ps5",
    "image": "https://imgur.com/1CxJz8E.png",
    "price": 59.99
  },
  {
    "id": 13,
    "product": "Assassin's Creed Valhalla",
    "category": "ps5",
    "image": "https://imgur.com/xJD093X.png",
    "price": 59.99
  },
  {
    "id": 14,
    "product": "Animal Crossing",
    "category": "switch",
    "image": "https://imgur.com/1SVaEBk.png",
    "price": 59.99
  },
  {
    "id": 15,
    "product": "The Legend of Zelda",
    "category": "switch",
    "image": "https://imgur.com/IX5eunc.png",
    "price": 59.99
  },
  {
    "id": 16,
    "product": "Stardew Valley",
    "category": "switch",
    "image": "https://imgur.com/aL3nj5t.png",
    "price": 14.99
  },
  {
    "id": 17,
    "product": "Mario Golf Super Rush",
    "category": "switch",
    "image": "https://imgur.com/CPxlyEg.png",
    "price": 59.99
  },
  {
    "id": 18,
    "product": "Super Smash Bros",
    "category": "switch",
    "image": "https://imgur.com/ZuLatzs.png",
    "price": 59.99
  },
  {
    "id": 19,
    "product": "Grand Theft Auto 5",
    "category": "pc",
    "image": "https://imgur.com/9LRil4N.png",
    "price": 29.99
  },
  {
    "id": 20,
    "product": "Battlefield V",
    "category": "pc",
    "image": "https://imgur.com/T3v629h.png",
    "price": 39.99
  },
  {
    "id": 21,
    "product": "Red Dead Redemption 2",
    "category": "pc",
    "image": "https://imgur.com/aLObdQK.png",
    "price": 39.99
  },
  {
    "id": 22,
    "product": "Flight Simulator 2020",
    "category": "pc",
    "image": "https://imgur.com/2IeocI8.png",
    "price": 59.99
  },
  {
    "id": 23,
    "product": "Forza Horizon 4",
    "category": "pc",
    "image": "https://imgur.com/gLQsp6N.png",
    "price": 59.99
  },
  {
    "id": 24,
    "product": "Minecraft",
    "category": "pc",
    "image": "https://imgur.com/qm1gaGD.png",
    "price": 29.99
  },
  {
    "id": 25,
    "product": "Rainbow Six Seige",
    "category": "pc",
    "image": "https://imgur.com/JIgzykM.png",
    "price": 7.99
  },
  {
    "id": 26,
    "product": "Xbox Controller",
    "category": "accessories",
    "image": "https://imgur.com/a964vBm.png",
    "price": 59.0
  },
  {
    "id": 27,
    "product": "Xbox Controller",
    "category": "accessories",
    "image": "https://imgur.com/ntrEPb1.png",
    "price": 69.0
  },
  {
    "id": 28,
    "product": "Gaming Keyboard",
    "category": "accessories",
    "image": "https://imgur.com/VMe3WBk.png",
    "price": 49.99
  },
  {
    "id": 29,
    "product": "Gaming Mouse",
    "category": "accessories",
    "image": "https://imgur.com/wvpHOCm.png",
    "price": 29.99
  },
  {
    "id": 30,
    "product": "Switch Joy-Con",
    "category": "accessories",
    "image": "https://imgur.com/faQ0IXH.png",
    "price": 13.99
  }
]
```


### api/\[category\].js
 pages/api/\[category\].js
 ```js
import { useRouter } from 'next/router'
import ProductCard from '../../components/ProductCard'
import styles from '../../styles/ShopPage.module.css'
import { getProductsByCategory } from '../api/products/[category]'

const CategoryPage = ({ products }) => {
  const router = useRouter()
  return (
    <div className={styles.container}>
      <h1 className={styles.title}>Results for {router.query.category}</h1>
      <div className={styles.cards}>
        {products.map((product) => (
          <ProductCard key={product.id} product={product} />
        ))}
      </div>
    </div>
  )
}

export default CategoryPage

export async function getServerSideProps(ctx) {
  const category = ctx.query.category
  const products = await getProductsByCategory(category)
  return { props: { products } }
}
```



# Redux

Redux インストール
```shell
$ npm install @reduxjs/toolkit react-redux

$ npm install --save redux-devtools-extension  // for chrome redux extension
```


### cart.slice.js
redux/cart.slice.js
```js
import { applyMiddleware, createSlice } from '@reduxjs/toolkit'    // applyMiddleware is for chrome redux extention

import { composeWithDevTools} from 'redux-devtools-extension'      // for chrome redux extention

const cartSlice = createSlice({
  name: 'cart',
  initialState: [],
  reducers: {
    addToCart: (state, action) => {
      const itemExists = state.find((item) => item.id === action.payload.id)
      if (itemExists) {
        itemExists.quantity++
      } else {
        state.push({ ...action.payload, quantity: 1 })
      }
    },
    incrementQuantity: (state, action) => {
      const item = state.find((item) => item.id === action.payload)
      item.quantity++
    },
    decrementQuantity: (state, action) => {
      const item = state.find((item) => item.id === action.payload)
      if (item.quantity === 1) {
        const index = state.findIndex((item) => item.id === action.payload)
        state.splice(index, 1)
      } else {
        item.quantity--
      }
    },
    removeFromCart: (state, action) => {
      const index = state.findIndex((item) => item.id === action.payload)
      state.splice(index, 1)
    },
  },
},
   composeWithDevTools(applyMiddleware())	// for chrome redux extention
)

export const cartReducer = cartSlice.reducer

export const {
  addToCart,
  incrementQuantity,
  decrementQuantity,
  removeFromCart,
} = cartSlice.actions

```

### store.js
redux/store.js
```js
import { configureStore } from '@reduxjs/toolkit'
import { cartReducer } from './cart.slice'

const reducer = {
  cart: cartReducer,
}

const store = configureStore({
  reducer,
})

export default store
```

