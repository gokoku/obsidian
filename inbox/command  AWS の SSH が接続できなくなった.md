#command #aws

---
2022-03-18  15:32

# command  AWS の SSH が接続できなくなった

AWSコンソール から ec2 インスタンスに入ってアクションから接続で接続した。

ポートを調べる。
```shell

$ ss -at

State   Recv-Q  Send-Q   Local Address:Port          Peer Address:Port Process  
LISTEN  0       80           127.0.0.1:mysql              0.0.0.0:*             
LISTEN  0       100            0.0.0.0:submission         0.0.0.0:*             
LISTEN  0       500          127.0.0.1:46157              0.0.0.0:*             
LISTEN  0       100            0.0.0.0:pop3               0.0.0.0:*             
LISTEN  0       100            0.0.0.0:imap2              0.0.0.0:*             
LISTEN  0       100            0.0.0.0:submissions        0.0.0.0:*             
LISTEN  0       4096     127.0.0.53%lo:domain             0.0.0.0:*             
LISTEN  0       128            0.0.0.0:ssh                0.0.0.0:*             
LISTEN  0       100            0.0.0.0:smtp               0.0.0.0:*             
LISTEN  0       500          127.0.0.1:46207              0.0.0.0:*             
LISTEN  0       100            0.0.0.0:imaps              0.0.0.0:*             
LISTEN  0       128            0.0.0.0:60322              0.0.0.0:*             
LISTEN  0       100            0.0.0.0:pop3s              0.0.0.0:*             
ESTAB   0       0        172.31.35.133:60322           118.6.85.0:1912          
ESTAB   0       0        172.31.35.133:60322           118.6.85.0:1918          
ESTAB   0       0        172.31.35.133:60322           118.6.85.0:1905          
ESTAB   0       0        172.31.35.133:60322           118.6.85.0:1908          
ESTAB   0       0        172.31.35.133:60322           118.6.85.0:1909          
ESTAB   0       0        172.31.35.133:60322           118.6.85.0:1907          
LISTEN  0       100               [::]:submission            [::]:*             
LISTEN  0       100               [::]:pop3                  [::]:*             
LISTEN  0       100               [::]:imap2                 [::]:*             
LISTEN  0       511                  *:http                     *:*             
LISTEN  0       100               [::]:submissions           [::]:*             
LISTEN  0       128               [::]:ssh                   [::]:*             
LISTEN  0       100               [::]:smtp                  [::]:*             
LISTEN  0       511                  *:https                    *:*             
LISTEN  0       100               [::]:imaps                 [::]:*             
LISTEN  0       128               [::]:60322                 [::]:*             
LISTEN  0       100               [::]:pop3s                 [::]:*   

```
サービス名に変換しないで。

```shell
$ ss -atn


State   Recv-Q   Send-Q     Local Address:Port      Peer Address:Port  Process  
LISTEN  0        80             127.0.0.1:3306           0.0.0.0:*              
LISTEN  0        100              0.0.0.0:587            0.0.0.0:*              
LISTEN  0        500            127.0.0.1:46157          0.0.0.0:*              
LISTEN  0        100              0.0.0.0:110            0.0.0.0:*              
LISTEN  0        100              0.0.0.0:143            0.0.0.0:*              
LISTEN  0        100              0.0.0.0:465            0.0.0.0:*              
LISTEN  0        4096       127.0.0.53%lo:53             0.0.0.0:*              
LISTEN  0        128              0.0.0.0:22             0.0.0.0:*              
LISTEN  0        100              0.0.0.0:25             0.0.0.0:*              
LISTEN  0        500            127.0.0.1:46207          0.0.0.0:*              
LISTEN  0        100              0.0.0.0:993            0.0.0.0:*              
LISTEN  0        128              0.0.0.0:60322          0.0.0.0:*              
LISTEN  0        100              0.0.0.0:995            0.0.0.0:*              
ESTAB   0        0          172.31.35.133:60322       118.6.85.0:1912           
ESTAB   0        0          172.31.35.133:60322       118.6.85.0:1918           
ESTAB   0        0          172.31.35.133:60322       118.6.85.0:1905           
ESTAB   0        0          172.31.35.133:60322       118.6.85.0:1908           
ESTAB   0        0          172.31.35.133:60322       118.6.85.0:1909           
ESTAB   0        0          172.31.35.133:60322       118.6.85.0:1907           
LISTEN  0        100                 [::]:587               [::]:*              
LISTEN  0        100                 [::]:110               [::]:*              
LISTEN  0        100                 [::]:143               [::]:*              
LISTEN  0        511                    *:80                   *:*              
LISTEN  0        100                 [::]:465               [::]:*              
LISTEN  0        128                 [::]:22                [::]:*              
LISTEN  0        100                 [::]:25                [::]:*              
LISTEN  0        511                    *:443                  *:*              
LISTEN  0        100                 [::]:993               [::]:*              
LISTEN  0        128                 [::]:60322             [::]:*              
LISTEN  0        100                 [::]:995               [::]:* 
```

ssh が 22 と 60322 で LISTEN してるようだが。

```shell
$ ss -t

State  Recv-Q   Send-Q     Local Address:Port      Peer Address:Port   Process  
ESTAB  0        0          172.31.35.133:60322       118.6.85.0:1912            
ESTAB  0        0          172.31.35.133:60322       118.6.85.0:1918            
ESTAB  0        0          172.31.35.133:60322       118.6.85.0:1905            
ESTAB  0        0          172.31.35.133:60322       118.6.85.0:1908            
ESTAB  0        0          172.31.35.133:60322       118.6.85.0:1909   
```
通信が確立しているポートとのこと。

Local Address:Port がサーバ側
Peer Address:Port が通信している側

なんのこと?


ポートが使われてるかどうか確認。
```shell
$ lsof -i
ruby 160892 ubuntu 7u IPv4 3624205 0t0 TCP localhost:46157 (LISTEN)
``````


sshd のポートを 60322 にしてみる。

```shell
$ sudo emacs /etc/ssh/sshd_config

Port 60322
```

エラー確認。
```shell
$ sudo sshd -t

何も表示されなければOK
```

sshd 再起動。
```shell
$ sudo systemctl status sshd
```

だめ。

## 他の手はないか

```shell
$ nc -vz xxx.xxx.xxx.xxx 6xx22
Connection to ......succeeded!
```

接続は出来てるのか。

Firewall はどうだ? 
```shel
$ sudo ufw status
Status: inactive
```
動いてない。


やりとりしてるんだけど、
```shell
$ ssh -i "aws_key.pem" xxxxxxxx@xxxxxxx.jp -p 6xx22 -v

Timeout, server xxxxx not response
```

ポートスキャンしたら、
```shell
$ nmap xxx.xxx.xxx.xxx
Starting Nmap 7.91 ( https://nmap.org ) at 2022-03-18 18:10 JST

Host is up (0.019s latency).
Not shown: 998 filtered ports
PORT    STATE SERVICE
80/tcp  open  http
443/tcp open  https

Nmap done: 1 IP address (1 host up) scanned in 43.33 seconds
```

なんと開いて無い。

# ~/.ssh/config の先頭に変なのが混じってただけだった

-v のログが読みきれてなかったのか。
