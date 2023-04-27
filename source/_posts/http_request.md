---
title: 如何使用不同方式发送http请求
tags: 技术
---

# http请求

    发送http请求,通过命令行或者代码方法
    > https://curlconverter.com


# cURL
```shell
curl -H "Content-Type:text/html" www.google.com # head
curl www.google.com?cmd="ls -al" # get
curl --data "param1=value1&param2=value2" www.google.com # post
```

# javascript fetch
```javascript
//head
fetch('http://www.google.com', {
    headers: {
        'Content-Type': 'text/html'
    }
});

// get
fetch('http://www.google.com?cmd=ls -al');

// post
fetch('http://www.google.com', {
    method: 'POST',
    headers: {
        'Content-Type': 'application/x-www-form-urlencoded'
    },
    body: 'param1'
});
```

# python request

    通过python request库发送http请求
    将cURL参数转为request代码

```python
# head
import requests

headers = {
    'content-type': 'text/html',
}

response = requests.get('http://www.google.com', headers=headers)
response = requests.post('http://www.google.com', headers=headers)

# get
response = requests.get('http://www.google.com')

# post
data = 'param1'
response = requests.post('http://www.google.com', headers=headers, data=data)
```

# chrome

    get: 直接在输入框内输入请求即可
    post: F12打开console用javascript
