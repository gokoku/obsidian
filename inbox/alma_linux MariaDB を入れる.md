---
type: note
---

#alma_linux 

---
2023-04-24  16:36

# alma_linux MariaDB を入れる


<div class="rich-link-card-container"><a class="rich-link-card" href="https://syutaku.blog/mariadb-10-8-install-almalinux-8-6/" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('http://syutaku.blog/wp-content/uploads/2021/09/NoImage_03.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">【MariaDB】Ver10.8 インストール手順 | AlmaLinux8.6向け</h1>
		<p class="rich-link-card-description">
		AlmaLinux8.6環境に、MariaDBのバージョン10.8をインストールする方法を解説します。
		</p>
		<p class="rich-link-href">
		https://syutaku.blog/mariadb-10-8-install-almalinux-8-6/
		</p>
	</div>
</a></div>



### インストール

/etc/yum.repos.d/mariaDB.repo

```shell
# MariaDB 10.10 RedHat repository list - created 2023-02-01 05:34 UTC
# https://mariadb.org/download/
[mariadb]
name = MariaDB
baseurl = https://ftp.yz.yamagata-u.ac.jp/pub/dbms/mariadb/yum/10.10/rhel8-amd64
module_hotfixes=1
gpgkey=https://ftp.yz.yamagata-u.ac.jp/pub/dbms/mariadb/yum/RPM-GPG-KEY-MariaDB
gpgcheck=1
enabled=1
```

```shell
$ dnf list MariaDB*
一式あるのを確認出来るか

$ dnf install -y MariaDB-client MariaDB-devel MariaDB-server

インストールパッケージの確認
$ dnf list installed | grep mariadb


インストール場所の確認
$ which mysqld
/usr/sbin/mysqld


起動する
$ sudo systemctl status mariadb
$ sudo systemctl start mariadb

自動起動の設定
$ sudo systemctl enable mariadb

確認
$ sudo systemctl is-enabled mariadb
enabled
```

### 初期設定

```shell
$ sudo mariadb-secure-installation

現在のrootパスワードの入力だけど、インストール直後は空なので、
Enter current password for root (enter for none):まんまリターン!!

unix_socket認証に切り替える
Switch to unix_socket authentication[Y/n]y

root バスワードの変更をする?
n
...skipping.

匿名ユーザーを削除しますか?
Remove anonymous users?[Y/n]y

rootユーザーでのリモートログインを禁止しますか?
y

テスト用のデータベースを削除しますか?
y

今すぐ設定を反映しますか?
Reload privilege tables now?[Y/n]y

Thanks for using MariaDB!
```
### 文字コードの設定

/etc/my.cnf.d/server.cnf
```
[server]
character-set-server=utf8
```
追加する。
```shell
$ sudo systemctl restart mariadb
```


### ユーザーを作る

```shell
$ mysql -uroot -p

> create user 'gtfsuser'@'localhost' identified by 'pmorioka7481'

> create database gtfs_production;
> grant all privileges on gtfs_production.* to 'gtfsuser'@'localhost';
```


