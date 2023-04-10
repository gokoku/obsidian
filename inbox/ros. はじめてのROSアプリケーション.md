---
type: note
---

#ros #unity

---
2022-08-22  09:53

# ros. はじめてのROSアプリケーション

## ROS1

Mac は Swap memory を 4G にしないと動かない模様。
```shell
$ docker run -p 6080:80 --shm-size=1024m tiryoh/ros-desktop-vnc:noetic
```

![[Pasted image 20220822100002.png]]

http://localhost:6080/

![[Pasted image 20220822100151.png | 300]]

LXTerminal を起動する。

```shell
$ rosversion -d
noetic


$ roscore
...
started core service
```

もう一つのターミナルでデモを起動する。

```shell
$ rosrun turtlesim turtlesim_node
```

![[Pasted image 20220822100703.png | 300]]

更にもう一つのターミナルでコントロールする。

```shell
$ rosrun turtlesim turtle_teleop_key

```
## ROS2
```shell
$  docker run -p 6080:80 --shm-size=1024m tiryoh/ros2-desktop-vnc:galactic
```


http://127.0.0.1:6080

```shell
$ ros2 run turtlesim turtlesim_node
```

```shell
$ ros2 run turtlesim turtle_teleop_key
```


