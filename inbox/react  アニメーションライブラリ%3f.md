#js/react 

---
2021-06-28

# https://codesandbox.io/s/react-waapi-library-sketch-wh09l?file=/src/App.js:0-858

```js
import "./styles.css";

import { animated } from "motion";

  

export default function App() {

return (

<animated.div

// Styles to use on the first render

first={{ opacity: 0, transform: "scale(0.5)" }}

// Any time a value in style changes, it will animate to it

style={{ opacity: 1, transform: "none" }}

// Animation options - maybe it'd be cool to have "firstOptions" too

options={{

duration: 1,

easing: [0.44, 0.03, 0.15, 1.27]

// delay: 1

// repeat: Infinity

}}

// Gesture animations - TODO options for each

hover={{ transform: "scale(1.2)" }}

press={{ transform: "scale(0.8)" }}

// Animation listeners

onStart={() => console.log("animation started")}

onComplete={() => console.log("animation completed")}

className="box"

/>

);

}
```