---
type: note
---

#alma_linux #docker 

---
2022-06-08  13:27

# alma_linux. Docker をインストールする

```shell
$ sudo dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

確認
$ dnf repolist

docker-ce-stable


$ sudo dnf -y install docker-ce docker-ce-cli containerd.io docker-compose-plugin

$ docker --version
Docker version 20.10.17, build ...
```

起動
```shell
$ sudo systemctl start docker
```

自動起動の設定
```shell
sudo systemctl enable docker
```

### sudo なしで動くようにする

```shell
$ sudo gpasswd -a george docker

$ groups george
george : george wheel docker
	```

logout && login

```shell
$ docker ps
```

OK!!

