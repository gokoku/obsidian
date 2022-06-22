---
type: note
---

#kubernetes

---
2022-05-12  15:32

# Kubernetesの動かしかた

https://codezine.jp/article/detail/15828?utm_source=codezine_regular_20220511
<div class="rich-link-card-container"><a class="rich-link-card" href="https://codezine.jp/article/detail/15828?utm_source=codezine_regular_20220511" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://codezine.jp/static/images/article/15828/15828_og.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">イラストではじめる「Kubernetesの動かしかた」～Kubernetesクラスタを用意し、Podを作ってコンテナを起動しよう</h1>
		<p class="rich-link-card-description">
		本連載ではKubernetesの簡単な説明からはじまり、開発者の方にとってKubernetesを利用することで何が嬉しいのか、どのように開発フローが変わっていくのかについて、イラストを交えながら紹介します。今回は、実際にKubernetesを触ってみる方法をご紹介します。まだまだ理解が怪しいと思っていても、触ってみると知識が深まることもあるでしょう。特にローカルクラスタを作ってさえしまえば、いくら壊しても大丈夫です。
		</p>
		<p class="rich-link-href">
		https://codezine.jp/article/detail/15828?utm_source=codezine_regular_20220511
		</p>
	</div>
</a></div>

chromebook pop_os でやってみたいから、minikube か kind か。

### kind で行きます

```shell
$ brew install kubectl
$ brew install kind

# Kubernetes 起動
$ kind create cluster

$ kubectl cluster-info --context kind-kind

# Kubernetes 削除
$ kind delete cluster
```

### kubectl を使って cluster に access

```shell
$ kubelctl get pod -A

Pod がズラズラ
```

### Pod を作成する

manifest を作成する。

pod.yaml

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
	containers:
	- name: nginx
	  image: nginx: 1.14.2
	  ports:
	  - containerPort: 80
```
#### Pod 作成
```shell
$ kubectl apply -f pod.yaml
$ kubectl get po

nginx Running
```

#### Pod 削除

```shell
$ kubectl delete -f pod.yaml
or
$ kubectl delete pod nginx

$ kubectl get po
No resources fond in default namespace.
```


### Namespace 指定に気をつけよう
`kubectl -n <namespace>`
  or
マニフェストに Namespace を記述


## Deployment 作成する

![[Pasted image 20220512173737.png]]

pod.yaml

```shell
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
EOF
```

```shell
$ kubectl apply -f pod.yaml
deployment.apps/nginx-deployement created

$ kubectl get development
3つ出来てる。

$ kubectl get pod

$ kubectl delete pod <pod name>
一つ消す

$ kubectl get pod

でも、すぐさま3つになる

$ kubectl delete -f pod.yaml
```

## Service を作成する

pod2.yaml

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  type: NodePort
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port:: 8080
      targetPort: 80
```

```shell
$ kubectl apply -f pod2.yaml

$ kubectl describe service my-service

NodePort:  32008/TCP
Endpoints: <none>
```

あれ?
アクセス出来ん。
Endpoints がない

### Debug
```shell
$ kubectl get endpoints my-service
```
ENDPOINTS が <none> だ

分からん。理解不足。後ほど学びます。
