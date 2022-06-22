#raspberry_pi #node-red 

---
2021-06-14

# Node-RED のブート時の自動起動

サービスで登録してある。

/lib/systemd/system/nodered.service

```shell
$ sudo systemctl daemon-reload

$ node-red-stop
$ node-red-start

$ node-red-status


$ sudo systemctl status .service
$ sudo systemctl enable nodered.service
$ sudo systemctl disable nodered.service
```



