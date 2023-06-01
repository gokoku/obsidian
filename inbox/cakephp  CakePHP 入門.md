	#php/cakephp

---
2022-02-20  22:30

# cakephp  CakePHP 入門


<div class="rich-link-card-container"><a class="rich-link-card" href="https://proengineer.internous.co.jp/content/columnfeature/17783" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://proengineer.internous.co.jp/common/images/favicons/android-chrome-192x192.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">【CakePHP入門】インストール方法・使い方・学習方法全まとめ！ | サービス | プロエンジニア</h1>
		<p class="rich-link-card-description">
		「CakePHP」は、PHPで開発するときに覚えておくと便利なフレームワークです。CakePHPの入門知識、基本的な使い方、Windows／MacなどのOSに合わせたインストール方法を解説します。【フリーランスエンジニア案件情報 | プロエンジニア】
		</p>
		<p class="rich-link-href">
		https://proengineer.internous.co.jp/content/columnfeature/17783
		</p>
	</div>
</a></div>

```shell
$ brew install composer

$ composer create-project --prefer-dist cakephp/app:~4.0 sample

$ cd sample
```

```shell
$ bin/cake server
```

http://localhost:8765

![[Pasted image 20220220223211.png]]

## controller

新規ファイル
```php:src/Controller/TestController.php
<?php
namespace App\Controller;

class TestController extends AppController {
    public function index() {
        $this->set('title', '初めてのCakePHPプログラミング');
        $this->set('name', 'George Tada');
    }

}
?>
```

### view

templates に書くようだ。(cakePHP 4)

```php:templates/Test/index.php
<div>
  <h1><?= $title ?></h1>
  <p>My name is <?= $name ?></p>
</div>
```

![[Pasted image 20220220224727.png]]

ここでは routing を何にも設定していない。

では、モデルはどうするのだ?

### Model

cakePHP 4 では config/app_local.php に設定してるようだ。

試しに DB の方にデータベース作るかな。