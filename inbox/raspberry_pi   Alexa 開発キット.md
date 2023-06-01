#raspberry_pi #alexa 

---
2021-06-01

https://dream-soft.mydns.jp/blog/developper/smarthome/2020/05/958/
これちょっと見てみる。

# Amazon に登録する

https://developer.amazon.com/login.html

Amazon アカウントで登録した。
このアカウントはechoに紐付いている。

プロフィールを入力して、submit すると、

![[Pasted image 20210601113400.png]]

ダッシュボード
![[Pasted image 20210601114059.png]]
 Alexa Voice Service を選択する。
 
製品をクリック
![[Pasted image 20210601114346.png]]

新しい商品を追加で、
- 製品名 : RaspberryAlexa
- 製品ID : RaspberryAlexa
- 製品タイプ : Alexa内蔵の端末
- コンパニオンを使用 : いいえ
- 商品カテゴリ : スマートホーム
- 製品概要 : Raspberry pi に搭載する Alexa
- やり取り : ハンズフリー
- 配信予定 : いいえ
- Alexa for Business : いいえ
- AWS IoT : いいえ
- 子供向け : いいえ

次へ

-　セキュリティプロファイル : 新規作成
-　セキュリティプロファイル名 : RaspberryAlexaセキュリティプロファイル
-　セキュリティプロファイル記述 : RaspberryAlexaセキュリティプロファイル

次へ

- セキュリティプロファイルID : amzn1.application.7500d7c892e64b01b590b65e557f3616
- プラットフォーム情報 : 他のデバイスやプラットフォーム
- クライアントID名 : RaspberryAlexa
- 一般IDクリック
- ダウンロードクリック --> config.json
- 同意 -->完了

![[Pasted image 20210601115831.png]]

![[Pasted image 20210601115857.png]]

このプロファイルを有効化
 [https://developer.amazon.com/lwa/sp/overview.html](https://developer.amazon.com/lwa/sp/overview.html)
 
 セキュリティプロファイルを選択。
 ![[Pasted image 20210601120013.png]]
 
URL は適当でいいらしい。
![[Pasted image 20210601120128.png]]
 
 ![[Pasted image 20210601120207.png]]

これで終わりらしい。

# Raspberry pi のセッティング

### マイクデバイスの確認

```shell
$ cat /proc/asound/modules
 snd_usb_audio  があればOK
 
 $ arecord -l
 デバイス名 [USB PnP Audio Device]
 カード 2
 デバイス 0
```

### 入出力デバイスの選択

![[Pasted image 20210601161909.png]]

### 録音テスト

```shell
$ arecord -D plughw:2,0 test.wav

$ aplay test.wav
```

### AVS Device SDK on Raspberry pi

https://developer.amazon.com/ja-JP/docs/alexa/avs-device-sdk/raspberry-pi.html

#### Set up the Raspberry pi environment

```shell
$ cd /home/pi/
$ mkdir sdk-folder
$ cd cdk-folder
$ mkdir sdk-build sdk-source third-party sdk-install db


$ sudo apt update
$  sudo apt-get -y install \
git gcc cmake build-essential libsqlite3-dev libcurl4-openssl-dev libfaad-dev 
\ libssl-dev libsoup2.4-dev libgcrypt20-dev libgstreamer-plugins-bad1.0-dev 
\ gstreamer1.0-plugins-good libasound2-dev doxygen
```

```shell
$ cd /home/pi/sdk-folder/third-party

 $ wget -c http://www.portaudio.com/archives/pa_stable_v190600_20161030.tgz

$ tar zxf pa_stable_v190600_20161030.tgz

$ cd portaudio
$ ./configure --without-jack
$ make
```

```shell
$ cd /home/pi/sdk-folder/sdk-source
$ git clone --single-branch git://github.com/alexa/avs-device-sdk.git
```

#### Sensory licensing agreement

```shell
$ cd /home/pi/sdk-folder/third-patry/
$ git clone git://github.com/Sensory/alexa-rpi.git


$ cd /home/pi/sdk-folder/third-party/alexa-rpi/bin/
$ ./license.sh
space...
yes
```

#### Build the SDK

```shell
$ cd /home/pi/sdk-folder/sdk-build
$ cmake /home/pi/sdk-folder/sdk-source/avs-device-sdk \
 -DSENSORY_KEY_WORD_DETECTOR=ON \
 -DSENSORY_KEY_WORD_DETECTOR_LIB_PATH=/home/pi/sdk-folder/third-party/alexa-rpi/lib/libsnsr.a \
 -DSENSORY_KEY_WORD_DETECTOR_INCLUDE_DIR=/home/pi/sdk-folder/third-party/alexa-rpi/include \
 -DGSTREAMER_MEDIA_PLAYER=ON \
 -DPORTAUDIO=ON \
 -DPORTAUDIO_LIB_PATH=/home/pi/sdk-folder/third-party/portaudio/lib/.libs/libportaudio.a \
 -DPORTAUDIO_INCLUDE_DIR=/home/pi/sdk-folder/third-party/portaudio/include


$ make SampleApp
```

#### Set up the configuration file

登録したときのダウンロードした config.json を SDK フォルダーに移す。

```shell
host$ scp config.json pi@people.local:/home/pi/sdk-folder/sdk-source/avs-device-sdk/tools/Install/
```

```shell
$ cd ~/sdk-folder/sdk-source/avs-device-sdk/tools/Install

$  bash genConfig.sh config.json 68060 \
 /home/pi/sdk-folder/db \
 /home/pi/sdk-folder/sdk-source/avs-device-sdk \
 /home/pi/sdk-folder/sdk-build/Integration/AlexaClientSDKConfig.json \
 -DSDK_CONFIG_MANUFACTURER_NAME="RaspberryAlexa" \
 -DSDK_CONFIG_DEVICE_DESCRIPTION="RaspberryAlexa"
```
こうしてみた。

AlexaClientSDKConfig.json が生成。

~/sdk-folder/sdk-build/Integration/AlexaClientSDKConfig.json を編集。
てっぺんにこんなふうに追加しろとのこと。

```json
{
    "gstreamerMediaPlayer": {
       "audioSink": "alsasink"
    },
    "cblAuthDelegate":{
```
まんまだけど、いいのか?

#### Set up the microphone

~/.asoundrc を作って書き込むって、書き込んであった。
おそらく、GUI のメニューで設定したやつだと思うので、このままにする。
```json
File Edit Options Buffers Tools Help
pcm.!default {
  type asym
  playback.pcm {
    type plug
    slave.pcm "output"
  }
  capture.pcm {
    type plug
    slave.pcm "input"
  }
}

pcm.output {
  type hw
  card 0
}

pcm.input {
  type hw
  card 2
}

ctl.!default {
  type hw
  card 0
}
```

マイクの録音テスト。sox でやるのか。
```shell
$ sudo apt install sox

$ rec test.wav
Input File     : 'default' (pulseaudio)
Channels       : 2
Sample Rate    : 48000
Precision      : 16-bit
Sample Encoding: 16-bit Signed Integer PCM

In:0.00% 00:00:20.48 [00:00:00.00] Out:979k  [      |      ]        Clip:0

$ aplay test.wav
録れてる
```

#### run and authorize the sample app

```shell
$ cd ~/sdk-folder/sdk-build/
$ PA_ALSA_PLUGHW=2 ./SampleApp/src/SampleApp ./Integration/AlexaClientSDKConfig.json DEBUG9

##################################
#       NOT YET AUTHORIZED       #
##################################

################################################################################################
#       To authorize, browse to: 'https://amazon.com/us/code' and enter the code: E6NP7Y       #
################################################################################################
```

なので、https://amazon.com/us/code に行って、code を入力する。

いろいろ allow したら、

```shell

###########################
#       Authorized!       #
###########################

#####################################
#       Client not connected!       #
#####################################

#######################################################
#       Successfully registered 1 endpoint(s).        #
#######################################################

########################################
#       Alexa is currently idle!       #
########################################
```

これで Raspberry pi から Alexa が利用出来るようになったらしい。
このまま、c を入力する。

```shell
+----------------------------------------------------------------------------+
|                          Setting Options:                                  |
|  Press '1' followed by Enter to see language options.                      |
|  Press '2' followed by Enter to see Do Not Disturb options.                |
|  Press '3' followed by Enter to see wake word confirmation options.        |
|  Press '4' followed by Enter to see speech confirmation options.           |
|  Press '5' followed by Enter to see time zone options.                     |
|  Press '6' followed by Enter to see the network options.                   |
|  Press '7' followed by Enter to see the Alarm Volume Ramp options.         |
|  Press 'q' followed by Enter to exit Settings Options.                     |
+----------------------------------------------------------------------------+
```
メニュー出た。

1 を入力。
```shell
+----------------------------------------------------------------------------+
|                          Language Options:
|
| Press '1' followed by Enter to change the locale to de-DE
| Press '2' followed by Enter to change the locale to en-AU
| Press '3' followed by Enter to change the locale to en-CA
| Press '4' followed by Enter to change the locale to en-GB
| Press '5' followed by Enter to change the locale to en-IN
| Press '6' followed by Enter to change the locale to en-US
| Press '7' followed by Enter to change the locale to es-ES
| Press '8' followed by Enter to change the locale to es-MX
| Press '9' followed by Enter to change the locale to es-US
| Press '10' followed by Enter to change the locale to fr-CA
| Press '11' followed by Enter to change the locale to fr-FR
| Press '12' followed by Enter to change the locale to hi-IN
| Press '13' followed by Enter to change the locale to it-IT
| Press '14' followed by Enter to change the locale to ja-JP
| Press '15' followed by Enter to change the locale to pt-BR
| Press '16' followed by Enter to change the locale combinations to ["en-CA","fr-CA"]
| Press '17' followed by Enter to change the locale combinations to ["en-IN","hi-IN"]
| Press '18' followed by Enter to change the locale combinations to ["en-US","es-US"]
| Press '19' followed by Enter to change the locale combinations to ["es-US","en-US"]
| Press '20' followed by Enter to change the locale combinations to ["fr-CA","en-CA"]
| Press '21' followed by Enter to change the locale combinations to ["hi-IN","en-IN"]
| Press '0' followed by Enter to quit.


14
###################################
#       Locale is ["en-US"]       #
###################################

###################################
#       Locale is ["ja-JP"]       #
###################################
```
14 で日本語にする。

#### 応答音の設定

```shell
c
+----------------------------------------------------------------------------+
|                          Setting Options:                                  |
|  Press '1' followed by Enter to see language options.                      |
|  Press '2' followed by Enter to see Do Not Disturb options.                |
|  Press '3' followed by Enter to see wake word confirmation options.        |
|  Press '4' followed by Enter to see speech confirmation options.           |
|  Press '5' followed by Enter to see time zone options.                     |
|  Press '6' followed by Enter to see the network options.                   |
|  Press '7' followed by Enter to see the Alarm Volume Ramp options.         |
|  Press 'q' followed by Enter to exit Settings Options.                     |
+----------------------------------------------------------------------------+

3
+----------------------------------------------------------------------------+
|                 Wake Word Confirmation Configuration:                      |
|                                                                            |
| Press 'E' followed by Enter to enable this configuration.                  |
| Press 'D' followed by Enter to disable this configuration.                 |
| Press 'q' followed by Enter to quit this configuration menu.               |
+----------------------------------------------------------------------------+
E
##############################################
#       WakeWordConfirmation is "NONE"       #
##############################################

##############################################
#       WakeWordConfirmation is "TONE"       #
##############################################
```
これで、ポーンと鳴るらしい。

#### タイムゾーンの設定

https://alexa.amazon.co.jp/

![[Pasted image 20210601181727.png]]

Alexa のダッシュボードだ。

![[Pasted image 20210601181840.png]]

RaspberryAlexa をクリックすると、このデバイスの設定が出来る。
![[Pasted image 20210601182007.png]]

```shell
###############################################
#       TimeZone is "America/Vancouver"       #
###############################################

########################################
#       TimeZone is "Asia/Tokyo"       #
########################################
```

反映された!!

このまま t を押すと Listening になって、返してくれる。
```shell
t
############################
#       Listening...       #
############################

###########################
#       Thinking...       #
###########################

###########################
#       Speaking...       #
###########################

##############################################################################
#     RenderTemplateCard
#-----------------------------------------------------------------------------
# Focus State         : FOREGROUND
# Template Type       : BodyTemplate2
# Main Title          : こんにちは
##############################################################################
```


ウェイクアップワード付きで話しかけてみよう。

https://dream-soft.mydns.jp/blog/developper/smarthome/2020/05/958/

「Alexa」 が効かない。

この記事の人と同じかな?
まあ、ここまでにしとく。

t + rt で聞かれれは答えてくれる。1回っきり。




