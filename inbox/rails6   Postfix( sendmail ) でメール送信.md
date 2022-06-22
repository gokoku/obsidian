#ruby/rails 


<https://easyramble.com/action-mailer-with-postfix.html>

サーバで Mail コマンドが使えることを確認した。

config/environments/production.rb

```ruby
  server_name = "kanegasaki.iwate.rabipeoca.jp"
  config.hosts << server_name

  config.force_ssl = true

  config.logger = Logger.new('log/production.log', 'daily')

  config.action_mailer.default_url_options = { host: server_name }
  config.action_mailer.delivery_method = :smtp
  config.action_mailer.smtp_settings = {
    :enable_starttls_auto => true,
    :address => "localhost",
    :port => 25,2
    :authentication => 'plain'
  }
```

```shell
$ sudo passenger-config restart httpd
```

default_url_options の :host はドメイン名。
smtp_settings の :address は　localhost
