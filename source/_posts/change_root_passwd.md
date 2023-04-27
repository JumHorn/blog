---
title: 忘记root密码如何在grub界面修改
tags: 技术
---

# 修改系统root密码

	此方法适用于忘记linux系统root密码时，进入到grub界面修改root密码

## 进入grub界面
1. 选择当前操作系统，按下e键
2. 找到linux开头的行
```shell
linux /boot/vmlinuz-linux root-UUID=d474f2-e6a2-4cc3-9899-aa98af13  rw  quiet
```
	在后面添加init=/bin/bash后变成
```shell
linux /boot/vmlinuz-linux root-UUID=d474f2-e6a2-4cc3-9899-aa98af13  rw  quiet init=/bin/bash
```
3. 按下ctrl+x

	进入单用户模式

4. 挂载文件系统
```shell
mount -n -o remount,rw /
```

5. 修改密码
```shell
passwd
```

6. 启动到正常模式
```shell
exec /sbin/init
```