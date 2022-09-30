# 用浏览器直接下载视频和音频

	使用简单的debug知识和少量的javascript

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

## 以acfun作为例子
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