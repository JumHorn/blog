---
title: Linux上使用Android模拟器
tags: 技术
---


# 安装android模拟器

	在centos上安装anbox
> https://github.com/anbox/anbox

## 工具
1. snap
```shell
# install snap
dnf install snapd

# make snap use proxy to download faster
snap install snap-store-proxy
snap install snap-store-proxy-client

# start snapd server
systemctl enable --now snapd.socket
# enable classic snap support
ln -s /var/lib/snapd/snap /snap
```
2. anbox
```shell
snap install anbox
```

## 配置

## 使用

## FAQ