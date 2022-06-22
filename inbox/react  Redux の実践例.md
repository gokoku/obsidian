#js/react #js/redux 

---
2021-08-02

# Redux の実践例

![[Pasted image 20210802145116.png]]

![[Pasted image 20210802145408.png]]


```shell
$ npx create-react-app myapp
$ cd myapp

$ npm install redux react-redux
```


## src

### src/index.js

```js
import React from "react";
import ReactDOM from "react-dom";
import { Provider } from "react-redux";
import "./index.css";
import App from "./App";
import { configureStore } from "./state/store";

const store = configureStore({});

ReactDOM.render(
    <Provider store={store}>
        <React.StrictMode>
            <App />
        </React.StrictMode>
    </Provider>,
    document.getElementById("root")
);
```

### src/index.css

```css
body {
    margin: 0;
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto",
        "Oxygen", "Ubuntu", "Cantarell", "Fira Sans", "Droid Sans",
        "Helvetica Neue", sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
}

code {
    font-family: source-code-pro, Menlo, Monaco, Consolas, "Courier New",
        monospace;
}
```

### src/App.js

```js
import "./App.css";
import { Shop } from "./components/Shop";
import { Cart } from "./components/Cart";

/**
 * アプリケーションの親コンポーネントです。
 */
export const App = () => {
    return (
        <div>
            <header className="header">Shop Site</header>
            <div className="container">
                <Shop />
                <Cart />
            </div>
        </div>
    );
};

export default App;
```

### src/App.css

```css
.header {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    padding: 1rem;
    border-bottom: 1px solid [[d0d0d0]];
}

a {
    text-decoration: none;
    color: #484848;
    transition: 0.3s ease-out;
}

a:hover {
    color: [[7fbfff]];
}

.container {
    padding: 1rem;
}

.shop {
    display: flex;
    gap: 2rem;
}

.shop_item {
    padding: 1rem;
    border-bottom: 1px solid [[d0d0d0]];
    box-shadow: 1px 1px 3px [[b1b1b1]];
}

.shop_itemName {
    text-align: left;
    font-weight: bold;
}

.shop_description {
    font-size: 12px;
    max-width: 300px;
}

.shop_addButton {
    border: none;
    background: [[2b91ff]];
    border-radius: 4px;
    color: #f;
    padding: 0.25rem 0.5rem;
    cursor: pointer;
}

.cart {
    margin: 4rem;
    display: grid;
    justify-content: center;
    border-top: 1px solid [[d0d0d0]];
    padding-top: 1rem;
}

.cart_title {
    font-weight: bold;
    font-size: 1.25rem;
}

.cart_item {
    display: flex;
    align-items: center;
}

.cart_block {
    width: 200px;
}

.cart_itemName {
    font-size: 1.25rem;
    margin: 0;
    font-weight: bold;
}
.cart_price {
    margin: 0;
}

.cart_stock {
    font-size: 1.5rem;
    margin-left: 5rem;
}

```

### src/components
#### src/compoments/Shop.jsx

```js
import React from "react";
import { useDispatch } from "react-redux";
import { addToCart } from "../state/cart/operations";
import { shopData } from "../data/shopData";

export const Shop = () => {
    // Redux と繋ぎ込みを行うために Dispatch関数を用意
    const dispatch = useDispatch();
    const shopItems = shopData.products.map((item) => (
        <div className="shop_item" key={item.id}>
            <p className="shop_itemName">{item.name}</p>
            <p className="shop_description">{item.description}</p>
            <p className="shop_itemName">{item.price} 円</p>
            {/* クリック時、ショップの商品をカートに追加するアクションを発行 */}
            <button
                className="shop_addButton"
                onClick={() => {
                    dispatch(addToCart(item.id, item.name, item.price));
                }}
            >
                カートに追加する
            </button>
        </div>
    ));
    return <div className="shop">{shopItems}</div>;
};

```

#### src/compoments/Cart.jsx

```js
import React from "react";
import { useSelector } from "react-redux";
/**
 * カートの中身を表示するコンポーネントです。
 */
export const Cart = () => {
    // カートの状態をReducerから取得
    const cartItems = useSelector((state) => state.cart);
    // カートの合計金額を表示
    const totalPrices = cartItems
        .map((item) => item.totalPrice)
        .reduce((pv, cv) => pv + cv, 0);
    if (cartItems.length === 0) {
        return <div className="cart">カートにアイテムがありません。</div>;
    }
    const cart = cartItems.map((item) => (
        <div className="cart_item" key={item.id}>
            <div className="cart_block">
                <p className="cart_itemName">{item.name}</p>
                <p className="cart_price">{item.price} 円</p>
            </div>
            <p className="cart_stock">{item.stock} 個</p>
        </div>
    ));
    return (
        <div className="cart">
            <p className="cart_title">カートの中身</p>
            <div>{cart}</div>
            <div className="cart_total">合計金額: {totalPrices} 円</div>
        </div>
    );
};

```



### src/state

#### src/state/store.js

```js
import { createStore, compose } from "redux";
import cartReducer from "./cart";

/**
 * Reduxのストアを作成する関数です。
 * @return {Store<unknown>}
 */
export const configureStore = () => {
    return createStore(
        cartReducer,
        compose(
            process.env.NODE_ENV === "development" && window.devToolsExtension
                ? window.devToolsExtension()
                : (f) => f
        )
    );
};
```

#### src/state/cart

##### src/state/cart/actions.js

```js
// actions では、アプリケーションから受け取った
// 状態変更依頼を受け取り、reducers に渡します。

import * as types from "./types";

/**
 * カートに商品を追加するアクションです。
 * @param id
 * @param name
 * @param price
 * @return {{payload: {price, name, id}, type: string}}
 */
export const addToCart = (id, name, price) => ({
    type: types.ADD_ITEM,
    payload: {
        id,
        name,
        price,
    },
});

/**
 * カートの中身をリセットするアクションです。
 * @return {{type: string}}
 */
export const clearCart = () => ({
    type: types.CLEAR,
});
```

##### src/state/cart/index.js

```js
import reducer from "./reducers";

import * as cartOperations from "./operations";
import * as cartSelectors from "./selectors";

export { cartOperations, cartSelectors };

export default reducer;
```

##### src/state/cart/operations.js

```js
// operations ではアクションを発行する前に実行したい、複雑な処理を記述します。
// 例えば、外部API通信やDBとの接続などです。
// 今回のケースでは外部通信などはないため、アクションをそのまま渡しています。

import { addToCart, clearCart } from "./actions";

export { addToCart, clearCart };
```

##### src/state/cart/reducers.js

```js
import * as types from "./types";
import {
    filterCartItem,
    getCartItem,
    sortCartItems,
    checkExistItem,
} from "./selectors";

const initialState = {
    cart: [],
};
/**
 * Reducerでは、受け取ったアクションから
 * アプリケーションの状態をどう変更するかを決定して新しい状態を返します。
 * @param state
 * @param action
 * @return {{cart: []}
 */
const reducer = (state = initialState, action) => {
    switch (action.type) {
        // ①カートにアイテムを追加するアクション
        case types.ADD_ITEM:
            // ②アクションから渡ってきたID、名前、値段を取得
            const { id, name, price } = action.payload;
            // 新しい状態を定義する配列を用意
            let newCart = [];
            // すでにカートに存在しているアイテムかどうかを確認
            const isExistItem = checkExistItem(state.cart, id);
            // すでにカート内に存在している商品であれば
            if (isExistItem) {
                // ステートの同じIDの商品を抽出
                const newItem = getCartItem(state.cart, id);
                // 個数を増やす
                const newItemStock = newItem.stock + 1;
                // 合計金額を増やす
                const newItemTotalPrice = newItem.price * (newItem.stock + 1);
                // 既存のステートから同じ商品IDのアイテムを一度除外
                const filteredCartItem = filterCartItem(state.cart, id);
                newCart = sortCartItems([
                    ...filteredCartItem,
                    {
                        ...newItem,
                        stock: newItemStock,
                        totalPrice: newItemTotalPrice,
                    },
                ]);
            } else {
                // 一度も追加されてことのない商品であれば、
                // 新しくオブジェクトを追加する
                newCart = sortCartItems([
                    ...state.cart,
                    {
                        id,
                        name,
                        price,
                        stock: 1,
                        totalPrice: price,
                    },
                ]);
            }
            // ③新しい状態を作り直す
            return {
                ...state,
                cart: newCart,
            };
        default:
            return state;
    }
};

export default reducer;
```

##### src/state/cart/selectors.js

```js
// selector では、ストアで管理している状態を参照する関数を定義します。

/**
 * 指定したIDのカート商品を検索して返します。
 * @param cart
 * @param id
 */
export const getCartItem = (cart, id) => {
    return cart.find((item) => item.id === id);
};

/**
 * 指定したIDのカート商品を除外して返します。
 * @param cart
 * @param id
 */
export const filterCartItem = (cart, id) => {
    return cart.filter((item) => item.id !== id);
};

/**
 * データをIDの昇順にして返します。
 * @param cart
 */
export const sortCartItems = (cart) => {
    return cart.slice().sort((a, b) => {
        // データが追加順になってしまうので、昇順での並び替え
        return a.id - b.id;
    });
};

/**
 * すでにカートに存在しているか商品かどうかを判別し、真偽値を返します。
 * @param cart
 */
export const checkExistItem = (cart, id) => {
    return cart.some((item) => item.id === id);
};
```

##### src/state/cart/types.js

```js
// types では、アクションの名称を定数として格納します。
export const ADD_ITEM = "cart/ADD_ITEM";
export const CLEAR = "cart/CLEAR";
```

#### src/data/shopData.js

```js
/**
 * 商品のモックデータです。
 * @type {{products: [{price: number, imageUrl: string, name: string, description: string, id: number}]}}
 */
export const shopData = {
    products: [
        {
            id: 1001,
            name: "バナナ",
            price: 200,
            description:
                "太さを保ちつつ長さもある大型のバナナ。デザートにどうぞ",
            imageUrl: "/dist/images/banana.jpg",
        },
        {
            id: 1002,
            name: "リンゴ",
            price: 150,
            description:
                "そのままでも、加工してリンゴ酒、ジャム、ジュース、菓子の材料などにもどうぞ。",
            imageUrl: "/dist/images/apple.jpg",
        },
        {
            id: 1003,
            name: "オレンジ",
            price: 100,
            description:
                "そのままでも、ジュース、肉料理のソースとしてもどうぞ。",
            stock: 100,
            imageUrl: "/dist/images/orange.jpg",
        },
        {
            id: 1004,
            name: "スイカ",
            price: 400,
            description:
                "甘くて美味しい、大玉のスイカです。夏にぴったりのフルーツです。",
            imageUrl: "/dist/images/watermelon.jpg",
        },
        {
            id: 1005,
            name: "ブドウ",
            price: 200,
            description:
                "熟したブドウです。そのままでも、ソースなどにもどうぞ。",
            imageUrl: "/dist/images/grape.png",
        },
    ],
};

```

## package.json
```json
{
  "name": "redux-example",
  "version": "0.1.0",
  "author": "ics-morita<morita@ics-web.jp>",
  "private": true,
  "homepage": ".",
  "dependencies": {
    "react": "17.0.2",
    "react-dom": "17.0.2",
    "react-scripts": "4.0.3",
    "redux": "4.1.0",
    "react-redux": "7.2.4"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build"
  },
  "eslintConfig": {
    "extends": [
      "react-app",
      "react-app/jest"
    ]
  },
  "browserslist": {
    "production": [
      ">0.2%",
      "not dead",
      "not op_mini all"
    ],
    "development": [
      "last 1 chrome version",
      "last 1 firefox version",
      "last 1 safari version"
    ]
  }
}
```
