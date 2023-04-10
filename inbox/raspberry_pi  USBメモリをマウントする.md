---
type: note
---

#raspberry_pi 

---
2023-01-06  11:57

# raspberry_pi  USBメモリをマウントする

```
proc            /proc           proc    defaults          0       0
PARTUUID=043fc52b-01  /boot           vfat    defaults          0       2
PARTUUID=043fc52b-02  /               ext4    defaults,noatime  0       1

UUID=32460a46-2430-4c24-9c78-47b26e0f746c /home/pi ext4 defaults,noatime 0 0
UUID=0330c82a-9519-4456-ae6a-81e55cba6cfb /tmp     ext4 defaults,noatime 0 0
UUID=3c716c57-4cd7-43fc-9bc4-00afdb65221d /var/tmp ext4 defaults,noatime 0 0
UUID=f9f3f9b7-2426-4671-aec0-2a76e6938d3b /log     ext4 defaults,noatime 0 0
# a swapfile is not a swap partition, no line here
#   use  dphys-swapfile swap[on|off]  for that
```

