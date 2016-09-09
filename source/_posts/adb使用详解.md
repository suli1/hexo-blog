---
title: adb使用详解
date: 2016-07-22 18:19:26
tags: [android, tools]
---


## 1.查看已连接的设备
```
$adb devices
```
如果存在多个设备连接，可以使用 adb -s DEVICE_ID 来指定特定的设备。


## 2.查看当前显示的activity
```
$adb shell dumpsys activity | grep mFocusedActivity
```

## 3.录屏并转换为gif
建议把size设置小一些，要不以现在手机的分辨率录屏后的文件有点大。
```
$adb shell screerecord --help
Usage: screenrecord [options] <filename>

Android screenrecord v1.2.  Records the device's display to a .mp4 file.

Options:
--size WIDTHxHEIGHT
Set the video size, e.g. "1280x720".  Default is the device's main
display resolution (if supported), 1280x720 if not.  For best results,
        use a size supported by the AVC encoder.
        --bit-rate RATE
        Set the video bit rate, in bits per second.  Value may be specified as
        bits or megabits, e.g. '4000000' is equivalent to '4M'.  Default 4Mbps.
        --bugreport
        Add additional information, such as a timestamp overlay, that is helpful
        in videos captured to illustrate bugs.
        --time-limit TIME
        Set the maximum recording time, in seconds.  Default / maximum is 180.
        --verbose
        Display interesting information on stdout.
        --help
        Show this message.

        Recording continues until Ctrl-C is hit or the time limit is reached.
                                        
```


接下来在[Ubuntn14.04下安装ffmpeg](http://www.faqforge.com/linux/how-to-install-ffmpeg-on-ubuntu-14-04/):
```
$sudo add-apt-repository ppa:mc3man/trusty-media
$sudo apt-get update
$sudo apt-get dist-upgrade
$sudo apt-get install ffmpeg
```

再使用ffmpeg转换mp4为gif
```
ffmpeg -t 10.8 -ss 00:00:01 -i input.mp4 output.gif
```
-t:转换的视频时间，单位为秒
-ss:开始时间的偏移 time_off
-i:输入文件,即视频文件

一般需要压缩一下视频图片


推荐一个非常好的video to gif的网站:http://ezgif.com/video-to-gif







