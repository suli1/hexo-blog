---
title: android的高质量文章分类与汇总(不定期更新)
date: 2016-09-11 16:48:29
updated: 2016-09-12 16:48:29
tags:
---

# 自定义控件

# 性能优化

1.[Android内存申请与分析](https://mp.weixin.qq.com/s?__biz=MzAwNDY1ODY2OQ==&mid=2649286327&idx=1&sn=b69513e3dfd1de848daefe03ab6719c2&scene=4&pass_ticket=CpVkZ1EyY%2B3%2FLTSXZbC%2ByzZDD5396zM3H8N9WbYy2Bst1n%2BpvTN02%2FI5388nSwbm)
> 来自WeMobileDev的文章  

Android Studio中有一个自带的功能Allocation Trakcer可以获取一段android系统中一端时间的内存申请信息,但会收集到系统这段时间内所有的内存申请情况，对于过滤到本应用的内存申请情况比较麻烦

目标:通过分析Android Studio和Davilk的源码实现在项目代码中直接发起Allocation Tracker来分析应用的内存申请情况  
效果: 暂时只有MX3 4.4.4系统的flyme os可以正常操作，仅支持Dalvik模式  
demo: https://github.com/ragnraok/AllocRecordDemo


