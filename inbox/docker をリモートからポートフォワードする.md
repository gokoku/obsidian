---
type: note
---

#docker

---
2022-06-06  14:29

# docker をリモートからポートフォワードする

https://www.yasunaga-lab.bio.kyutech.ac.jp/EosJ/index.php/%E3%83%AA%E3%83%A2%E3%83%BC%E3%83%88%E3%82%B5%E3%83%BC%E3%83%90%E3%83%BC%E3%81%AE%E4%B8%AD%E3%81%AEDocker%E3%81%AB%E3%83%AD%E3%83%BC%E3%82%AB%E3%83%AB%E3%81%8B%E3%82%89%E6%8E%A5%E7%B6%9A%E3%81%99%E3%82%8B

pop-os に docker.sock がある
`/var/run/docker.sock`

これをクライアントから ssh でポートフォワードすると、クライアントであたかも Docker が動いてる感じになる。

```shell
$ ssh -fNL localhost:9999:/var/run/docker.sock george@pop-os.local
```

-f はバックグラウンドにするやつ。やってること忘れるからめんどくさくてあまりやらない。

-N はポートフォワーディング指定
-L で、クライアント : リモート　という感じで設定。ここでは ソケットを指定。

残り : リモートの接続先

---
### DOCKER_HOST 環境変数を設定


リモートで立ち上げる。
```shell
$ docker compose run --rm ex_book1 /bin/bash
```

クライアントからぽートフォワーディングする。
```shell
$ ssh -NL localhost:9999:/var/run/docker.sock george@pop-os.local
```

```shell
$ export DOCKER_HOST=localhost:9999
$ docker ps
CONTAINER ID   IMAGE               COMMAND       CREATED          STATUS          PORTS     NAMES
668de1866c30   ex_book1_ex_book1   "/bin/bash"   31 minutes ago   Up 31 minutes             ex_book1_ex_book1_run_9ddb8c860c03
```

マジかよ!! まるでdocker がクライアントマシンで起動してるみたいじゃないか。

```shell
$ docker attach 668de
```

...でも、ssh で入って弄るのが手順が少ないかも。
