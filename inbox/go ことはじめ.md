#go


## Hello world

$GOPATH がデフォルトで ~/go/1.14.4 になってるので、ここを作業ディレクトリとする。

```shell
$ mkdir -p $GOPATH/src/helloworld
$ ln -s $GOPATH ~/Desktop/go_1_14_4
```

    └── src
        └── helloworld
            └── main.go
            
 ### VSCode の Go extentions を入れた。



main.go

```go
package main

import "fmt"

func main() {
	fmt.Pringln("Hello, world!")
}
```

これ書いてるうちにもガンガンモジュールを自動的にインストールされる。

### 実行

src の中を見てくれるようだ。

```go
$ cd $GOPATH
$ go run helloworld
Hello, world
```

### ビルド

```shell
$ cd $GOPATH
$ go build helloworld
$ ./helloworld
Hello, world
```

