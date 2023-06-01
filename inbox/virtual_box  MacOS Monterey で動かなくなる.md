#virtual_box 

---
2021-11-19



## MacOS Monterey で動かなくなる

インストールできるのだが、再起動後に kali が立ち上がらなくなる。

![[Pasted image 20211119103726.png]]

https://www.eripyon.com/mt/2021/11/imac_monterey_virtualbox_windows10_not_working.html

```shell
sudo kextload -b org.virtualbox.kext.VBoxDrv  
sudo kextload -b org.virtualbox.kext.VBoxNetFlt  
sudo kextload -b org.virtualbox.kext.VBoxNetAdp  
sudo kextload -b org.virtualbox.kext.VBoxUSB
```

https://forums.virtualbox.org/viewtopic.php?f=39&t=104272

### brew で入れて shell Script で動かすと上手く行った

```shell
$ brew install --cask virtualbox
```

~/bin/vbox

```shell
#!/bin/bash

sudo kextload -b org.virtualbox.kext.VBoxDrv  
sudo kextload -b org.virtualbox.kext.VBoxNetFlt  
sudo kextload -b org.virtualbox.kext.VBoxNetAdp  
sudo kextload -b org.virtualbox.kext.VBoxUSB

VirtualBox
```

