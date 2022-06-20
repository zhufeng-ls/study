# SDL 问题

## No available audio device

参考网址:

http://forums.libsdl.org/viewtopic.php?t=7609&sid=40fdb9756b8e22e1b8253cda3338845f


解决方案:

```bash
apt-get
install libasound2-dev libpulse-dev
```

然后重新编译


原理：

编译时没有找到 PulseAudio 或 ALSA 的开发头，就会默认使用 /dev/dsp, 而不是支持我们想要的设备了