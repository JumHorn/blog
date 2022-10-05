# 远程桌面连接
	VNC(Virtual Network Computing)
	协议是通用的和系统无关
	以下用mac连接centos

## 工具
	主要工具是Xvnc，在其上层的脚本有vncserver

```shell
# 安装桌面环境
sudo dnf groupinstall "Server with GUI"
dnf install tigervnc-server
```

## 配置
1. 添加vnc用户,设置密码
```shell
vncserver
```

## 使用

	mac是不用安装客户端即可使用
	finder -> go -> connect to server
```shell
vnc://ip:5901
```

## 总结

	总共消耗内存在800M，该方法可行性非常高
	远程桌面也是如此简单，其它应用只是在上面套了一层皮竟然还要收费，
	为那些开源的人表示为由衷的敬意。
	可想而知，知识付费的本质就是我知道你不知道