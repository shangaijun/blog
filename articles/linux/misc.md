- 1.如果ssh public key放置到指定地方，但是仍然无法使用ssh private key登陆，需要检查下服务器端的 .ssh 目录的属主和权限是否正确
- 2.如何知道一个服务器是虚拟机还是物理机呢？
```
[root@xxx ~]# dmidecode -s system-manufacturer
Red Hat
[root@xxx ~]# dmesg |grep -i virtual
Booting paravirtualized kernel on Xen
input: Macintosh mouse button emulation as /devices/virtual/input/input0
Initialising Xen virtual ethernet driver.
```

