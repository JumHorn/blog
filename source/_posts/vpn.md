---
title: 自建VPN服务
tags: 技术
---

# 创建vpn服务器

	用PPTP/L2TP创建vpn服务
	想要使用vpn将多台公网服务器共同组建成局域网服务器

# PPTP

## 工具
```shell
dnf install pptpd
```
## 配置
1. ip配置

	编辑/etc/pptpd.conf
	localip是服务器的局域网ip
	remoteip是客户端的局域网ip
```ini
localip 192.168.0.1
remoteip 192.168.0.100-200
```

2. 用户配置

	编辑/etc/ppp/chap-secrets
	其中\*表示任意ip
```shell
# client server passwd ip
client1 pptpd 123 *
client2 pptpd 223 *
client3 pptpd 323 *
```

3. 重启pptpd服务
```shell
systemctl restart pptpd
```

4. 允许转发数据包(IP packet forwarding)
```shell
vim /etc/sysctl.conf
# add the following
net.ipv4.ip_forward = 1
# enable it
sysctl -p
```

## 使用

	直接在客户端添加vpn并连接即可,手机和电脑默认支持
	苹果系高版本已经不支持

## FAQ
1. 让客户端之间可以相互通信
	配置iptables
```shell
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE && iptables-save
iptables --table nat --append POSTROUTING --out-interface ppp0 -j MASQUERADE
iptables -I INPUT -s 10.0.0.0/8 -i ppp0 -j ACCEPT
iptables --append FORWARD --in-interface eth0 -j ACCEPT
```

# L2TP

## 工具
```shell
dnf install xl2tpd
```
## 配置
1. ip配置

	编辑/etc/xl2tpd/xl2tpd.conf
	localip是服务器的局域网ip
	remoteip是客户端的局域网ip
```ini
ip range = 192.168.1.128-192.168.1.254
local ip = 192.168.1.99
```

2. 用户配置

	编辑/etc/ppp/chap-secrets
	其中\*表示任意ip
```shell
# client server passwd ip
client1 * 123 *
client2 * 223 *
client3 * 323 *
```

3. secret配置

4. 重启xl2tpd服务
```shell
systemctl restart xl2tpd
```

5. 允许转发数据包(IP packet forwarding)
```shell
vim /etc/sysctl.conf
# add the following
net.ipv4.ip_forward = 1
# enable it
sysctl -p
```

## 使用

	同pptp

## FAQ

	使用上再次卡到ios14bug，连接不上
> https://developer.apple.com/forums/thread/660499