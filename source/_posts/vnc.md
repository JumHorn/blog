---
title: Linux搭建VNC，以及使用novnc
tags: 技术
---

# 远程桌面连接
	VNC(Virtual Network Computing)
	协议是通用的和系统无关
	以下用mac连接centos

## 工具
	主要工具是Xvnc，在其上层的脚本有vncserver

```shell
# CentOS
# 安装桌面环境
dnf groupinstall "Server with GUI"
dnf install tigervnc-server
# Ubuntu
apt install xserver-xorg-core
apt install tigervnc-standalone-server tigervnc-xorg-extension
# Arch
pacman -S tigervnc
```

## 配置
1. 添加vnc用户,设置密码
```shell
# 设置密码
vncpasswd
# 启动vnc服务
vncserver  # vncserver :0(set display to :0)
# ubuntu默认localhost无法从外网访问解决方法
vncserver -localhost no
```
2. 允许使用剪切板

	编辑$home/admin/.vnc/xstartup
```shell
vncconfig -nowin &
autocutsel -fork
```

3. 默认启动不同linux桌面

	编辑$home/admin/.vnc/xstartup
```shell
# 启动默认桌面
/etc/X11/xinit/xinitrc
# 启动Xfce
startxfce4
# 启动lxde
startlxde
# 启动gnome
dbus-launch --exit-with-session gnome-session &
```

## 使用

* Mac客户端连接

	mac是不用安装客户端即可使用
	finder -> go -> connect to server
```shell
vnc://ip:5901
```

* 服务器关闭桌面环境
```shell
vncserver -kill :0
# or close all
vncserver -kill :*
```

## 总结

	总共消耗内存在800M，该方法可行性非常高
	远程桌面也是如此简单，其它应用只是在上面套了一层皮竟然还要收费，
	为那些开源的人表示为由衷的敬意。
	可想而知，知识付费的本质就是我知道你不知道

## 扩展

1. VNC client web application

	vnc协议的使用需要同时配置客户端和服务端,如果转为http协议就可以使用浏览器
	[novnc](https://github.com/novnc/noVNC)项目出现了

# FAQ
1. vncserver修改端口

	其实vncserver是脚本，打开脚本文件直接修改即可
```shell
# 查找5900，直接修改
$vncPort = 55900 + $displayNumber;
my $rfb_port = 55900 + $n;
```

2. 不支持公网IP访问

	可以使用ssh端口映射，将远程端口映射到本地
```shell
# ssh -L localport:remoteip:remoteport user@host
ssh -L 5901:127.0.0.1:5900 user@hostname
```
