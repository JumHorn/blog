---
title: 如何使用cloudflare zero trust
tags: 技术
---

# Cloudflare Zero Trust

	利用内网穿透,暴露本地服务到对应域名。
	相比于frp,不需要自备服务器,使用cloudflare免费服务即可

## 安装
1. repo安装
```shell
apt install cloudflare
```

2. package安装

> https://github.com/cloudflare/cloudflared/releases

	去官网下载对应版本
```shell
dpkg -i cloudflared.deb
```

## token

	官网dashboard页面，点击Zero Trust
	Access页面下的Tunnels
	就可以看到自己的token
```shell
sudo cloudflared service install your_token
```
	一个主机帐户token只需要安装一个token,一个token可以创建多个tunnel为多个端口提供服务
	一个cloudflare帐户只有一个token

## tunnel

	每个主机安装好token后,可以自己创建多个tunnel,这些tunnel就是为要暴露的端口提供服务的

	点击create tunnel ==> public hostname
	添加url到不同端口的映射即可

# FAQ

* IPv6

	cloudflare默认安装是只支持IPv4,需要手动开启IPv6。
	修改配置文件/etc/systemd/system/cloudflared.service
	为启动命令加上--edge-ip-version 6
	下面是服务的配置样例
```ini
[Unit]
Description=cloudflared
After=network.target

[Service]
TimeoutStartSec=0
Type=notify
ExecStart=/usr/bin/cloudflared --no-autoupdate --edge-ip-version 6 tunnel run --token your_token
Restart=on-failure
RestartSec=5s

[Install]
WantedBy=multi-user.target
```

# Others

## cloudflare mail
## cloudflare workers