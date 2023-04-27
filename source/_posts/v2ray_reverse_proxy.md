---
title: 用v2ray实现反向代理
tags: 技术
---

# v2ray反向代理
	为了清晰说明连接方式，这里使用三台机器来说明
	A <==> B <==> C
	A是提供服务的机器，B是代理服务器，C是普通客户端

	现在由于网络限制，A只能向B发起连接，B不能主动连接A
	A ==> B <== C
	在这种情况下，C想要访问A的服务就需要用到反向代理

	实现过程，由A向B发起v2ray连接，B开通监听端口给C访问，并将C的请求转发给A
	注意B转发给A是通过，A主动连接B产生的反向连接

## A机器配置
```json
{
	"log": {
		"loglevel": "debug"
	},
	"reverse": {
		"bridges": [
			{
				"tag": "bridge",
				"domain": "localhost.com"
			}
		]
	},
	"outbounds": [
		{
			"tag": "tunnel",
			"protocol": "vmess",
			"settings": {
				"vnext": [
					{
						"address": "jumhorn.com",
						"port": 29091,
						"users": [
							{
								"id": "897244b5-0170-44a3-9f76-672fa6734c9b"
							}
						]
					}
				]
			}
		},
		{
			"tag": "direct",
			"protocol": "freedom"
		}
	],
	"routing": {
		"rules": [
			{
				"type": "field",
				"inboundTag": [
					"bridge"
				],
				"domain": [
					"full:localhost.com"
				],
				"outboundTag": "tunnel"
			},
			{
				"type": "field",
				"inboundTag": [
					"bridge"
				],
				"outboundTag": "direct"
			}
		]
	}
}
```

## B机器配置
```json
{
	"log": {
		"loglevel": "debug"
	},
	"reverse": {
		"portals": [
			{
				"tag": "portal",
				"domain": "localhost.com"
			}
		]
	},
	"inbounds": [
		{
			"tag": "interconn",
			"port": 29090,
			"protocol": "vmess",
			"settings": {
				"clients": [
					{
						"id": "897244b5-0170-44a3-9f76-672fa6734c9b"
					}
				]
			},
			"streamSettings": {
				"network": "tcp"
			},
			"sniffing": {
				"enabled": true,
				"destOverride": [
					"http",
					"tls"
				]
			}
		},
		{
			"tag": "tunnel",
			"port": 29091,
			"protocol": "vmess",
			"settings": {
				"clients": [
					{
						"id": "897244b5-0170-44a3-9f76-672fa6734c9b"
					}
				]
			},
			"streamSettings": {
				"network": "tcp"
			},
			"sniffing": {
				"enabled": true,
				"destOverride": [
					"http",
					"tls"
				]
			}
		}
	],
	"routing": {
		"rules": [
			{
				"type": "field",
				"inboundTag": [
					"interconn"
				],
				"outboundTag": "portal"
			},
			{
				"type": "field",
				"inboundTag": [
					"tunnel"
				],
				"domain": [
					"full:localhost.com"
				],
				"outboundTag": "portal"
			}
		]
	}
}
```

## C机器配置
```json
{
	"log": {
		"loglevel": "debug"
	},
	"inbounds": [
		{
			"port": 1080,
			"listen": "127.0.0.1",
			"protocol": "socks",
			"settings": {
				"udp": true
			},
			"streamSettings": {
				"network": "tcp"
			},
			"sniffing": {
				"enabled": true,
				"destOverride": [
					"http",
					"tls"
				]
			}
		}
	],
	"outbounds": [
		{
			"protocol": "vmess",
			"settings": {
				"vnext": [
					{
						"address": "jumhorn.com",
						"port": 29090,
						"users": [
							{
								"id": "897244b5-0170-44a3-9f76-672fa6734c9b"
							}
						]
					}
				]
			}
		}
	]
}
```