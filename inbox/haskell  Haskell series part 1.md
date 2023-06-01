#haskell

---
2021-07-13

# Haskell series part1

https://blog.kalvad.com/haskell-series-part-1/

[[mac asdf-vm にする | haskell の install]] 

```shell
$ ghci
ghci> :t ['a', 'b', 'c']
型がわかる。

String と [Char] は一緒のようだ。

ghci> ['a','b','c'] == "abc"
```

haskell のファイルを書く。

fn.hs
```haskell
powerOfTwo x = powerOfX x 2
powerOfThree x = powerOfX x 3
powerOfX x y = x ** y

main = do
  putStrLn "Let's calculate some power !"
  print $ powerOfX 2 2

```
読み込む。
```shell
$ ghci
ghci> :l fn.hs     
ghci> poserOfTwo 4
16.0

ghci> :r  再読み込み
```

コンパイル。

```shell
$ ghc --make fn.hs

$ ./hn
Let's calculate some power !
4.0
```