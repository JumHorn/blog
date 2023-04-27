---
title: P2P原理研究
tags: 技术
---

# p2p

    对于两台公网机器p2p十分容易实现，如今内网都会经过nat，解决用UDP实现p2p
    用工具或代码实现

## 工具
## 配置
## 使用

# FAQ
## tcp raw_socket

    对于已经握手成功的tcp连接，假如存在第三者冒出服务器发送数据给客户端是否能成功,
    目前实验没有成功，还需要跟踪数据包
1. 模拟环境
```shell
nc -lvp 1234 # server
# when client connected it will show the ip and port of client

nc 127.0.0.1 1234 # client
```

2. 使用hping3发送恶意改造的数据包
```shell
echo "malicious" | sudo hping3 -c 1 -p client_port -s 1234 -A 127.0.0.1
```

## tcp劫持
## tcp重建
## DNS欺诈
## 创建自定义DNS服务器