---
title: 无处不在的linux云服务
tags: 信息
---

# cloud server

## [IBM LinuxONE](https://linuxone.cloud.marist.edu)

	IBM教育免费服务器，支持免费用120天

## [intel](https://notebooks.edge.devcloud.intel.com/)

	jupter 10小时使用时长，250G内存，CPU豪华级配置
	devcloud 16G 支持使用120天

## [woiden](https://woiden.id/)

	云主机使用telegram注册，可以使用7天，可以持续续期.
	共享公网IP的NAT VPS，可以使用反向代理
	[登录IPv6](https://ssh.hax.co.id/)

## [alwaysdata](https://www.alwaysdata.com)

	100M免费空间,可以使用ssh登录,创建定时任务

# cloud shell
	分享各种提供linux shell的云服务

## [xshellz](https://www.xshellz.com)

	提供的shell可以连续运行15天

## [google cloud shell](https://cloud.google.com/shell)

	有5G免费空间，可以用tmux一直保持运行,网页版本身就是tmux,每周50小时限制

## [github codespaces](https://github.com/codespaces)

	有8G内存,32G空间,写代码专用,30分钟无活动自动关闭

## [github actions](github_action.md)

	可以创建定时任务

## [red hat openshift](https://console.redhat.com)

	30天免费试用,等待90天后可以再次使用
	功能太多，目前找到sandbox ide也非常好用
	https://developers.redhat.com/developer-sandbox
    sandbox中的oc(openshift client)命令本质是kubernetes
    用来练习和使用kubernetes也是绝好的,有30G内存

## [replit](https://replit.com/)

	执行程序，部署联网应用

# online IDE
	在线编辑器往往都提供直接的编译运行环境，都是可以直接拿到shell

## [gitpod](https://www.gitpod.io)

	github codespaces替代网站

## [goormide](https://ide.goorm.io)

	联网shell,10GB空间

## [blinder](https://mybinder.org/)

	无需注册，直接launch github链接

## [glitch](https://glitch.com)

	1000 hours/month,6G内存

## [Theia](https://theia-ide.org/)

	eclipse che编辑器直接使用vscode插件

## [cloud studio](https://cloudstudio.net/)

	来自腾讯

# web hosting

	可以通过建站站点权限漏洞获取系统shell

## php建站系统漏洞获取

[phpshell](webshells_php.md)

## nodejs，python建站系统漏洞获取

	可以直接部署wetty,webssh网页即可完成

## 可以测试的网站

[web hosting](webhosting.md)

# others

	其他能获取shell的服务

## [OnWorks](https://www.onworks.net/)

	提供linux图形化机器，直接使用

## [Play with Docker](https://labs.play-with-docker.com/)

	4小时使用时间，可以部署docker容器

## [Microsoft](https://learn.microsoft.com/en-us/training/modules/implement-common-integration-features-finance-ops/10-exercise-1)

	2小时使用时间

## [AppOnFly](https://www.apponfly.com/)

	20分钟windows使用时间

# 内网穿透

	以上拿到shell的机器，通常不具备公网IP,使用内网穿透服务
	可以很好的规避这个问题

## [ngrok](https://ngrok.com/)

	提供域名，将内网应用部署到外网
	比起frp不需要自己建立服务器

## [Cloudflare](https://www.cloudflare.com/)

	cloudflare tunnel内网穿透,有自己的域名，该域名可以直接用作内网服务
	对于http，客户端不需要下载cloudflare
	对于tcp，客户端和服务器都要下载cldouflare
