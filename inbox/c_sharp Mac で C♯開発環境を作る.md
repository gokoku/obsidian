#c_sharp

---
2021-10-04

# C♯ 開発環境を作る

MacVisualStudio があるとのこと。

Mono/.NetCore が .Net Framework の Unix Linux 版とのこと。

Visual Studio for Mac のインストール

brew で入れるのにオプションが必要だったのだろうか。なんかあれ? ってことで、ダウンロードしたら

![[Pasted image 20211004172157.png]]

ここで全入れにしよう。

Xqmarin も入る。Unity が云々と出てる。

https://visualstudio.microsoft.com/ja/thank-you-downloading-visual-studio-mac/?sku=communitymac&rel=16

無料版とあって安心する。

![[Pasted image 20211004174103.png]]

homebrew cask でおとしたのよりいろいろあってほっとする。

.NET Core SDK がほしい。

```shell
$ brew install --cask dotnet

$ dotnet --version
5.0.401
```

最新のようだ。

https://qiita.com/cannorin/items/59d79cc9a3b64c761cd4

## Helloworld

```shell
$ dotnet new console -lang="F#" -o helloworld

$ cd helloworld
```

Program.fs が F# code

helloworld.fsproj メタデータとか色々設定系??

Program.fs

```c
[<EntryPoint>]
let main argv =
    printfn "Hello world from F#!"
    0 // return an integer exit code
```

```shell
$ dotnet run
Hello world from F#!
```
