---
title: 简单的CGI尝试
tags: 技术
---

# CGI(common gateway interface)

    CGI是服务器的标准,也就是任意具有输入输出的编程语言都可以轻易编写CGI程序

## python http server

    通过python的http模块作为server

```shell
# 启动服务器
python3 -m http.server --cgi
```
    以下细节要注意
    CGI脚本一定要放在cgi-bin目录下
    CGI脚本一定要有执行权限

### 标准CGI脚本
1. bash脚本CGI
```shell
#!/bin/bash

echo "Content-type: text/html"
echo "" # 输出换行是http协议格式必须的2个换行

echo "<html><body>Hello CGI</body></html>"
```

2. python脚本CGI
```python
#!/bin/python
print("Content-type: text/html")
print()
print("<html><body>Hello CGI</body></html>")
```

3. C语言CGI
```C
#include<stdio.h>
int main()
{
    printf("Content-type: text/html\n");
    printf("\n");
    printf("<html><body>Hello CGI</body></html>");
}
```

## 实际验证
1. 将以上CGI程序的可执行文件命名为echo
2. 为echo文件添加执行权限
```shell
chmod +x echo
```
3. 将echo文件放入cgi-bin目录下
4. 浏览器验证
```shell
curl ip:port/cgi-bin/echo
```

## nginx server

    尚未验证,需要fcgiwrap,因为nginx支持fastCGI