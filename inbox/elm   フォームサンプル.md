#elm

---
2021-08-11

# フォームサンプル

```shell
$ mkdir form
$ cd form
$ elm init
```

src/Main.elm

```elm
module Main exposing (Model, Msg(..), init, main, update, view)

import Browser
import Html exposing (..)
import Html.Attributes exposing (..)
import Html.Events exposing (..)


main : Program () Model Msg
main =
    Browser.sandbox
        { init = init
        , view = view
        , update = update
        }
```

```elm
type alias Model =
    { input : String
    , memos : List String
    }


init : Model
init =
    { input = ""
    , memos = []
    }
```

```elm

type Msg
    = Input String
    | Submit


update : Msg -> Model -> Model
update msg model =
    case msg of
        Input input ->
            { model | input = input }

        Submit ->
            { model
                | input = ""
                , memos = model.input :: model.memos
            }
```

```elm
view : Model -> Html Msg
view model =
    div []
        [ Html.form [ onSubmit Submit ]
            [ input [ value model.input, onInput Input ] []
            , button [ disabled (String.length model.input < 1) ] [ text "Submit" ]
            ]
        , ul [] (List.map viewMemo model.memos)
        ]


viewMemo : String -> Html Msg
viewMemo memo =
    li [] [ text memo ]
```

![[Pasted image 20210811092657.png]]

## ピンポイント

```elm
 model.input :: model.memos
 ```
 
 ### :: は?
 
 リストに追加するやつ。
 
 ```elm
> 1 :: [2, 3, 4]
[1, 2, 3, 4]
```
 
 
 ```elm
  [ input [ value model.input, onInput Input ] []
  ```
  
  ### onInput とは?

https://package.elm-lang.org/packages/elm/html/latest/Html-Events

Html.Events モジュール

```elm
onInput : (String -> msg) -> Attribute msg

```
入力文字列を msg に変換する関数を受け取っている。

Input は type Msg でカスタムタイプで、コンストラクタも作られている。

update で model に input の更新した値が入ったものが入る。

Input は String -> Msg であるとわかる。

onInput (\s -> Input s) でもOK。




