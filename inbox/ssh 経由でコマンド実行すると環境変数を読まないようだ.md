---
type: note
---

#pop_os 

---
2022-06-08  10:00

# ssh 経由でコマンド実行すると環境変数を読まないようだ

https://nobwak.github.io/posts/2013-07-25-freebsdlinux_ssh%E7%B5%8C%E7%94%B1%E3%81%A7%E3%82%B3%E3%83%9E%E3%83%B3%E3%83%89%E5%AE%9F%E8%A1%8C%E3%81%99%E3%82%8B%E3%81%A8%E7%92%B0%E5%A2%83%E5%A4%89%E6%95%B0%E3%82%92%E8%AA%AD%E3%81%BE%E3%81%AA%E3%81%84%E3%81%A7%E3%81%94%E3%81%96%E3%82%8B/

sshd_config で PermitUserEnvironment を yes にするやり方をやってみる。
上手くいかなかった。

.profile を動かしたいだけなのに。

.zshrc に linuxbrew の設定を仕込むことにした。

zsh には OS 毎に設定できる alias が多いとのこと。


<div class="rich-link-card-container"><a class="rich-link-card" href="https://qiita.com/Suzuki09/items/8e36b609508238b79321" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://qiita-user-contents.imgix.net/https%3A%2F%2Fcdn.qiita.com%2Fassets%2Fpublic%2Farticle-ogp-background-9f5428127621718a910c8b63951390ad.png?ixlib=rb-4.0.0&w=1200&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTkxNiZ0eHQ9dmltJUUzJTgxJUE4JUUzJTgxJThCdG11eCVFMyU4MSVBOCVFMyU4MSU4QnpzaCVFMyU4MSVBRU9TJUU1JTg4JUE0JUU1JUFFJTlBJnR4dC1jb2xvcj0lMjMyMTIxMjEmdHh0LWZvbnQ9SGlyYWdpbm8lMjBTYW5zJTIwVzYmdHh0LXNpemU9NTYmdHh0LWNsaXA9ZWxsaXBzaXMmdHh0LWFsaWduPWxlZnQlMkN0b3Amcz01NjczODQzYTg3Njc0ZjRmNzExMzU3NGI4MDBiMzI1Yw&mark-x=142&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTYxNiZ0eHQ9JTQwU3V6dWtpMDkmdHh0LWNvbG9yPSUyMzIxMjEyMSZ0eHQtZm9udD1IaXJhZ2lubyUyMFNhbnMlMjBXNiZ0eHQtc2l6ZT0zNiZ0eHQtYWxpZ249bGVmdCUyQ3RvcCZzPTJjNWQwMjcyOTRhZGVjMWI3ZDM0ZjMzMDg5MDdkMDNj&blend-x=142&blend-y=491&blend-mode=normal&s=8f6d8d5e447cc32d5ba029f92ce44d79')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">vimとかtmuxとかzshのOS判定 - Qiita</h1>
		<p class="rich-link-card-description">
		はじめに  開発機で Mac と Arch Linux を使っているのでよくOS毎に設定をしたいことが多い Terminal環境は nvim + zsh + tmux を使用している  Arch では gnome-terminal...
		</p>
		<p class="rich-link-href">
		https://qiita.com/Suzuki09/items/8e36b609508238b79321
		</p>
	</div>
</a></div>


```shell
 SSH to use Linuxbrew
case ${OSTYPE} in
    linux*)
        echo "Linux OS!"
        eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"
esac
```



