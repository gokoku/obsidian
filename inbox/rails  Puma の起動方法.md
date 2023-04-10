---
type: note
---

#rails

---
2022-09-30  10:55

# rails  Puma の起動方法

## rails コマンド

```shell
開発
$ rails s

本番
$ rails s -e production
$ rails s RAILS_ENV=production

### ENV[RACK_ENV] = 'production'
$ rails s
```

## puma コマンド
```shell
開発
$ puma

本番
### ENV[RACK_ENV] = 'production'
$ puma
$ RACK_ENV='production' puma

停止は rails プロジェクトの tmpディレクトリの中の pid を kill する
$ kill $(cat tmp/pids/server.pid)
```




## pumactl コマンド
```shell
開発
$ pumactl start


本番
### ENV[RACK_ENV] = 'production'
$ pumactl start
$ RACK_ENV='production' pumactl start

$ pumactl stop
```