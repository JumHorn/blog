# 用浏览器直接下载视频和音频

	学会这项技能的学习资料不是javascript，不是chrome debug
	而是http协议
> https://developer.mozilla.org/zh-CN/docs/Web/HTTP

# 下载音频

## 使用bing translator为例子

> https://cn.bing.com/translator

1. F12打开浏览器调试界面，打开network标签页，并清空该标签页
2. firefox直接类型选择media,chrome需要all
3. 输入文本，点击试听，浏览器会播放一段音频
4. 找到对应语音的post请求
5. 目前是v1那个请求，右键copy as cURL
6. terminal中复制刚才的命令并加上--output tts.mp3
7. 完成下载即可

## 浏览器直接播放该音频
1. 在刚下载好的音频目录，启动http服务器
```shell
python3 -m http.server
```
2. 浏览器console标签页
```javascript
audio = new Audio("http://localhost:8000/tts.mp3");
audio.play();
```


# 下载视频

	目前可行的通用方法是F12后把device改成手机，那么url就是直接的地址，通过web端，则要对付blob url，比较麻烦
## 通用工具
	you-get
```shell
pip3 install you-get
```
```shell
you-get url
```

## m3u8格式
1. 使用ffmpeg下载网络视频流
```shell
ffmpeg -i 'https://url/to/some/file.m3u8' -bsf:a aac_adtstoasc -vcodec copy -c copy -crf 50 file.mp4
```


## 浏览器直接播放该视频

1. 在刚下载好的音频目录，启动http服务器
```shell
python3 -m http.server
```

2. 浏览器console标签页
```html
<html>
<title>JumHorn</title>

<body>
<video id='video' src='file.mp4' width='800' height='600'></video>
<br/>
<button onclick='video.paused ? video.play() : video.pause()'>Play/Pause<button>

<script>
  var video = document.getElementById('video');
  console.log(video.height);
</script>

</body>
</html>

```

# B站特殊案例

	B站是将音频和视频分开的，所以最终使用工具下载比较方便
	但是挡不住我的探索

## 如何下载视频
1. B站的视频是分段的，每段是m4s文件
2. m4s文件的请求大小是可以在request里面的请求头里的Range来指定的
3. 随便在浏览器的network标签页，找到一段请求，右键copy as cURL
4. 复制到终端，将请求头range范围改为 Range : bytes=0-
5. 即可下载全部视频

# ok.ru特殊案例

	这是一个俄罗斯视频网站，当然you-get并不支持，这时候稍微懂点原理就可以了，用工具就没有办法处理了

## 如何下载视频

	该站点的视频不是m3u8,m4s不能用上述方法下载
	chrome浏览器不是很方便，所以我使用了firefox浏览器

1. F12打开调试页面，跳转到network标签页
2. 文件类型限制在mp4
3. 找到对应的url，右键copy as cURL
4. 复制到终端，将请求头的url参数中的byte修改为byte=0-
5. 即可下载全部视频