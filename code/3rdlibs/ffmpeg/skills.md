# 技巧

## 指定字体
drawtext=fontfile=arial.ttf:text=test

## 录制屏幕
```
ffmpeg -f x11grab -s 1920x1080 -i :0.0 output.mp4
```

## 录制摄像头
```
ffmpeg -i /dev/video0 output.mkv
```

### 录制音频
```
ffmpeg -f alsa -i default output.mp3
```