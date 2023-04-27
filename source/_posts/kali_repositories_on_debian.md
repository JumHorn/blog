---
title: 如何在debian机器上使用kali源
tags: 技术
---

# 在debian系统上添加kali源

	突然想要用到kali,但是不想安装.但是kali是基于debian的，想到区别应该不大，把源加上就行了

## 添加kali源

	编辑/etc/apt/sources.list
```config
# debian
deb http://mirrors.ustc.edu.cn/debian/ bullseye main non-free contrib
deb-src http://mirrors.ustc.edu.cn/debian/ bullseye main non-free contrib
# kali
deb http://mirrors.ustc.edu.cn/kali/ kali-rolling main non-free contrib
```

## 添加kali官方密钥
```shell
wget -q -O - https://archive.kali.org/archive-key.asc | apt-key add
```

## 更新源
```shell
apt update
```

# 奇葩想法
1. 在debian上安装pacman