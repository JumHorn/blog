---
title: 自建KMS服务器激活Windows
tags: 技术
---

# kms

	微软windows/office激活工具(Key Management Services)
	微软连接https://learn.microsoft.com/en-us/windows-server/get-started/kms-create-host
	这里是要创建linux版的激活服务器
	项目基于https://github.com/Wind4/vlmcsd

## 工具

	直接从github.com/Wind4/vlmcsd下载可执行文件
	如需编译，自行编译
```shell
wget https://github.com/Wind4/vlmcsd/releases/download/svn1113/binaries.tar.gz
```

## 配置

	启动服务即可
```shell
# create a server listening on 1688
./vlmcsd
```

## 使用

1. Windows激活

	在需要激活的windows上执行如下命令(带管理员的cmd)
```shell
# 输入激活密钥
slmgr.vbs -ipk VDYBN-27WPP-V4HQT-9VMD4-VMK7H
# 连接kms服务器
slmgr.vbs -skms ip_of_kms_server
slmgr.vbs -ato
# 查看激活状态
slmgr.vbs -dlv
```

2. Office激活

	查找ospp.vbs文件，以office16为例子
```shell
cd \Program Files\Microsoft Office\Office16
cscript ospp.vbs /sethst:ip_of_kms_server
cscript ospp.vbs /act
```
## FAQ

* 激活密钥从哪来

	激活密钥由微软官方提供
	https://learn.microsoft.com/en-us/windows-server/get-started/kms-client-activation-keys

## 总结

	很简单的一件事情，以前也总是在网络上搜索一键激活，被流氓软件骚扰。
	其实东西早已开源，而我们从一开始就被定义为电脑小白。定义别人为小白，然后流氓自己为所欲为。
	但凡有人稍微指引一下，也可以少走很多弯路。现在看来流氓软件自身也不过是小白。