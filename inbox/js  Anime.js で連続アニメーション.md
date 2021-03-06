#js/anime 

---
2021-08-06

# Anime.js の連続アニメーション

https://naruhodo.repop.jp/anime-js-continuous-animation/

## Delay で調整する方法

```js

  anime({
    targets: [door_t],
	...
    duration: 2000,
    delay: 0,
  })
  anime({
    targets: [door_l],
	...
    duration: 2000,
    delay: 2000,
  })
  anime({
    targets: [door_r],
	...
    duration: 2000,
    delay: 4000,
  })
}
```

## Keyframes で連続させる方法

```js
  anime({
    targets: '#t',
    loop: true,
    translateX: [
    { value: 150, duration: 1000, delay: 300, elasticity: 0 },
    { value: 300, duration: 1000, delay: 300, elasticity: 0 },
    { value: 0, duration: 1000, delay: 300, elasticity: 0 }
    ]
  });
```

## Timeline で連続させる方法

```js
const animationName = anime.timeline({
 loop: true,
});

animationName
  .add({
    targets: '#t',
    translateX: 150,
    elasticity: 0,
  })
  .add({
    targets: '#t',
    translateX: 300,
    elasticity: 0,
  })
  .add({
    targets: '#t',
    translateX: 0,
    elasticity: 0,
  });
```

