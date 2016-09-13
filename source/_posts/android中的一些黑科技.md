---
title: android中的一些黑科技.md
date: 2016-09-02 18:27:02
tags: [android, programming]
---


Android中偶尔会用到的一些黑科技


# app保活(传说中的永生不死?)
> [Android保活手段](https://github.com/D-clock/AndroidDaemonService)

1.启动前台Service

2.利用系统漏洞启动前台Service

3.不同的app进程利用广播相互唤醒(包括利用系统提供的广播进行唤醒)



# 判断app处于前台还是后台
一共收集了6种方法来判断app是处于前台还是后台
> 参考[AndroidProcess](https://github.com/wenmingvs/AndroidProcess)

1.RunningTask, 5.0以上不行
2.RunningProcess,
3.ActivityLifecycleCallbacks
4.UsageStatsManager
5.通过Android无障碍功能实现
6.通过读取/proc目录下的信息





