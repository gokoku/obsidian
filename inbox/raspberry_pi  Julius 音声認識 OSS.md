#raspberry_pi 

https://julius.osdn.jp/

https://github.com/julius-speech/julius

```shell
$ sudo apt-get install build-essential zlib1g-dev libsdl2-dev libasound2-dev
$ git clone https://github.com/julius-speech/julius.git
$ cd julius
$ ./configure --enable-words-int
$ make -j4
$ ls -l julius/julius

\-rwxr-xr-x 1 ri lab 746056 May 26 13:01 julius/julius
```

```shell
$ julius/julius -C julius-kit/dictation-kit-4.5/main.jconf -C julius-kit/dictation-kit-4.5/am-gmm.jconf -nostrip
```

なんだかめちゃくちゃに認識されている。

ちゃんとモデル用意しないとだめなのかな。
てゆーか、ここからだと果てしない。


