---
title: java高级编程技术
date: 2016-10-14 19:28:35
tags: [java, programming]
---

# JVM的工作原理简介
> [JVM的工作原理,层次结构以及GC工作原理](https://segmentfault.com/a/1190000002579346)  
> [Java Garbage Collection Introduction](http://javapapers.com/java/java-garbage-collection-introduction/)  
JVM = 类加载器ClassLoader + 执行引擎execution engine + 运行时数据区域runtime data area, ClassLoader把硬盘上的class文件加载到JVM中的运行时数据区域,但是它不负责这个类文件是否执行,而这个是执行引擎负责



# ClassLoader
> [Android动态加载基础ClassLoader工作机制](https://segmentfault.com/a/1190000004062880)  
> [热修复入门:Android中的ClassLoader](http://jaeger.itscoder.com/android/2016/08/27/android-classloader.html)
classloader有两种装载class的时机:
1.隐式:运行过程中,碰到new方式生成对象时,隐式调用ClassLoader到JVM
2.显示:通过class.forname()动态加载

采用双亲委派模型(Parent Delegation Model)来加载类  
1.当前ClassLoader首先从自己已经加载的类中查询,在缓存中则直接返回原来已经加载的类  
2.当前ClassLoader中没有则委托父类加载器去加载,父类加载器采用同样的策略,先查看自己的缓存,然后再委托父类的父类去加载,一直到bootstrap ClassLoader  
3.当所有的父类加载器都没有加载时,再由当前的类加载器加载,并放入缓存

好处:
1.安全性,避免用户自己编写的类动态替换Java的一些核心类,如String  
2.避免重复加载

* 注意每个类加载器都有自己的命名空间,在不同的命名空间中可能出现类的完整名字相同的两个类

在Android中的类加载器:
1.BootClassLoader(加载Frameword层级的类)  
2.PathClassLoader(应用程序启动时创建,加载已经安装过的apk中的类)  
3.DexClassLoader(可以加载jar/apk/dex,可以从SD卡中加载未安装的apk)  


# Annotation

# 反射

# 依赖注入

# 动态代理



