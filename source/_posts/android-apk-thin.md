---
title: android apk 瘦身
date: 2016-11-10 14:14:25
tags:[android,apk]
---

# TinyPNG
使用TinyPNG压缩资源图片大小,可减少图片80%~90%的大小

# [去除R.class](https://github.com/mogujie/ThinRPlugin)
在编译时,将除R$styleable.class以外所有的R.class删除掉,并在引用的地方替换成对应的常量,从而达到缩小包大小和减少dex个数的效果.已使用在蘑菇街app上

> 参考资料
[PNG图片压缩对比分析](https://mp.weixin.qq.com/s?__biz=MzI1NjEwMTM4OA==&mid=2651232233&idx=1&sn=03d9858ac451f2768b804d2604a8e12e)  : QQ音乐技术团队文章,主要比较了tinypng和pngquant
[官方Reduce APk Size](https://developer.android.com/topic/performance/reduce-apk-size.html#apk-structure): 官方的文档,有对应的[翻译](https://developer.android.com/topic/performance/reduce-apk-size.html#apk-structure)

