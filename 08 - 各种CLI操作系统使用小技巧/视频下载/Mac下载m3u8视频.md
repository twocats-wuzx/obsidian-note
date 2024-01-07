# 使用FFmpeg
<br/>

```bash
# 安装ffmpeg工具
brew install ffmpeg

# 找到m3u8地址
打开需要下载视频的网页，播放视频，按F12 打开开发者工具，选择网络，搜索“m3u8”

# 使用ffmpeg工具下载视频
ffmpeg -i https://domain/demo/test.m3u8 demo_m3u8.mp4

# 使用ffmpeg工具下载视频同时指定视频下载的保存路径
ffmpeg -i https://domain/demo/test.m3u8 /Users/username/Videos/test.mp4
```
