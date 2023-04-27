---
title: Linux上轻量桌面对比
tags: 技术
---

# 性能消耗最小的linux desktop

	使用的是只有2G内存的CentOS云服务器，尝试一下desktop

## linux系统切换桌面环境
	使用vnc连接桌面,编辑$HOME/.vnc/xstartup
1. 让vnc启动Xfce桌面环境
```shell
/etc/X11/xinit/xinitrc
```

2. 关闭vnc
```shell
vncserver -kill :1
```

## gnome

	gnome一开就占用500M空间，且不用的情况下，内存占用还会扩大，作为桌面还是可以的，
	但是作为服务器就差强人意了

## Xfce
1. 安装
```shell
dnf groupinstall "Xfce"
```
2. 启动

	编辑$HOME/.vnc/xstartup
```shell
# /etc/X11/xinit/xinitrc
startxfce4
```

3. 测评

	实际使用空间在300M，比gnome好很多了

## lxde
1. 安装
```shell
pacman -S lxde
```
2. 启动

	编辑$HOME/.vnc/xstartup
```shell
# /etc/X11/xinit/xinitrc
exec startlxde
```

3. 测评

	实际空间使用在150M左右，很好用的桌面环境，干净到啥也没有