#js/react 

---
2021-07-15

# Storybook

https://storybook.js.org/

Storybook を使ってみる。

```shell
$ npx create-react-app storybook-sample

$ cd storybook-sample

$ npx sb init
$ npm run storybook
```

.storybook が作られた。src/stories が作られた。

自動でブラウザから立ち上がった。

![[Pasted image 20210715152710.png]]

## 自前のコンポーネントカタログを作る

src/stories/components/atoms/Button.js

```js
import React from 'react'

export const Button = ({ onClick, text}) => (
   <button onClick={onClick}>{text}</button>
)
```

Button コンポーネントをストーリーに登録する。

src/stories/components/atoms/atoms.stories.js

```js
import React from 'react'
import { Button } from './Button'
import { action } from '@storybook/addon-actions'

export default {
  title: 'atoms',
}

export const Buttons = () => (
  <Button onClick={action('clicked')} text='Button'
)
```

![[Pasted image 20210715170756.png]]

色々アドオンがあるらしい。

#### もう少し使えるようにしたい

src/stories/components/atoms/Button.js
```js
import React from 'react'
import PropTypes from 'prop-types'
import './button.css'

export const Button = ({ type, onClick, text }) => (
  <button
    type='button'
    className={['my-button', `my-button--${type}`].join(' ')}
    onClick={onClick}>
    {text}
  </button>
)

Button.propTypes = {
  type: PropTypes.string,
  onClick: PropTypes.func,
  text: PropTypes.string,
}

Button.defaultProps = {
  type: 'primary',
  onClick: undefined,
  text: '',
}
```

src/stories/components/atoms/button.css
```css
.my-button {
  font-family: Helvetica, Arial, sans-serif;
  font-weight: 700;
  border: 0;
  border-radius: 3em;
  cursor: pointer;
  display: inline-block;
  line-height: 1;
  padding: 10px 10px;
}

.my-button--primary {
  color: white;
  background-color: [[1ea7f0]];
}
.my-button--primary:hover {
  background-color: [[1ea0ffaa]];
}

.my-button--secondary {
  color: #333;
  background-color: transparent;
  box-shadow: rgba(0,0,0,0.15) 0px 0px 0px 1px;
}
.my-button--secondary:hover {
  box-shadow: rgba(0,0,0,0.2) 1px 1px 0px 1px inset;
}
```

src/stories/components/atoms/atoms.stories.js
```js
import React from 'react'
import { Button } from './Button'
import { action } from '@storybook/addon-actions'

export default {
  title: 'atoms',
  component: Button,
}

const Template = (args) => <Button {...args} />

export const Primary = Template.bind({})
Primary.args = {
  type: 'primary',
  text: 'Button1',
  onClick: action('clicked'),
}

export const Secondary = Template.bind({})
Secondary.args = {
  type: 'secondary',
  text: 'Button2',
  onClick: action('clicked'),
}
```

![[Pasted image 20210715175721.png]]

![[Pasted image 20210715175735.png]]

## オレオレカタログがどこかにあればいいかも

トランジションとかも含められるのかな。

CSS で面白そうなの集めてみたりして。
