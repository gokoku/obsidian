#centos #vmware 

---
2022-01-26  15:10

# centos  SSH のタイムアウト時間を解除する

vmware で ssh 接続で MySQL に接続してて、コネクトが切断されて困る。

vmware で centos 6 を立ち上げているので、設定は


<div class="rich-link-card-container"><a class="rich-link-card" href="https://onoredekaiketsu.com/prevent-ssh-connection-timeout/" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://onoredekaiketsu.com/wp-content/uploads/2019/01/prevent-ssh-connection-timeout.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">SSH接続がタイムアウトで自動的に切断されないようにする</h1>
		<p class="rich-link-card-description">
		SSHでAWSやconohaのVPSに接続している際に、一定期間何もしないとタイムアウトで自動的に接続が切れる。そのたびに何度も再接続するのは手間が掛かるので自動的に切断されないようにする設定です。自動切断の原因もともとSSHは接続を持続す
		</p>
		<p class="rich-link-href">
		https://onoredekaiketsu.com/prevent-ssh-connection-timeout/
		</p>
	</div>
</a></div>

```shell:/etc/ssh/sshd_config
ClientAliveInterval 120
ClientAliveCountMax 3
```

-   「ClientAliveInterval」：ここで指定した秒数ごとに応答確認を行う。
-   「ClientAliveCountMax 」：応答確認を行う回数。

デフォルトでは応答確認しないらしい。( ClientAliveInterval 0)
クライアントから切断しない限りタイムアウトにならないとのこと。

もし、ネットワークが切断されてしまった場合に、3回 (ClientAliveCountMax 3) 繰り返して
応答を確認するので、ゴーストユーザーが残っても切断してくれて安心とのこと。

centos 6 の sshd のリスタート

```shell
$ sudo /etc/rc.d/init.d/sshd restart

$ sudo service sshd restart
```

centos 7 からは

```shell
$ sudo systemctl restart sshd.service
```


