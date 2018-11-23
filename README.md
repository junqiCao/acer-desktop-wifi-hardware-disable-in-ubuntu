# acer-desktop-wifi-hardware-disable-on-ubuntu-system
wifi hardware disable asus acer 貌似都有这个毛病，还有ubuntu等也有，既然都能显示，hardware disable即硬件显示关闭，硬件强制关闭，就是说，无线网卡没有坏，虽然曾经一度怀疑无线网卡坏了，折腾了好久，又是百度，google bing等搜索无果，重装系统等等，强迫症患者  突然今天，偶然发现  https://blog.csdn.net/ls12165/article/details/68189943 和我的电脑型号一样，遇到的问题也一样，下面说一下解决办法。

解决办法：
** 1、首先打开硬件开关  **
** 2、冲突模块加入blacklist **

硬件开关是否开启，acer4750G是fn+f3 打开后但还是显示无法打开等

rfkill list 显示两个模块，但笔记本只有一个，所以猜测这两个模块可能有冲突

lsmod | grep acer 发现系统启动了 acer——wmi模块

解决方法禁用acer-wmi模块

在  /etc/modprobe.d/ 中新增blacklist.conf 内容为 blacklist acer_wmi


两个模块冲突，将其中一个模块加入黑名单就行le blacklist

华硕笔记本就有此问题，通过修改asus-nb-wmi模块的参数解决的。

存在的问题：

linux发行版为fedora 25，电脑为acer4750G，系统装好后开机wifi模块处于关闭状态，需要手动按Fn+F3才能开启。


问题分析：

首先利用rfkill命令查看目前无线传输设备的状态，rfkill 是一个命令行工具，您可使用它查询和更改系统中启用了RFKill的设备。

rfkill的常用方法为:

rfkill list all: 获得设备列表

rfkill block [index|type]: 通过索引或类型禁用设备

rfkill unblock [index|type]:通过索引或类型启用设备

当前设备状态如下：

×××@localhost ~]$ rfkill list all
0: phy0: Wireless LAN
Soft blocked: no
Hard blocked: yes
1: acer-wireless: Wireless LAN
Soft blocked: no
Hard blocked: no

通过rfkill启用/禁用设备无法对硬件阻塞产生作用。

由于笔记本只有一个无线网卡，这两个模块应该是有冲突

使用命令lsmod | grep acer可以看出系统启动了acer_wmi模块

通过查找资料后，解决方法为禁用acer_wmi模块。

解决方法：

通过在/etc/modprobe.d/文件夹下，新增blacklist.conf文件，内容为blacklist acer_wmi。

重启后wifi自动为打开状态，此时rfkill list all命令结果如下:

[***@localhost ~]$ rfkill list all
0: phy0: Wireless LAN
Soft blocked: no
Hard blocked: no
