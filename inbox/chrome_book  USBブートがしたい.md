---
type: note
---

#chrome_book  #pop_os

---
2022-05-06  21:57

# chrome_book  USBブートがしたい

MrChromebox.tech でサポートしてるデバイスにこの chrome_book があった。

                                                                                    RW_LEGACY   UEFI Firmware
https://mrchromebox.tech/#devices

![[Pasted image 20220506215718.png]]

RW_LEGACY Firmware と UEFI Firmware (Full ROM) ともにチェックが入ってる。

SCREW とは何だろ WP Method とあるが。


<div class="rich-link-card-container"><a class="rich-link-card" href="https://eizone.info/chromebook-boot-from-usb/" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://eizone.info/wp-content/uploads/Google-Chrome-icon.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Chromebook でブータブルUSB を起動する方法</h1>
		<p class="rich-link-card-description">
		起動用USB を利用可能にするため  ChromeOS の起動モードをデベロッパーモードに変更し、ファームウェアのインストール / 更新、レガシーブートモードの有効化など、Chromebook で ブータブルUSB を起動する方法 を紹介。
		</p>
		<p class="rich-link-href">
		https://eizone.info/chromebook-boot-from-usb/
		</p>
	</div>
</a></div>

## chromebook でブータブルUSBを起動する
developer モードは作ってあるので、
![[Pasted image 20220506232151.png | 300]]

この時に Ctrl + D でログインする。

Chrome OS で Ctrl + Alt + T で Crosh を開く。

shell を打ち込む。

```shell
crosh> shell
```

これを打ち込んだ。

```shell
$ cd; curl -LO mrchromebox.tech/firmware-util.sh
$ sudo install -Dt /usr/local/bin -m 755 firmware-util.sh
$ sudo firmware-util.sh
```

![[Pasted image 20220506234444.png | 300]]

これ出た!!

RW_LEGACY Firmware を install したいので、1 でリターン。
Default to booting from USB? で y でリターン。
成功した。リターンで menu --> R でリターン。
再起動。

これで 例の OS の確認機能はオフになっています画面で、Ctrl + L

すると、SeaBIOSが起動するので、ESC で ブート USB を選べる。

ここで、Pop!_OSのインストーラをセットして、SSD に Pop!_OS をインストールした!!!!!!!!!!!!!!!!


Ctrl + L を一々入れなくてもいいように出来るようだ。

### SeaBIOS を使い続ける
デフォルトで SeaBIOS を起動するように Coreboot を設定するとのこと。

https://wiki.archlinux.jp/index.php/Chrome_OS_%E3%83%87%E3%83%90%E3%82%A4%E3%82%B9

ChromeOS に入って、Ctrl + Alt + F2(→)　でコンソールに入る。

```
$ sudo su -
# flashrom --wp-disable #書き込み保護の無効化

# flashrom --wp-status  #確認

# /usr/share/vboot/bin/set_gbb_flags.sh

VB2_GBB_FLAG_DEV_SCREEN_SHORT_DELAY      0x00000001
VB2_GBB_FLAG_LOAD_OPTION_ROMS            0x00000002
VB2_GBB_FLAG_ENABLE_ALTERNATE_OS         0x00000004
VB2_GBB_FLAG_FORCE_DEV_SWITCH_ON         0x00000008
VB2_GBB_FLAG_FORCE_DEV_BOOT_USB          0x00000010
VB2_GBB_FLAG_DISABLE_FW_ROLLBAK_CHECK    0x00000020
VB2_GBB_FLAG_ENTER_TRIGGERS_TONORM       0x00000040
VB2_GBB_FLAG_FORCE_DEV_BOOT_ALTFW        0x00000080
VB2_GBB_FLAG_RUNNING_FAFT                0x00000100
VB2_GBB_FLAG_DISABLE_EC_SOFTWARE_SYNC    0x00000200
VB2_GBB_FLAG_DEFAULT_DEV_BOOT_ALTFW      0x00000400
VB2_GBB_FLAG_DISABLE_AUXFW_SOFTWARE_SYNC 0x00000800
VB2_GBB_FLAG_DISABLE_LID_SHUTDOWN        0x00001000
VB2_GBB_FLAG_FORCE_MANUAL_RECOVERY       0x00004000
VB2_GBB_FLAG_DISABLE_FWMP                0x00008000
VB2_GBB_FLAG_ENABLE_UDC                  0x00010000
```

ここで必要なフラグは
```
VB2_GBB_FLAG_DEV_SCREEN_SHORT_DELAY   0x00000001
VB2_GBB_FLAG_FORCE_DEV_SWITCH_ON      0x00000008
VB2_GBB_FLAG_FORCE_DEV_BOOT_ALTFW     0x00000080
VB2_GBB_FLAG_DEFAULT_DEV_BOOT_ALTFW   0x00000400
```

<div class="rich-link-card-container"><a class="rich-link-card" href="https://johnlewis.ie/how-to-make-the-legacy-seabios-firmware-slot-the-default-on-a-haswell-broadwell-based-chromebook/" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://johnlewis.ie/favicon.ico')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">How to make the legacy SeaBIOS firmware slot the default on a Haswell/Broadwell based Chromebook</h1>
		<p class="rich-link-card-description">
		
		</p>
		<p class="rich-link-href">
		https://johnlewis.ie/how-to-make-the-legacy-seabios-firmware-slot-the-default-on-a-haswell-broadwell-based-chromebook/
		</p>
	</div>
</a></div>

これをORして 0x489
これを書き込む。

```
# /usr/share/vboot/bin/set_gbb_flags.sh 0x489 
```
どうも失敗したようだ。

https://chromium.googlesource.com/chromiumos/docs/+/master/write_protection.md

Hardware write protection Status をチェックする。

```
# crossystem | grep wpsw
wpsw_cur　= 1 # [RO/int] Firmware write protect hardware switch current position
```
これが Write Protect Screw ってやつか。

## ネジ外す 

```shell
$ crossystem
...
wpsw_cur = 1  # Firmware write protect hardware switch current position
```
ハードウェアスイッチが入りっぱなしってことらしい。

<div class="rich-link-card-container"><a class="rich-link-card" href="https://www.unixlegion.com/2015/12/asus-c300-with-coreboot-and-u-boot.html" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('//dl.unixlegion.com/i/PG0eNYZZ6F0.jpg')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">ASUS C300 with Coreboot and U-boot</h1>
		<p class="rich-link-card-description">
		Overview of the best and most popular tech blogs covering web apps, tech businesses, web technology trends, tech startups and more.
		</p>
		<p class="rich-link-href">
		https://www.unixlegion.com/2015/12/asus-c300-with-coreboot-and-u-boot.html
		</p>
	</div>
</a></div>
![[IMG_0711.jpg | 300]]

ASUS C200 のここに Screw が隠されてた。

![[IMG_0712.jpg | 300]]

あった!!! ここだったのか !!!
ネジを外した。

```
# crossystem
...
wpsw_cur = 0


# flashrom --wp-disable #書き込み保護の無効化
SUCCESS

# flashrom --wp-status  #確認
WP: status: 0x0000
WP: status.srp0: 0
WP: status.srp1: 0
WP: write protect is disabled.
WP: write protect range: start=0x00000000, len=0x00000000
```
オール解除。

もう一度フラグを書き込む。
```
# /usr/share/vboot/bin/set_gbb_flags.sh 0x489 
SUCCESS
```

元に戻す?
書き込み保護を有効にするだけでいいや。

```
# flashrom --wp-enable

```

これで、スイッチ入れたら

![[Pasted image 20220506232151.png | 300]]

これが見えてもほっとけば SeaBIOS に入って POP!_OSになるようになった。
