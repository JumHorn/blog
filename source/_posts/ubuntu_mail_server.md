---
title: Ubuntu上使用postfix搭建邮件服务器
tags: 技术
---

# postfix mail server on ubuntu

	考虑到之前用[sendmail](create_mail_server.md)创建的server太旧了，很怕失去维护
	所以打算用新的技术栈，为jumhorn.eu.org域名创建邮件服务器


## 安装
```shell
sudo apt install postfix
```

## 配置
```shell
sudo dpkg-reconfigure postfix
```

```shell
#使得配置生效
systemctl reload postfix
```