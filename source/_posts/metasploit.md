---
title: 如何使用metasploit
tags: 技术
---

# 网络安全之metasploit

	各种漏洞测试的工具

# metasploit安装

这下不能直接命令安装了，搜索[官网](https://docs.metasploit.com/docs/using-metasploit/getting-started/nightly-installers.html)

```shell
curl https://raw.githubusercontent.com/rapid7/metasploit-omnibus/master/config/templates/metasploit-framework-wrappers/msfupdate.erb > msfinstall && \
  chmod 755 msfinstall && \
  ./msfinstall
```

# 开始使用
	以暴力破解ssh为例，开始演示
```shell
msfconsole
use auxiliary/scanner/ssh/ssh_login #使用这个模块
msf auxiliary(ssh_login) > set RHOSTS 10.0.0.27 # 设置remote host
msf auxiliary(ssh_login) > set USERPASS_FILE root_userpass.txt # 设置密码文件
msf auxiliary(ssh_login) > run # 开始测试
```

# 其它常用命令
1. help

	显示帮助命令

2. search

	查找漏洞模块,使用查找后use auxiliary/scanner/ssh/ssh_login，等价于下面的搜索
```shell
search ssh_login
use 0
```

3. options
	查看模块可以设置哪些选项
	主要设置required为yes的没有current setting的选项，即设置默认为空的选项