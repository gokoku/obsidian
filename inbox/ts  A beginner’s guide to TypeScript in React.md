#ts

---
2022-03-07  09:21

# ts  A beginner’s guide to TypeScript in React


<div class="rich-link-card-container"><a class="rich-link-card" href="https://medium.com/@joan728728/a-beginners-guide-to-typescript-in-react-3afa6a0e7f53" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">A beginner’s guide to TypeScript in React</h1>
		<p class="rich-link-card-description">
		1. Build a new react project with typescript
		</p>
		<p class="rich-link-href">
		https://medium.com/@joan728728/a-beginners-guide-to-typescript-in-react-3afa6a0e7f53
		</p>
	</div>
</a></div>


```shell
$ mkdir beginner-guide
$ cd beginner-guide
$ npx create-react-app . --template typescript

$ npm i
$ npm start
```




<div class="rich-link-card-container"><a class="rich-link-card" href="https://www.youtube.com/watch?v=1jMJDbq7ZX4" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://www.youtube.com/embed/1jMJDbq7ZX4?feature=oembed')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">TypeScript Course In ReactJS - TypeScript</h1>
		<p class="rich-link-card-description">
		In this video I will go over everything I thing a beginner should learn on how to use Typescript in React.-❤️ Want to Support the Channel?:https://www.youtub...
		</p>
		<p class="rich-link-href">
		https://www.youtube.com/watch?v=1jMJDbq7ZX4
		</p>
	</div>
</a></div>




このvideoのまとめだった。

### 大体のコード
![[Pasted image 20220307104410.png]]

```ts:App.tsx
import React, {FC, createContext} from 'react';
import './App.css'
import {Person} from './components/Person';
import { HairColor } from './Enums';
interface AppContextinterface {
  name: string
  age: number
  country: string
}

const AppContext = createContext<AppContextinterface | null>(null)

const App: FC = () => {
  const contextValue: AppContextinterface = {
    name: "John",
    age: 30,
    country: "USA"
  }

  return (
    <AppContext.Provider value={contextValue}>
      <div className="App">
        <Person
          name={contextValue.name}
          age={20}
          email="pedro@example.com"
          hairColor={HairColor.Blonde}
        />
      </div>
    </AppContext.Provider>
  );
}

export default App;
```

```ts:Enums.ts
export enum HairColor {
  Blonde = "Your hair is blonde, good for you",
  Brown = "Cool hair color",
  Pink = "Wow that is so cool",
}
```

```ts:Person.tsx
import {ChangeEvent, FC, useState} from 'react';
import {User} from '../Interfaces'


export const Person: FC<User> = ({name, email, age, hairColor}) =>{

  const [country, setCountry] = useState<string | null>("")

  const handleChange = (event: ChangeEvent<HTMLInputElement>) => {
    setCountry(event.target.value)
  }
  return (
    <div>
      <h1>{name}</h1>
      <h1>{email}</h1>
      <h1>{age}</h1>
      <input placeholder="Write down your country..." onChange={handleChange}/>

      {country}
      {hairColor}
    </div>
  );
}
```


```ts:Interfaces.ts
import {HairColor} from "./Enums";

export interface User {
  name: string
  age: number
  email: string
  hairColor: HairColor
}
```