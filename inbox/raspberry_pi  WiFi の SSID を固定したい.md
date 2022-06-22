#raspberry_pi 

---
2021-08-18

# WiFi の SSID を固定したい

/boot/wpa_supplicant.conf  を作って、初期値で掴みたいWiFi のSSID をセットするとうまくいった。

```shell

ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=JP

network={
     ssid="peopleTONO_A_own"
     psk="pmorioka7481"
     key_mgmt=WPA-PSK
}

```

