# 创建邮件服务器

	用最简单的方式创建邮件服务器
	以centos8为例子

## 修改域名服务器记录值
1. 登录至域名管理界面
2. 修改MX记录值,将记录值设置为mail server IP

	也可以设置为该服务器的domain name

3. A记录

	A记录设置www.jumhorn.com和jumhorn.com同时能解析到，要添加以下两条A记录
```txt
@ 记录值 your IP
\* 记录值 your IP
```
4. TXT记录

	必须设置该记录，来防止SPF反垃圾邮件过滤
```txt
v=spf1 ip4:101.35.162.40 -all
```

5. 使用nslookup查看记录修改
```shell
# 查看A记录,这两条是不一样的
nslookup www.jumhorn.com
nslookup jumhorn.com
# 查看TXT
nslookup -q=txt jumhorn.com
# 查看MX
nslookup -q=mx jumhorn.com
# 查看NS
nslookup -q=ns jumhorn.com
# 反向DNS查询
nslookup ip
```

## 修改主机sendmail.mc配置
1. 安装工具
```shell
dnf install m4
dnf install sendmail
dnf install sendmail-cf
```

2. 修改sendmail.mc配置

	修改/etc/mail/sendmail.mc文件,修改对应位置文件
```mc
define(`SMART_HOST', `jumhorn.com')dnl
DAEMON_OPTIONS(`Port=smtp,Addr=0.0.0.0, Name=MTA')dnl
MASQUERADE_AS(`jumhorn.com')dnl
MASQUERADE_DOMAIN(jumhorn.com)dnl
```
	取消以下几行注释
```mc
define(`confAUTH_REALM', `mail')dnl
TRUST_AUTH_MECH(`EXTERNAL DIGEST-MD5 CRAM-MD5 LOGIN PLAIN')dnl
define(`confAUTH_MECHANISMS', `EXTERNAL GSSAPI DIGEST-MD5 CRAM-MD5 LOGIN PLAIN')dnl
```

3. 重新生成sendmail.cf文件
```shell
m4 /etc/mail/sendmail.mc  >  /etc/mail/sendmail.cf
```

5. 修改host文件
```ini
# 执行命令 hostname即可得到本机host
127.0.0.1 HOSTNAME jumhorn.com
```

6. 重启sendmail服务
```shell
systemctl restart sendmail
```

## 使用sendmail发邮件
	原始方法
```shell
# send
sendmail
# to
To: recipient@whatever.com
# from
From: you@mail.com
# set subject
Subject: hello
# set content type default plain text mode
Content-Type: text/html

# email body
... body start here

# end edit and sent it
CTRL+D
```

## 管理邮件
	mail命令更加上层，更为方便查阅和发送邮件
1. 发送邮件
```shell
# end with <ctrl + D>
mail -s "subject" someone@example.com
# or
mail -s "subject" someone@example.com <<< "email body"
# email body from file
mail -s "subject" someone@example.com < emailbody.txt
```
2. 查阅邮件
```shell
mail
# press number to read mail
& 7
# back to list
& z
# quit
& q
```

# FAQ
* 配置邮件尝试过于着急

	DNS更新需要时间，在配置更改之后要先用nslookup查询到修改了，再测试

* 为什么发送的邮件都是在垃圾邮件中

	PTR反向解析，从IP解析到域名
	目前腾讯云支持该功能，收费1500/年，好坑