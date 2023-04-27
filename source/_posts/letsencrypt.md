---
title: 如何让网站使用https
tags: 技术
---

# Let's encrypt

	获取ssl证书，让网站安全访问

## 验证方式
1. http验证

	给你一个随机内容的文件，你把这个文件放在一个特定的位置，用待绑定的域名+特定path可以访问到这个文件，可以认为域名是你的
2. DNS验证

	给你一个随机字符串，你把这个字符串添加为待绑定域名的txt值，只要能通过公网上的DNS服务器解析到这个字串，就认为域名是你的

## 安装
```shell
pip install certbot
```

## http验证

	http验证比较简单,按照提示步骤即可
```shell
sudo certbot --nginx
```

## DNS验证

	DNS验证多了一步，要到DNS服务上添加一条text记录
```shell
certbot --text --agree-tos --email <your@email.com> -d <your.domain.com> --manual --preferred-challenges dns --expand --renew-by-default  --manual-public-ip-logging-ok certonly
```
## 服务器(nginx)

	其中fullchain.pem,privkey.pem都是上述验证命令给出的位置
```conf
server {
	listen       8880 ssl;
	server_name  chat.jumhorn.com;

	ssl_certificate      /etc/letsencrypt/live/chat.jumhorn.com/fullchain.pem;
	ssl_certificate_key  /etc/letsencrypt/live/chat.jumhorn.com/privkey.pem;

	ssl_session_cache    shared:SSL:1m;
	ssl_session_timeout  5m;

	ssl_ciphers  HIGH:!aNULL:!MD5;
	ssl_prefer_server_ciphers  on;

	location / {
		proxy_pass  http://127.0.0.1:54000/;
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

# FAQ

1. http验证简单，为什么要DNS验证

	如果vps服务器供应商屏蔽了http的80/443端口，那么http就无法验证，所以可以使用DNS验证

2. 既然80/443端口被封，那么要ssl证书有什么用

	http无论那个端口在域名没有备案时都会直接被封，因为服务器厂商能过截获http明文报文内容,
	https经过加密厂商无法知道传输内容，所以加密的重要性就不言而喻了(tls/ssl)