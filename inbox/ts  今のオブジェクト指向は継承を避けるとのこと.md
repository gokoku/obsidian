#ts

---
2021-08-23

# 今のオブジェクト指向は継承を避けるとのこと

## 継承
```ts
class Rectangle {
  name = 'rectangle'
  sideA: number
  sideB: number

  constructor(sideA: number, sideB: number) {
    this.sideA = sideA
    this.sideB = sideB
  }

  getArea = (): number => this.sideA + this.sideB
}

class Square extends Rectangle {
  readonly name = 'square'
  side: number

  constructor(side: number) {
    super(side, side)
  }
}

const s: Square = new Square(3)

console.log(s.name, s.getArea())
```

![[Pasted image 20210823134303.png]]

隠れインスタンスや隠れメソッドが生じたりして厄介なことになったり。

かと言って private や protected や public でコントロールするのもめんどくさい。

作るときは調子いいが、想像以上にメンテが難しくなりがち。

実は可読性が悪い。


## 合成

```ts
class Rectangle {
  readonly name = 'rectangle'
  sideA: number
  sideB: number

  constructor(sideA: number, sideB: number) {
    this.sideA = sideA
    this.sideB = sideB
  }

  getArea = (): number => this.sideA + this.sideB
}

class Square {
  readonly name = 'square'
  side: number

  constructor(side: number) {
    this.side = side
  }

  getArea = (): number => new Rectangle(this.side, this.side).getArea()
}

```

![[Pasted image 20210823135812.png]]

継承の代わりに、合成 composition を使う。

親と子の分離性の良さが、可読性の良さになる。