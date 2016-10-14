---
title: android的高质量文章分类与汇总(不定期更新)
date: 2016-09-11 16:48:29
updated: 2016-09-12 16:48:29
tags: [android]
---

# 一些很好的技术网站和博客
[github](https://github.com)
[android developer](https://developer.android.com)
[stackoverflow](http://stackoverflow.com/)
[java papers](http://javapapers.com/):包括Java,Android,设计模式,Java Web相关技术

[grepcode](http:///grepcode.com):一个很好的搜索查看源码的网站,包括JDK,android
[android-arsenal](https://android-arsenal.com/):android工具，库，开源app的汇总
[Android开源项目汇总](https://github.com/Trinea/android-open-project):国内最强的开源项目收集
[codeKK](http://www.codekk.com/open-source-project-analysis):开源项目源码分析的网站

[android weekly](http://androidweekly.net/):每周一必读的android开发周报
[干货集中营](http://gank.io):国内的android，iOS等技术日报，一周到周五每天都有

其他的一些高质量社区,里面的技术比较全面
[diycode](http://www.diycode.cc/)
[掘金](http://gold.xitu.io/):有app的




# Java
[Java工程师的成神之路](http://www.hollischuang.com/archives/489)
隔一段时间可以按这个路数撸一撸，查漏补缺...


# android
[Best Android Gists](https://github.com/lopspower/BestAndroidGists)

# 自定义控件

# 性能优化

1.[Android内存申请与分析](https://mp.weixin.qq.com/s?__biz=MzAwNDY1ODY2OQ==&mid=2649286327&idx=1&sn=b69513e3dfd1de848daefe03ab6719c2&scene=4&pass_ticket=CpVkZ1EyY%2B3%2FLTSXZbC%2ByzZDD5396zM3H8N9WbYy2Bst1n%2BpvTN02%2FI5388nSwbm)
> 来自WeMobileDev的文章  

Android Studio中有一个自带的功能Allocation Trakcer可以获取一段android系统中一端时间的内存申请信息,但会收集到系统这段时间内所有的内存申请情况，对于过滤到本应用的内存申请情况比较麻烦

目标:通过分析Android Studio和Davilk的源码实现在项目代码中直接发起Allocation Tracker来分析应用的内存申请情况  
效果: 暂时只有MX3 4.4.4系统的flyme os可以正常操作，仅支持Dalvik模式  
demo: https://github.com/ragnraok/AllocRecordDemo

# 工具的使用
1.[git教程](https://www.gitbook.com/book/lvwzhen/git-tutorial)
深入浅出的git教程,基本1,2个小时看完,并照着例子敲一下就OK了,你应该知道的git的几个重要概念,如工作区(Working Directory), 版本库(Reponsitory),暂存区(stage),master,HEAD,stash, 以及常用的提交,版本回退,分支管理,标签管理的相关操作.

2.[MAT-Memory Analyzer Tool使用进阶](http://www.lightskystreet.com/2015/09/01/mat_usage/)


