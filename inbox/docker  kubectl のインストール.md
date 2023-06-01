#docker

https://kubernetes.io/ja/docs/tasks/tools/install-kubectl/


---
## mac Homebrew

```shell
$ brew install kubectl　　　#error URLが 404 になってる

$ brew install kubernetes-cli

あるいは

$ curl -LO "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/darwin/amd64/kubectl"

$ chmod +x ./kubectl
$ sudo mv ./kubectl /usr/local/bin/kubectl
```


## Ubuntu Debian
```shell
sudo apt-get update && sudo apt-get install -y apt-transport-https gnupg2
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update
sudo apt-get install -y kubectl
```

手動でも入れられる。
```shell
$ curl -LO "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl"


$ chmod +x ./kubectl
$ sudo mv ./kubectl /usr/local/bin/kubectl

$ kubectl version --client
```


---
自動補完機能

```shell
zsh

# ~/.zshrc
source <(kubectl completion zsh)


```