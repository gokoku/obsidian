#notion 

---
2022-04-19  16:39

# notion  Notion で Gmail を飛ばせるか

Notion developer で login する。
Notion でぴーぷるアカウント作ってlogin してたので解除するにはそこで、login して Logout すると、全てLogout になってめでたく自分アカウントログイン出来た。

one password で認証。

https://www.notion.so/my-integrations

![[Pasted image 20220419165620.png]]

新しいインテグレーションを作成をクリック。

![[Pasted image 20220419165711.png]]

送信
![[Pasted image 20220419165819.png]]

トークン取得。

![[Pasted image 20220419175901.png]]

これでいいみたいだ。

### zipier の設定
https://zapier.com/apps/notion/integrations

![[Pasted image 20220419170029.png]]

Connect Notion クリックした。

Notion に接続を許可。

![[Pasted image 20220419183522.png | 400]]

データベース毎に Share を設定しないと見つけてくれないようだ。

![[Pasted image 20220419184123.png]]

Gmail サイド

![[Pasted image 20220419184400.png]]

send mail しか上手くいってない。

![[Pasted image 20220419184446.png]]

![[Pasted image 20220419184545.png]]

テストうまくいった。

![[Pasted image 20220419184630.png]]

ON にした。

Notion にアイテムを追加したら、

すごくタイムラグがある。

![[Pasted image 20220419190811.png]]

これは追加したアイテムの名前カラムを subject と body にして、宛先を email のカラムから撮ったものを投げてるのがわかった。

![[Pasted image 20220419191017.png]]

Gmail の設定のところでアイテムが 1 番目のやつしか選べてなかったので、不安だったが、ちゃんとカレントなアイテムのカラムから取得してた。

Good !






