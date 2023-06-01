#js/react 

---
2021-10-14

# ReactのUIコンポーネントライブラリ「Chakra UI」とは？ カスタマイズ性と生産性を両立しよう【前編】

https://codezine.jp/article/detail/14911?utm_source=codezine_regular_20211013&utm_medium=email


公式 : https://chakra-ui.com/docs/getting-started#installation

```shell
$ npx create-react-app chakra-ui-sample --template @chakra-ui
$ cd chakra-ui-sample
$ yarn start
```

![[Pasted image 20211014102054.png]]


![図4：UIの構造](https://cz-cdn.shoeisha.jp/static/images/article/14911/14911_004.png)

\<Box\> は \<div\> の代わりに使うとのこと。

xs  : 0.75rem
sm : 0.875rem
md : 1rem
lg    : 1.125rem
xl    : 1.25rem
2xl  : 1.5rem
3xl  : 1.875rem
4xl  : 2.25rem
5xl  :  3rem
6xl  :  3.75rem
7xl  :  4.5rem
8xl  : 6rem
9xl  : 8rem

```
<Box> は <div> の代わりに使うとのこと。



<Grid> は CSS Grid Layout 
	<Grid minH="100vh" p={3}>
		 minH : minHeight
		 p = padding
		 
<VStack> は <Box> の拡張、中身を立て並べにする。
	spacing 要素の間隔
	spacing={8} は 2rem
```

## テーマのカスタマイズ

extendTheme 関数

```js

import React from 'react';
import {
  ChakraProvider,
  Box,
  Text,
  Link,
  VStack,
  Code,
  Grid,
  extendTheme,
} from '@chakra-ui/react';
import { ColorModeSwitcher } from './ColorModeSwitcher';
import { Logo } from './Logo';

const theme = extendTheme({
  colors: {
    brand: {
      main: '[[008ccf]]',
    },
    // デフォルトのテーマはこんな形
    // teal: {
    //    50: "[[36fffa]]",
    //    100: "[[b2f5ea]]",
    //   ...
    // },
    // ...
  },
});

function App() {
  return (
    <ChakraProvider theme={theme}>

            <Link
              //color="teal.500"
              color="brand.main"
              href="https://chakra-ui.com"
              fontSize="2xl"
              target="_blank"
              rel="noopener noreferrer"
            >	  
	  
```

color を brand.main で指定出来た。


