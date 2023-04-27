---
title: 腾讯云上安装archlinux
tags: 技术
---

# 腾讯云安装archlinux

## 准备工作

1. 开放端口

	服务器==>防火墙
	先删除默认规则，再添加规则开放所有端口

2. 确保可以vnc登录

	vnc登录要设置密码

## 下载镜像

	https://mirrors.ustc.edu.cn/archlinux/iso
	下载最新的archlinux镜像到云服务器
```shell
wget https://mirrors.ustc.edu.cn/archlinux/iso/2022.10.01/archlinux-2022.10.01-x86_64.iso
```

## 编辑grub

	编辑/boot/grub2/grub.cfg,添加如下内容(有些系统路径/boot/grub/grub.cfg)
	/dev/vda1 要用fdisk -l确定
```config
set timeout=60
menuentry 'ArchISO' --class iso {
	insmod iso9660
	set isofile=/archlinux-2022.10.01-x86_64.iso
	loopback loop0 $isofile
	#archisolabel设置archiso文件驻留的文件系统标签。
	#img_dev指明archiso文件驻留的设备
	#img_loop是archiso文件在img_dev里的绝对位置
	linux (loop0)/arch/boot/x86_64/vmlinuz-linux archisolabel=archlinux img_dev=/dev/vda1 img_loop=$isofile
	initrd (loop0)/arch/boot/x86_64/initramfs-linux.img
}
```

## 重启
	reboot重启
	网页vnc登录，查看状态
	到Reached target Graphical Interface要手动输入任意键继续

## 挂载磁盘
```shell
#重设磁盘 vda1 的读写权限
mount -o rw,remount /dev/vda1
#进入 vda1 挂载目录
cd /run/archiso/img_dev
#删除原系统文件（除了arch.iso）
rm -rf [b-z]*
#重新挂载 vda1 至 /mnt
mount /dev/vda1 /mnt
```

## 安装系统
1. 配置软件源

	编辑 /etc/pacman.d/mirrorlist
```conf
Server = http://mirrors.ustc.edu.cn/archlinux/$repo/os/$arch
```

2. 安装基础软件包
```shell
#使用 pacstrap 脚本，安装 base 软件包和 Linux 内核以及常规硬件的固件
pacstrap /mnt base linux-lts linux-firmware
#使用 pacstrap 脚本，安装常用软件
pacstrap /mnt base-devel grub openssh intel-ucode vim man dhcpcd
```

3. 配置系统
```shell
#生成 fstab 文件
genfstab -U /mnt >> /mnt/etc/fstab
#将环境变更至新系统下
arch-chroot /mnt
```

4. 配置root用户密码
```shell
passwd
```

5. GRUB配置
```shell
#生成 grub 相关文件
grub-install --target=i386-pc /dev/vda
#生成 grub.cfg
grub-mkconfig > /boot/grub/grub.cfg
```

6. 配置网络
```shell
#自动分配IP
systemctl enable dhcpcd
#开启 ssh 服务
systemctl enable sshd
```

# FAQ
1. 没有设置root密码

	[change root passwd](change_root_passwd.md)

2. ping不通arch服务器

	正常现象arch没有配置ping服务，可以使用ssh连接