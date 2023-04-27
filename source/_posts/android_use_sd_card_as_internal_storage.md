---
title: Android上使用sdcard作为内置存储卡
tags: 技术
---

# android6.0 支持用sd卡作为内置存储

	由此想到了家里的小米电视4C，500M的可用存储空间只能看一下小米下载的广告，变空间不足了
	正好看到了android的新特性，便想拿小米电视做实验，没想到成功了，特别有成就感，所以想记录一下

# 工具
1. adb

	adb是[android-sdk](https://developer.android.com/studio#downloads)下的工具，各个平台安装方法相似，以下推荐简单的安装方法

	建议安装commandline-tools才100M大小对比android-studio 900M，还是命令行充满了简洁性
```shell
# debian
apt install android-tools-adb # only 3M
# mac
brew install android-sdk
```

# 开始实践
## 打开电视开发者选项
1. 连续点击build number是android的通用方法
	对于小米电视是连续点击产品型号，直到出现您已处于开发者模式即可
2. 允许adb调试
	在账号与安全页面允许adb调试

## adb连接设备
```shell
# 连接到设备
adb connect ip_of_device

# adb在本地开启了一个服务，用完之后我会主动关闭它
adb devices # 如果有设备连接，那么服务便是开启的
# 关闭adb服务
adb disconnect
```

## 设置U盘为内部存储设备
```shell
adb shell # 让android系统给出shell让你操作
sm set-force-adoptable true # 强制使用SD卡
sm list-disks # 查看U盘disk id
sm partition disk:xxx,xx private
sm list-volumes # 查看U盘的UUID
pm move-primary-storage xxxxxxxxxx-xxxx-xxxxx-xxxxx-xxxxxxxxxx
```

## 安装应用到内部存储设备

	以下用百度网盘作为例子，将应用移动到SD卡
```shell
# UUID是这样的[xxxxxxxxxx-xxxx-xxxxx-xxxxx-xxxxxxxxxx]
pm move-package com.baidu.netdisk.tv UUID

# 假如需要移动回内部存储
pm move-package com.baidu.netdisk.tv internal
```

# 问题
1. 为什么要移动应用到SD卡

	其实root是可以设置系统默认安装到SD卡，但是现在的android root太麻烦了，以后也会更麻烦，厂商有毒
```shell
pm set-install-location [0/auto] [1/internal] [2/external]
pm get-install-location
```

2. adb删除内置应用

	非常简单了，找到包名，直接删除就可以了
	什么小米电视商城，小米应用商店都可以删除
```shell
pm list packages -f # 查看包名
pm uninstall --user 0 com.baidu.netdisk.tv # 删除对应应用
```

3. 用adb直接安装应用

	用adb安装应用太方便了
```shell
# 前提是adb已经connected
adb install name.apk
adb -f install name.apk # 安装到内部存储
adb -s install name.apk # 安装到sd卡，未root的设备，只能先安装到内部存储，再移动到sd卡
adb -r install name.apk # 覆盖旧版本apk
```

4. 如何知道sm/pm如此好用的命令还有什么功能

	我也是直接sm/pm --help等它报错就能看到介绍了

5. df命令帮了我很多忙

	可以看到磁盘分区容量和剩余，太方便了
