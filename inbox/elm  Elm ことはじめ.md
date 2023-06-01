#elm

---
2021-08-09

# Elm ことはじめ


## repl と ファイル

```shell
$ mkdir sample
$ cd sample
$ elm init
```

Elm ファイルの内容を reple で実行するには

src/Main.elm
```elm
module Main exposing (add, output)

output =
	"1 + 1 = " ++ String.fromInt (add 1 1)

add a b =
	a + b
```

```shell
$ elm repl
> import Main

> Main.output
"1 + 1 = 2"

> Main.add 2 3
5
```

## コンパイル

```shell
$ mkdir html
$ cd html
$ elm init
```

src/Main.elm

```elm
module Main exposing (main)

import Html exposing (Html, a, text)
import Html.Attributes exposing (href)


main : Html msg
main =
    a [ href "https://elm-lang.org" ] [ text "Elm" ]

```

vscode で terminal を出すには、ctrl + shift + ^

```shell
$ elm make src/Main.elm
```

index.html が作られる。

### js ファイルにする

jsファイル script.js を作るコンパイル

```shell
$ elm make src/Main.elm --output script.js
```

index.html

```html
<div id="main" class="main"></div>
<script src="script.js"></script>
<script>
	Elm.Main.init({
		mode: document.getElementById('main')
	})
</script>
```


## Elm アーキテクチャ

src/Main.elm

```elm
module Main exposing (main)

import Browser
import Dict exposing (update)
import Html exposing (Html, button, div, text)
import Html.Events exposing (onClick)


main : Program () Model Msg
main =
    Browser.sandbox
        { init = init
        , update = update
        , view = view
        }



-- MODEL


type alias Model =
    Int


init : Model
init =
    0



--UPDATE


type Msg
    = Increment
    | Decrement


update : Msg -> Model -> Model
update msg model =
    case msg of
        Increment ->
            model + 1

        Decrement ->
            model - 1



--VIEW


view : Model -> Html Msg
view model =
    div []
        [ button [ onClick Decrement ] [ text "-" ]
        , div [] [ text (String.fromInt model) ]
        , button [ onClick Increment ] [ text "+" ]
        ]
```

デバッガーオプションで make すると

```shell
$ elm make src/Main.elm --debug
```

Elm アーキテクチャに特化したデバッガがポップアップ表示される。

![[Pasted image 20210810112547.png]]


