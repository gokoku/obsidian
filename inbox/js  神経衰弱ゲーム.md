#js

---
2021-10-14

# 神経衰弱ゲーム

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <title>Memory Game</title>
  <link rel="stylesheet" href="style.css"></link>
  <script src="app.js" charset="utf-8"></script>
</head>
<body>
  <h3>Score:<span id="result"></span></h3>
  <div class="grid"></div>
</body>
</html>
```


```css
.grid {
  display: flex;
  flex-wrap: wrap;
  width: 400px;
  height: 300px;
}
```


```js
document.addEventListener('DOMContentLoaded', () => {
  //card options
  const cardArray = [
    {
      name: 'fries',
      img: 'images/fries.png',
    },
    {
      name: 'cheeseburger',
      img: 'images/cheeseburger.png',
    },
    {
      name: 'ice-cream',
      img: 'images/ice-cream.png',
    },
    {
      name: 'pizza',
      img: 'images/pizza.png',
    },
    {
      name: 'miklshake',
      img: 'images/milkshake.png',
    },
    {
      name: 'hotdog',
      img: 'images/hotdog.png',
    },
    {
      name: 'fries',
      img: 'images/fries.png',
    },
    {
      name: 'cheeseburger',
      img: 'images/cheeseburger.png',
    },
    {
      name: 'ice-cream',
      img: 'images/ice-cream.png',
    },
    {
      name: 'pizza',
      img: 'images/pizza.png',
    },
    {
      name: 'miklshake',
      img: 'images/milkshake.png',
    },
    {
      name: 'hotdog',
      img: 'images/hotdog.png',
    },
  ]

  cardArray.sort(() => 0.5 - Math.random())

  const grid = document.querySelector('.grid')
  const resultDisplay = document.querySelector('#t')
  var cardsChosen = []
  var cardsWon = []

  //create your board
  function createBoard() {
    cardArray.forEach((card, index) => {
      var elm = document.createElement('img')
      elm.setAttribute('src', 'images/blank.png')
      elm.setAttribute('data-id', index)
      elm.setAttribute('data-name', card.name)
      elm.setAttribute('data-img', card.img)
      elm.addEventListener('click', flipCard)
      grid.appendChild(elm)
    })
  }

  function openBoard() {
    const cards = document.querySelectorAll('img')
    cards.forEach((card, index) => {
      card.setAttribute('src', cardArray[index].img)
    })
  }

  function closeBoard() {
    const cards = document.querySelectorAll('img')
    cards.forEach((card) => {
      card.setAttribute('src', 'images/blank.png')
    })
  }

  //check for matches
  function checkForMatch() {
    var cards = document.querySelectorAll('img')
    const optionOneId = cardsChosen[0].id
    const optionTwoId = cardsChosen[1].id

    if (optionOneId == optionTwoId) {
      cards[optionOneId].setAttribute('src', 'images/blank.png')
      cards[optionTwoId].setAttribute('src', 'images/blank.png')
      alert('You have clicked the same image!')
    } else if (cardsChosen[0].name === cardsChosen[1].name) {
      cards[optionOneId].setAttribute('src', 'images/white.png')
      cards[optionTwoId].setAttribute('src', 'images/white.png')
      cards[optionOneId].removeEventListener('click', flipCard)
      cards[optionTwoId].removeEventListener('click', flipCard)
      cardsWon.push(cardsChosen)
    } else {
      cards[optionOneId].setAttribute('src', 'images/blank.png')
      cards[optionTwoId].setAttribute('src', 'images/blank.png')
    }
    cardsChosen = []
    resultDisplay.textContent = cardsWon.length
    if (cardsWon.length === cardArray.length / 2) {
      resultDisplay.textContent = 'Congratulations! You found them all!'
    }
  }

  //flip your card
  function flipCard() {
    var cardId = this.getAttribute('data-id')
    const cardName = this.getAttribute('data-name')
    const cardImg = this.getAttribute('data-img')
    cardsChosen.push({ id: cardId, name: cardName })
    this.setAttribute('src', cardImg)
    if (cardsChosen.length === 2) {
      setTimeout(checkForMatch, 1000)
    }
  }
  createBoard()
  openBoard()
  setTimeout(closeBoard, 3000)
})

```

![[images.zip]]