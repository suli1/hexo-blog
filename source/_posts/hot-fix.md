---
title: Android热修复框架研究与分析
date: 2016-09-25 11:05:02
tags:
---

> [微信Tinker](http://mp.weixin.qq.com/s?__biz=MzAwNDY1ODY2OQ==&mid=2649286384&idx=1&sn=f1aff31d6a567674759be476bcd12549&scene=0#rd)


## [Android apk增量更新](http://gold.xitu.io/post/57fba92abf22ec00649de645)
实验环境Ubuntu14.04
流程:
1.在手机用户上提取当前安装应用的apk  
2.使用bsdiff生成old.apk和new.apk的增量文件  
3.使用bspatch将增量文件与1中的old.apk合并生成新apk, MD5值与new.apk相同, 然后安装  

步骤:
1.下载bsdiff,编译安装, 出现的错误及解决方案  
a)
```
$make
Makefile:13: *** missing separator.  Stop.
```
将Makefile文件中的.ifndef和.endif前面加上一个缩进

b)
```
bsdiff.c:33:19: fatal error: bzlib.h: No such file or directory
```
使用apt-file,如果没有apt-file,则使用apt-get install apt-file安装
```
$apt-file search bzlib.h
libbz2-dev: /usr/include/bzlib.h
libghc-bzlib-doc: /usr/lib/ghc-doc/haddock/bzlib-0.5.0.4/bzlib.haddock
$sudo apt-get install libbz2-dev
```

c)
```
/tmp/ccLcGItc.o: In function `main':
bsdiff.c:(.text.startup+0x2aa): undefined reference to `BZ2_bzWriteOpen'
.........
collect2: error: ld returned 1 exit status
make: *** [bsdiff] Error 1
```
修改Makefile文件,参考[android增量更新](http://www.jianshu.com/p/104560997986)








