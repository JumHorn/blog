---
title: 建立webssh服务
tags: 技术
---

# webssh

	通过网页http协议ssh登录获取终端
	最终效果通过域名shell.jumhorn.eu.org访问网页ssh客户端

## 安装
```pip
pip3 install webssh
```

## 配置

	使用nginx反向代理
```conf
server {
	listen 80;
	server_name shell.jumhorn.eu.org;

	location / {
		proxy_pass  http://127.0.0.1:8888;
		proxy_http_version 1.1;
		proxy_read_timeout 300;
		proxy_set_header Upgrade $http_upgrade;
		proxy_set_header Connection "upgrade";
		proxy_set_header Host $http_host;
		proxy_set_header X-Real-IP $remote_addr;
		proxy_set_header X-Real-PORT $remote_port;
	}
}
```

## 使用
```shell
wssh --fbidhttp=False --xsrf=False --origin="*" --debug --xheaders=False --address="localhost"
```

## 相关应用
* [gotty](https://github.com/yudai/gotty)
```shell
gotty bash
```
* [ttyd](https://github.com/tsl0922/ttyd)
```shell
ttyd bash
```
