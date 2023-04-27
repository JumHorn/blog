---
title: 一条命令设置系统代理
tags: 技术
---

# 设置系统代理的方法

## shell

	由于shell往往是login的，表示一个用户，故shell设置的代理只能在当前shell中使用
	通用的shell代理设置方法

1. 设置shell代理
```shell
export all_proxy="socks://127.0.0.1:1080" # 设置全部代理
export http_proxy="http://127.0.0.1:1080" # 代理http
export https_proxy="https://127.0.0.1:1080" # 代理https
```

2. 关闭shell代理
```shell
unset all_proxy
unset http_proxy
unset https_proxy
```

## linux gnome代理设置
1. 设置shell代理
```shell
gsettings set org.gnome.system.proxy.socks host 'localhost'
gsettings set org.gnome.system.proxy.socks port 1080
```

2. 关闭shell代理
```shell
gsettings set org.gnome.system.proxy mode 'none'
```

## Mac 桌面代理设置
1. 设置Mac桌面代理
```shell
# 查找所使用的网络(wifi or ethernet)
networksetup -listallnetworkservices
# 设置系统socks代理，“Wi-Fi”表示上个命令中找到的网络设备
networksetup -setsocksfirewallproxy "Wi-Fi" localhost 1080
```
2. 关闭Mac桌面代理
```shell
networksetup -setsocksfirewallproxystate "Ethernet" off
```

## 软件代理设置
1. wget
	编辑$HOME/.wgetrc文件，并加入如下代理
```config
use_proxy=no
http_proxy=127.0.0.1:7890
https_proxy=127.0.0.1:7890
```

2. pip
	编辑$HOME/.pip/pip.conf,并加入如下代理
```shel
[global]
index-url = https://pypi.mirrors.ustc.edu.cn/simple
```